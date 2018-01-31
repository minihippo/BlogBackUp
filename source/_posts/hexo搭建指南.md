---
title: hexo搭建指南
date: 2017-09-23 08:29:59
tags:
     - 备份
     - hexo
     - 教程
toc: true
reward: true
---

### 安装配置

*precondition*: nodejs ——生成静态页面
  
1.npm安装
   ~~~shell
   $ npm install hexo-cli -g
   ~~~
  
<!--more-->

### 建站

1.初始化
   ~~~shell
   $ hexo init <folder>
   $ cd folder
   $ npm install
   ~~~
2.文件目录
   ~~~shell
   .
   ├── _config.yml  //配置文件信息
   ├── package.json  //应用程序信息
   ├── scaffolds  //模板文件夹，当新建文章时，会按模版填充内容。可修改./post.md中的Front-matter
   ├── source  //存放用户资源，markdown和html文件会被解析到public中。以_开头的文件名会被忽略
   |   ├── _drafts  
   |   └── _posts
   └── themes //主题
   ~~~
3.配置文件`config.yml`
    
   *具体配置文件参数见手册*

     | Setting     | Description |
     | ----------- | ----------- |
     | title       |             |
     | subtitle    |             |
     | description |             |
     | author      |             |

### 部署

1.安装hexo-deployer-git，提交远程仓库
   ~~~shell
   $ npm install hexo-deployer-git --save
   ~~~
2.修改配置文件deploy部分
   ~~~shell
   deploy: 
     type: git
     repo: https://github.com/minihippo/minihippo.github.io.git
     branch: master
   ~~~
3.生成静态文件
   ~~~shell
   $ hexo generate/d
   ~~~
4.启动服务器
   ~~~shell
   $ hexo server/s         //访问http://localhost:4000/
   ~~~
5.push到github命令
   ~~~Shell
   $ hexo deploy/d
   ~~~

### 写作

1.新建文章
   ~~~shell
   $ hexo new [layout] "pagename"  //layout in scaffolds,three kinds default
   ~~~
2.`<!—more—>`显示到此处截止

3.草稿`source/_draft`
   ~~~shell
   $ hexo --draft  //显示草稿
   ~~~
4.`publish`发表草稿
   ~~~shell
   $ hexo publish [layout] <filename>  //将草稿移到_post中
   ~~~
5.Front-matter
   ~~~shell
   //用于指定个别文件的变量
   ---
   title: Hello World
   date: 2013/7/13 20:46:25
   ---
   ~~~
   -预设参数-

   | Setting    | Description     |
   | ---------- | --------------- |
   | layout     |                 |
   | title      |                 |
   | date       |                 |
   | updated    | 更新日期            |
   | comments   | 开启文章评论功能，默认true |
   | tags       |                 |
   | categories | 分类有顺序性和层次性      |
   | permalink  | 覆盖文章网址          |
6.`render`渲染文章
   
7.`list`显示网站内容
   ~~~shell
   $ hexo list <type>  //types: page, post, route, tag, category
   $ hexo list route  //显示文件夹目录
   $ hexo list post
   INFO  Start processing
   INFO  周记17.09.18 encryped
   INFO  copy crypto-js.js to public/js/
   Date        Title         Path                    Category  Tags
   2017-09-17  Hello World   _posts/hello-world.md             hexo, 杂谈
   2017-09-18  碎碎念        _posts/碎碎念.md                    杂谈
   2017-09-18  周记17.09.18  _posts/周记17-09-18.md              周记
   2017-09-18  git使用指南   _posts/git使用指南.md                教程, 备份, git
   ~~~
8.关于`README.md`
   　　一般的，readme并不需要每次渲染，并且不被归入`archieve`中。因此需要在每次渲染生成的时候跳过该文件。
   ~~~shell
   $ vi public/README.md
   //修改配置文件
   skip_render: README.md
   ~~~

### 主题

1.安装主题yilia
   ~~~shell
   $ git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia
   ~~~
2.修改根目录配置文件`_config.yml`
   ~~~shell
   theme: yilia
   ~~~
3.更新主题
   ~~~shell
   $ cd themes/yilia
   $ git pull
   ~~~
4.当根目录添加了git远程仓库，主题目录可用subtree管理
   ~~~shell
   $ cd blog
   $ git subtree add --prefix=themes/yilia http://... branchname   //添加子项目
   $ git subtree pull --prefix=themes/yilia origin master //update
   ~~~

### Plug-in

1.某篇文档加密
   * 下载encrypt组件
     ~~~shell
     $ npm install hexo-encrypt --save
     ~~~
   * 添加配置项`_config_yml`
     ~~~shell
     encrypt:
       password: xxxxxx
     ~~~
   * 文章需加密front-matter添加
     ~~~shell
     ---
     encrypt: true
     enc_pwd: xxxx
     ---
     ~~~
2.搜索组件
   * 下载组件
     ~~~shell
     $ npm install hexo-generator-json-content --save
     ~~~
   * 添加配置项`_config.yml`
     ~~~shell
     jsonContent:
         meta: false
         pages: false
         posts:
           title: true
           date: true
           path: true
           text: false
           raw: false
           content: false
           slug: false
           updated: false
           comments: false
           link: false
           permalink: false
           excerpt: false
           categories: false
           tags: true
     ~~~

### Reference

- [hexo Documentation](https://hexo.io/docs/)


