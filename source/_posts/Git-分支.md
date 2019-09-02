---
title: Git-分支
date: 2019-09-02 15:45:17

categories:
- Git
tags:
- Git
---
![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_logo.png)

# 简介
每次提交新版本，Git 都会把新版本连接到上一个版本的后面，串成一条时间线，这个时间线就是分支。

分支管理是 Git 的强项，有别于 SVN 的分支是整个项目的一个拷贝，Git 的分支仅仅是一个指向某次提交的指针。所以 Git 创建、删除分支有天然的优势，仅仅创建、删除一个指针即可，成本极低。

<!-- more -->

# 分支操作

## 创建分支
`git branch develop` 创建一个名为 `develop` 的分支。

创建一个名为 develop 的指针指向当前最新的提交。

`git checkout -b develop` 创建并切换到 `develop` 分支。

创建一个名为 develop 的指针指向当前最新的提交，同时 HEAD 指针也会移动到 develop 分支。

HEAD 指针永远都指向当前分支最新的节点。

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_checkout_b.gif)

## 切换分支
`git checkout develop` 命令用于切换到已有分支 `develop`。

Git 将 HEAD 指针指向 develop 分支最新的提交，切换分支仅仅是移动一个指针即可。

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_checkout.gif)

## 删除分支
`git branch -D develop` 命令用于删除 `develop` 分支。

Git 只要删除 develop 指针，就完成了分支的删除。

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_checkout_d.gif)

## 其它常用命令
`git branch -a` 查看所有分支。

`git branch -a -v` 查看所有分支详细信息。

# 小结
分支的存在让并行开发成为了可能，而 Git 强大的分支管理，极大的提升了开发效率和版本迭代效率。