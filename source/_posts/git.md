---
title: Git基本操作
date: 2017-07-25 20:47
comments: true
layout: post
tags: [git]
categories: git
---



# Git add

`git add -A` 和 `git add .` 和 `git add -u`

- git add   **.** ：他会监控工作区的状态树，使用它会把工作时的**所有变化提交**到暂存区，包括文件内容修改(modified)以及新文件(new)，但不包括被删除的文件。

<!--more-->

- git add -u ：他仅监控**已经被add的文件**（即tracked file），他会将被修改的文件提交到暂存区。add -u 不会提交新文件（untracked file）。（git add --update的缩写)
- git add -A ：是上面两个功能的合集（git add --all的缩写）

自我理解:

- git add -A  提交所有变化
- git add -u  提交被修改(modified)和被删除(deleted)文件，不包括新文件(new)
- git add .  提交新文件(new)和被修改(modified)文件，不包括被删除(deleted)文件

# commit过程

- git status  检查工作区是否干净
- git add 命令主要用于把我们要提交的文件的信息添加到索引库中。当我们使用git commit时，git将依据索引库中的内容来进行文件的提交。
- git commit -am "xxxxxxx"
- git branch 
- git push origin xxxx




# 删除分支

- git branch -D xx **删除本地分支**
- git push origin :br  (origin 后面有空格) **删除远程分支**



# 解决冲突

- git fetch origin cms:new  创建新本地分支new
- git branch   
- git merge new  合并
- git branch -D new 删除本地分支

# diff

查看自上次提交以来，本地代码改动的具体情况

# Git log

在提交了若干更新之后，又或者克隆了某个项目，想回顾下提交历史，可以使用 `Git log` 命令查看

# Git show

git show <commit-hash-id>查看某次commit的修改内容

# Git reset --hard <commit>

现在让我们来重置回那次提交的状态：

# Git rebase

对于git rebase, 你亦可以选择进行交互式的rebase。这种方法通常用于在向别处推送提交之前对它们进行重写。交互式rebase提供了一个简单易用的途径让你在和别人 分享提交之前对你的提交进行分割、合并或者重排序。在把从其他开发者处拉取的提交应用到本地时，你也可以使用交互式rebase对它们进行清理。

如果你想在rebase的过程中对一部分提交进行修改，你可以在'git rebase'命令中加入'-i'或'--interactive'参数去调用交互模式。

$ git rebase -i origin/master

这个命令会执行交互式rebase操作，操作对象是那些自最后一次从origin仓库拉取或者向origin推送之后的所有提交。

若想查看一下将被rebase的提交，可以用如下的log命令：

$ git log github/master..



# git branch -a

查看远程分支

master
remotes/origin/HEAD -> origin/master
remotes/origin/Release
remotes/origin/master

# git checkout -b myRelease origin/Release

切换到 origin/Release分支，并在本地新建分支 myRelease