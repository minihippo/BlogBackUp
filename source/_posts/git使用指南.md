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
## 基本概念

![git结构](/picture/git结构.png)

<!--more-->

* 当工作区修改的文件执行`git add`命令后，暂存区的目录树会被更新，并且修改文件内容会被写入到对象库的一个新的对象中（对象的id被记录到暂存区的文件索引中）。
* 当执行`git commit `命令后，暂存区的目录树被写到版本库中，master分支做更新，它指向的目录树就是提交时暂存区的目录树
* 当执行`git reset HEAD`命令后，暂存区的目录树会被重写，替换成master分支指向的，但工作区不受影响

## 安装配置

git config —global user.name/email 配置用户主目录下的信息，用于识别用户

for mac: user.name *minihippo*
         user.email *gengxiaoyu1996@gmail.com*

* 查看配置信息

```shell
$ git config --list
```

## 创建版本库

1. ` git init`进入目录 
  ```shell
  $ git init
  Initialized empty Git repository in /Users/shawygeng/Project/blog/.git/
  ```

2. `clone`添加远程仓库*

   ~~~shell
   $ git clone [url]
   ~~~


3. `add`添加修改文件到缓存

   ```shell
   $ git add filename
   $ git add --all /.  //添加所有改动（包括删除操作）
   ```

4. `commit`提交修改到仓库

   ```shell
   $ git commit -m 'information'  //附加信息
   ```

## 远程仓库

`remote/clone `,`remote add`,`remote rm`,`push`,`fetch`,`merge`

1. ssh加密

   ```shell
   $ ssh-keygen -t rsa -C 'gengxiaoyu1996@gmail.com'
   Generating public/private rsa key pair.
   ```

2. `remote add`添加远程仓库地址

   ```shell
   $ git remote add origin https://github.com/minihippo/LeetCode.git
   $ git remote add -f ssh/http  //fetch，添加并获取最新版本到本地
   ```

3. `remote rm`删除远程仓库

   ~~~shell
   $ git remote rm origin  //主机名
   ~~~

4. `remote`查看当前远程库

   ```shell
   $ git remote
   origin
   $ git remote -v
   origin    https://github.com/minihippo/LeetCode.git (fetch)
   origin    https://github.com/minihippo/LeetCode.git (push)
   ```

5. `push`向远程库提交内容

   ```shell
   $ git push -u origin masterß
   $ git push -u origin branchname:branchname
   ```

6. 提取远程库内容

   * `fetch`从远程仓库下载新分支与数据

     ~~~shell
     $ git fetch origin  //主机名
     ~~~

   * `merge`从远程仓库提取数据并合并到当前分支

     ~~~shell
     $ git merge origin/master //将origin master分支合并到本地分支 
     ~~~

## 分支管理

`branch`,`branch -d` ,`cheeckout`,`merge`,`clean`


1. `branch`创建分支

   ```shell
   $ git branch branchname
   ```

2. `branch`删除分支

   ```shell
   $ git branch -d branchname
   ```

3. `branch`列出分支

   ```shell
   $ git branch
   ```

4. `checkout`切换分支

   ```shell
   $ git checkout branchname
   $ git checkout -b branchname  //创建新分支并切换到当前目录下
   ```

5. `merge`合并分支

   ```Shell
   $ git merge <branchname>  //合并分支branchname到当前分支
   ```

6. `clean`切换分支冲突

   当我们在一个分支下修改文件并未add，此时需要切换另一分支，就会产生冲突。
   clean命令就是用来删除未add的文件
   ~~~shell
   $ The following untracked working tree files would be overwritten by checkout
   $ git clean -d -fx
   //-n 显示将要删除的文件和目录；
   //-x -----删除忽略文件已经对git来说不识别的文件
   //-d -----删除未被添加到git的路径中的文件
   //-f -----强制运行
   ~~~

## 撤销修改

`checkout`,`reset`,`revert`,`rm`,`mv`

1. `checkout` 撤销对工作区的修改

   ~~~shell
   $ git checkout ./<filename>
   ~~~

2. `reset` 撤销对缓存区的修改，取消add。 //用master指向的目录替换缓存区目录文件
   ~~~shell
   $ git reset HEAD /file
   ~~~

3. `rm`移除缓存区内容

   ```shell
   $ git rm filename  //同时删除工作区内容
   $ git rm --cached filename  //只删除缓存区内容
   ```

4. `revert` 已经commit且**push**，撤销commit

   git revert是用一次新的commit回滚之前的commit，不会造成版本冲突问题。

   ~~~shell
   $ git revert commit标识码
   ~~~

5. `reset`已经commit且push

   本地版本回退

   ~~~shell
   $ git reset --hard <版本号>  //抛弃当前工作区修改
   $ git reset --soft          //保留工作区修改
   ~~~
   本地提交，此时本地版本落后远程版本
   ~~~shell
   $ git push origin branch --force
   ~~~

6. `mv`移动或重命名工作区文件

   ```shell
   $ git mv filename filenname
   ```

## 版本管理

`status`,`diff`

1. `status`查看缓存区状态

   ```shell
   $ git status 
   $ git status -s  //简短输出
   ```

2. `diff`查看项目缓存区状态
   ```shell
   $ git diff  //没缓存的改动
   $ git diff --stat  //摘要
   $ git diff --cached  //已缓存的改动
   $ git diff HEAD  //查看所有改动
   ```
#### 标签

`tag`,`tag -d`,`show`,`log`

1. `tag`添加标签

   ```shell
   $ git tag v1.0
   $ git tag -a v1.0  //创建一个带注解的标签
   ```

2. `tag`查看已有标签

   ```Shell
   $ git tag
   ```

3. `tag`删除标签

   ```shell
   $ git tag -d v1.0
   ```

4. `show`查看版本修改内容

   ```shell
   $ git show v1.0
   $ git log -oneline
   ```

## 子项目

`subtree`,`subtree add`,`subtree pull`,`subtree push`

1. `subtree`概念

     当一个项目里有个目录，而这个目录是包括其他独立项目。因此需要使用subtree管理同步子目录下的项目。

2. 添加子项目的远程库

   ~~~shell
   $ git remote add subpro ssh/http
   ~~~

3. `subtree add`添加子项目

   ~~~shell
   $ git subtree add --prefix=subfile_path subpro branchname
   $ git subtree add --prefix subfile_path hostname branchname --squash  //把子项目的更新记录当作一个commit合并，并且合并到主项目中
   ~~~

4. `subtree push`提交子项目更改

   ~~~shell
   $ git subtree push --prefix=subfile_path hostname branchname
   ~~~

5. `fetch,subtree pull`更新子项目

   ~~~shell
   $ git fetch hostname branchname
   $ git subtree pull --prefix=subfile_path hostname branchname [--squash]
   ~~~

## Reference

[官网手册]https://git-scm.com/docs

[国内教程]http://www.runoob.com/git/git-commit-history.html
