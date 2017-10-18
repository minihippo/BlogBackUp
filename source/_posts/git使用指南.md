---
title: git使用指南
date: 2017-09-18 21:44:38
tags:
     - 教程
     - 备份
     - git
reward: true
toc: true
---
<p>    
### 基本概念

![git结构](/picture/git结构.png)

<!--more-->

* 当工作区修改的文件执行`git add`命令后，暂存区的目录树会被更新，并且修改文件内容会被写入到对象库的一个新的对象中（对象的id被记录到暂存区的文件索引中）。
* 当执行`git commit `命令后，暂存区的目录树被写到版本库中，master分支做更新，它指向的目录树就是提交时暂存区的目录树
* 当执行`git reset HEAD`命令后，暂存区的目录树会被重写，替换成master分支指向的，但工作区不受影响
* 当执行`git rm —cached file`命令后，直接删除暂存区的文件，工作区不受影响
* ！当执行`git checkout ./-- <file>`命令时，会用暂存区全部／指定的文件替换工作区的文件。
* ！当执行`git checkout HEAD/<file>`命令时，会将head指向分支中的全部／部分文件替换暂存区和工作区的文件

<p>
### 安装配置
git config —global user.name/email 配置用户主目录下的信息，用于识别用户
for mac: user.name *minihippo*  
         user.email *gengxiaoyu1996@gmail.com*
* 查看配置信息
```shell
$ git config --list
```
<p>
### 创建版本库
1.` git init`进入目录 
  ```Shell
  $ git init
  Initialized empty Git repository in /Users/shawygeng/Project/blog/.git/
  ```

2.`clone`克隆远程仓库*
   ~~~shell
   $ git clone [url]
   ~~~

3.`add`添加文件到缓存
   ```shell
   $ git add filename
   $ git add --all  //添加所有改动（包括删除操作）
   ```

4.`commit`提交修改到仓库
   ```shell
   $ git commit -m 'information'  //附加信息
   ```
   *\#常用操作*
5.`status`查看项目状态
   ```shell
   $ git status -s  //简短输出
   ```

6.`diff`查看项目缓存区状态
   ```shell
   $ git diff  //没缓存的改动
   $ git diff --stat  //摘要
   $ git diff --cached  //已缓存的改动
   $ git diff HEAD  //查看所有改动
   ```

7.`reset`取消已缓存内容
   ```shell
   $ git reset HEAD
   ```

8.`rm`移除缓存区内容
   ```shell
   $ git rm filename  //同时删除工作区内容
   $ git rm --cached filename  //只删除缓存区内容
   ```

9.`mv`移动或重命名文件
   ```shell
   $ git mv filename filenname
   ```
<p>
### 远程仓库
1.ssh加密
   ```shell
   $ ssh-keygen -t rsa -C 'gengxiaoyu1996@gmail.com'
   Generating public/private rsa key pair.
   ```

2.`remote add`添加远程仓库地址
   ```shell
   $ git remote add origin https://github.com/minihippo/LeetCode.git
   ```

3.`push`提交内容
   ```shell
   $ git push -u origin master  //远程主机名 分支名
   $ git push -u origin branchname:branchname
   ```

4.`remote`查看当前远程库
   ```shell
   $ git remote
   origin
   $ git remote -v
   origin	https://github.com/minihippo/LeetCode.git (fetch)
   origin	https://github.com/minihippo/LeetCode.git (push)
   ```

5.提取远程库内容
   * `fetch`从远程仓库下载新分支与数据
     ~~~shell
     $ git fetch origin  //别名
     ~~~
   * `merge`从远程仓库提取数据并合并到当前分支
     ~~~shell
     $ git merge  origin/master
     ~~~
     一般在更新分支与数据后会执行merge合并到当前分支

6.`remote rm`删除远程仓库
   ~~~shell
   $ git remote rm origin  //别名
   ~~~

<p>

### 分支管理
1.`branch`创建分支
   ```shell
   $ git branch branchname
   ```

2.`checkout`切换分支
   ```shell
   $ git checkout branchname
   $ git checkout -b branchname  //创建新分支并切换到当前目录下
   ```

3.`merge`合并分支
   ```Shell
   $ git merge <branchname>
   ```

4.`branch`列出分支
   ```shell
   $ git branch
   ```

5.`branch`删除分支
   ```shell
   $ git branch -d branchname
   ```
<p>

### 标签
1.`tag`添加标签
   ```shell
   $ git tag v1.0
   $ git tag -a v1.0  //创建一个带注解的标签
   ```

2.`tag`查看已有标签
   ```Shell
   $ git tag
   ```

3.`tag`删除标签
   ```shell
   $ git tag -d v1.0
   ```

4.`show`查看版本修改内容
   ```shell
   $ git show v1.0
   $ git log -oneline
   ```
<p>

### 子项目

1.`subtree`概念
   当一个项目里有个目录，而这个目录是包括其他项目。因此需要使用subtree同步子目录下的项目

2.`subtree add`初始化
   ~~~shell
   $ cd blog
   $ git subtree add --prefix=themes/yilia https://github.com/litten/hexo-theme-yilia.git master
   ~~~

3.`subtree push`提交子项目更改
   ~~~shell
   $ git subtree push --prefix=themes/yilia origin branchname
   ~~~

4.`subtree pull`更新子项目
   ~~~shell
   $ git subtree pull --prefix=themes/yilia origin master
   ~~~

<p>
### Reference
[官网手册]https://git-scm.com/docs
[国内教程]http://www.runoob.com/git/git-commit-history.html  

Updating . . . 
