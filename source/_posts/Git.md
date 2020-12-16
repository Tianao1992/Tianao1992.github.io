---
title: Git
date: 2020-12-16 09:50:48
index_img: /img/ittree/gitimg/gitlogo.png
categories:
- ItTree
tags:
- Git
---

### Git介绍
Git 是一个开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。
Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。
Git 与常用的版本控制工具 CVS, Subversion 等不同，它采用了分布式版本库的方式，不必服务器端软件支持

### git安装（mac）
Mac的终端上输入git检测是否安装git
```
https://git-scm.com/downloads

```
安装完成之后查看git版本

```
git --version

```
创建一个全局用户名、全局邮箱作为配置信息
```
git config --global user.name "your_name"  
git config --global user.email "your_email@youremail.com"

```
### git 本地仓库创建

打开终端
```
$cd 某一个文件夹下
$ git init
$ git add .
$ git commit -m '初始化项目版本'
```
#### 如果远端已有仓库

使用命令  
$git remote add origin 添加远端仓库
$git push -u origin master 推送到远端仓库


### 工作区域

git本地有三个工作区域：工作目录 (working) 、暂存区(stage)、本地仓（local rep,如果添加远端git仓（Rmmote）

![](/img/ittree/gitimg/gitwork.png)

#### git 常用六个命令

- git add file: 本地文件添加到暂存区

- git commit: 提交到本地仓库

- git push: 推送到远端仓库

- git pull: 拉取远端分支内容

- git reset: 撤销提交内容

- git checkout: 撤销缓存内容

### 分支管理

切出分支命令
```
git checkout -b dev

```

创建项目时，会针对不同环境创建三个常设分支

- master: 主分支发布版本不轻易修改

- dev: 功能开发分支

- test: 测试分支，提交给测试部门

![](/img/ittree/gitimg/gitliucheng.png)


### git 撤销回滚

#### 撤销

**文件被修改了，但未执行git add操作**

-  git checkout -- fliename  某个文件名

-   git checkout -- .   将多个文件一次性撤销可以用

**已经添加到暂存区域，从暂存区撤销**

- git reset HEAD filename 撤销某个

- git reset HEAD 多个撤销

**commit之后，想要撤销到某个版本**

1. git log查看提交记录

2. git  reset --hard  commit_id

#### 回滚

**回滚一次错误的合并**

1. git branch backup 备份一个

2. git push origin backup: backup 推送到远端

3. git reset --hard xxxxx 本地仓库彻底回退到xxxxx版本，xxxxx版本之后的commit信息将丢失

4. git push origin :master 删除对应远端分支

5. git push origin master 重新创建远端分支

git revert   
**是回滚某个 commit ，不是回滚“到”某个**

如果错误的修改已经不是最新的commit，回退某次提交的上一步，用git revert。并且会生成一个新的提交来撤销某次提交，此次提交之前的commit都会被保留。

比如：a操作添加了“haha”，commit了a，b操作添加了“xixi”，commit b。现在想回滚到只添加了“haha”，需要的是删除“xixi”，也就是逆向操作b，所以应该`git revert b的commit_id`。 `git revert` 应该翻译成“反转、逆转”比较好理解，而不是回退

git reset
删除commit 之前提交的内容 