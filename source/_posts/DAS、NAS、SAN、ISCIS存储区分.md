---
title: DAS、NAS、SAN、ISCIS存储区分
date: 2018-06-01 15:20:27
tags:
    - 存储
    - 小记
toc: true
---

### Storage Classification
![class](/picture/DAS-NAS-class.png)

<!--more-->

(*封闭系统*——大型机)

### Overview
![overview](/picture/DAS-NAS.png)

#### DAS(Direct Attached Storage)
直接连接存储:
　　将外置存储设备通过SCSI(服务器外接设备接口)或FC接口连接到应用服务器上

紧耦合，I/O性能导致服务器瓶颈，存储不能在服务器间动态分配，数据处理复杂度高

#### NAS(Network Attached Storage)
网络附加存储:
　　将存储与服务器分离，存储系统包含简单的文件管理系统；网络交换机连接存储和服务器建立私网，应用通过网络(TCP/IP)存取数据；采用业界标准文件共享协议如：NFS、HTTP、CIFS实现共享。


数据通过普通数据网络传输，依赖网络情况。只能访问文件，不能访问数据块

#### SAN(Storage Area Network)
存储区域网:
　　通过光纤交换机连接存储阵列和服务器，建立专用数据存储的存储私网（存储采用高端RAID阵列），采用SCSI、FC-AL接口。

专用网路速度快，光纤价格昂贵

！！ NAS与SAN的去呗就是NAS有自己的FS管理

#### ISCSI(Internet SCSI)
Internet小型计算机接口：使用普通的数据网传输SCSI数据块

