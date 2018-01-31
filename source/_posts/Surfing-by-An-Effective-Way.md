---
title: Surfing by An Effective Way
date: 2017-10-18 13:57:40
tags:
     - shadowsocks
     - GFW
     - Mac OS
     - Centos 7
     - Linux
     - ios
     - ssh
     - 备份
     - vpn
reward: true
toc: true
---

### 关于ShadowSocks

　　它是一个基于SOCKS5协议的协议。实现原理：

~~~shell
client <--> ss-local <--[加密]--> ss-remote <--> target
~~~

<!--more-->

ss-local执行类似SOCKS5服务端的操作，给client提供代理。它加密来自client的数据并且转发给remote。remote再解密转发给目标服务器。服务器返回的信息再通过热remote加密转回去。

- socks5的地址结构：[1-byte type]|[variable-length host]|[2-byte port] 。ipv6／4／hostname
- TCP连接：[target address]|[payload]，ss-local创建一个和remote的tcp连接，头部是目标地址和数据，这个数据流是加密的。remote接收并解密数据流，分析头部目标地址，然后创建一个到目标地址新的tcp连接，并且转发数据给它。连接直到ss-local断开。
- 加密方式：一个AEAD加密的tcp流是根据每个session的subkey生成一个随机的salt，后面跟着许多的chunk。每个chunk的结构是[encrypted payload length]|[length tag]|[encrypted payload]|[payload tag]，一部分是payload长度加密，一部分是payload加密。

#### 现阶段维护版本

1. 服务器端
   - shadowsocks: 原始python版本
   - shadowsocks-libev: C版本轻量级支持多种嵌入式设备和终端
   - shadowsocks-go: 商用，支持多端口，多密码。
   - go-shadowsocks2: 注重核心特性和复用性的商用版本
  
2. 客户端
   - shadowsocks-android: Android
   - shadowsocks-windows: Windows
   - shadowsocksX-NG: MacOS
   - shadowsocks-qt5: Cross-platform Windows/MacOS/Linux.

### CentOS 7 搭建ss

#### 安装ss

　　ss已经打包发布到Python的第三方库上了，因此可以直接下载使用。GUI客户端[Shadowsocks-Qt5](https://github.com/shadowsocks/shadowsocks-qt5/wiki/Installation)。

1. 首先要有python环境，安装pip，python的包管理工具

   ~~~shell
   $ sudo yum install python-pip
   ~~~

2. 下载ss

   ~~~Shell
   $ sudo pip install shadowsocks
   ~~~

3. 编辑配置文件

   ~~~shell
   $ sudo vi /etc/shadowsocks.json
   {
     "server":"服务器ip地址／主机名"
     "server_port":"服务器端口号"
     "local_port":"本地端口号"
     "password":"密码"
     "timeout":"时限"
     "method":"加密方式" 
   }
   ~~~

4. 运行ss

   ~~~shell
   $ ss-local -c /etc/shadowsocks.json
   ~~~

#### 浏览器代理

　　由于流量都是走本地端口号才能出去，而浏览器的默认端口并不是本地端口，因此需要给浏览器设置代理。

- Google Chrome

  安装插件SwitchyOmega，现在貌似升级到了SwitchSharp。
  设置socks 5 代理，地址为本机地址“localhost”，端口为本地端口。

- Firefox

  不需要安装插件，直接设置就好。
  配置方式如上，但需勾上远程DNS

### Mac OS Sierra 搭建ss

　　GUI 客户端维护在github上，[ShadowsocksX-NG](https://github.com/shadowsocks/ShadowsocksX-NG)。当然也可以按照centos的方式通过pip安装。前者浏览器访问不需要插件。后者并没有在mac上尝试。当然还可以直接通过mac包管理命令brew安装shadowsocks-libev。

　　配置如上，还是按照服务器端配置。

### ios 10 安装ss

　　官方维护的app是Wingy。笔者通过pp助手下载了shadowrocket app，配置如上。

### 关于GFW

　　介绍各个阶段GFW的手段吧。

1. DNS污染

   　　网址转换成ip需要DNS服务器去解析，GFW要求国内运营商DNS服务器拦截某些网址，然后群众的策略是把DNS服务器地址改成google的8.8.8.8。因为DNS协议是无安全较验的协议。所以当用户访问了GFW想拦截的域名，那么它会返回一个伪造的ip，而后来的google服务器返回真实的ip包会被丢弃。
   　　最简单有效的方法。

2. ip封锁

   　　针对1问题，群众的解决方案是改hosts，加入google的ip和域名，自行建立映射。GFW直接就进行ip封锁，无法ping通。

3. 封锁端口

   　　针对2问题，群众的解决方案是建立ssh连接、openvpn。ssh协议是有特征的，它使用22号端口通信。因此GFW可以识别，封锁服务器端口访问。这样切换端口还是能上，GFW就再封一直到封了ip。但是考虑商业使用，GFW是针对ssh有个判定机制，超过了流量、链接上限就封锁。

   - 关于ssh协议

     它是建立客户端与服务器间通信的加密协议。主要分为SSH-TRANS、SSH-AUTH、SSH-CONN三部分。SSH-TRANS定义了传输的包和加密通道，后二者都是建立在前者协议的基础上。客户端接受服务器的公钥，使用公钥加密。服务器接收数据后使用私钥解密。

4. 关检词封锁

   　　由于http协议是明文传输的，因此传输内容是可见的。GFW在你的交换机上设置旁路镜像，拆http包，检查是否有关键字，有就断掉TCP连接。

5. 针对https协议进行随机中断

   　　因此很多网站都使用了https协议，然后GFW就会对这些连接进行随机中断。

6. VPN

   　　vpn虚拟专用网络它主要是为了分散网络用户接入局域网，进行加密通讯。它是一个商用的远程连接基础协议。由于它的工作层在OSI中的最下面第二层链路层，所以它的性能超级好，而且可靠性高，免设置。但是vpn特征明显，固定端口通信，固定连接所以也hen容易被发现。GFW考虑商用的问题所以不太会被墙。

7. 随机森林

   　　最近vpn也不行了，群众的解决方式是ss，搭建vps。因为ss数据是加密的，属于样本少，数据不可获得的特点，所以GFW使用随机森林进行判断。1万个样本能80%以上的概率拦截ss。

8. 其他被淘汰解决方式

   　　google goAgent、找第三方服务器解析DNS，本地DNS服务。

### Refernece

- [shadowsocks官网](http://shadowsocks.org/en/download/clients.html)


