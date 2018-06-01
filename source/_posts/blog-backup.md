---
title: blog backup
date: 2018-05-31 22:17:45
tags:
	- 备份
	- hexo
    - git	
---

### Motivation
-　针对blog本地文件迁移的问题，需要对本地文件进行备份。
-　引用主题需要同步更新

<!--more-->

#### BackUp
　　git版本管理

~~~shell
$ git remote add  origin git@github.com:minihippo/BlogBackUp.git
~~~

修改`.gitignore`, 增加
~~~shell
/.deploy_git    # 防止和hexo自带的deploy冲突
/public       
/_config.yml
~~~

#### 主题管理
　　git子项目管理

~~~shell
$ git subtree add --prefix=themes/yilia path
 # 更新主题
$ git subtree pull --prefix=themes/yilia origin master
~~~


年纪大了，容易忘。。。
