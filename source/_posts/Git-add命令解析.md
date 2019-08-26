---
title: Git-add命令解析
date: 2019-08-24 16:53:34

categories:
- Git
tags:
- Git
---
![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_logo.png)

前文有了解到，`git add fileName` 命令会将指定文件加入到暂存区，那么 `git add` 背后都做了哪些操作，下面详细介绍下。

<!-- more -->

## 1. 保存已修改的对象
在执行 `git add` 操作后，Git 首先会执行 `git hash-object` 命令把当前修改的文件压缩成二进制文件，我们称为一个 Git 对象。同时还会计算当前文件内容的哈希值，哈希值一共 40 位，前两位作为文件夹的名字，后两位作为这个 Git 对象的名字，存入 .git/objects 目录下。这个新生成的 Git 对象就是前文 [《Git-版本库目录结构解析》](http://chirenhua.com/2019/08/24/Git-%E7%89%88%E6%9C%AC%E5%BA%93%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84%E8%A7%A3%E6%9E%90/) 提到过的 blob 类型的文件。

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_objects.png)

## 2. 将修改的信息添加到暂存区
文件保存完后，还需要告诉 Git 哪些文件发生了改动，这些变动的文件会统一存放在暂存区中。

Git 通过执行 `git update-index` 命令，将修改的文件名，对应 Git 对象名，以及文件权限等信息写入到暂存区。

文件加入到暂存区后，就可以通过 `git status` 命令查看到当前修改的文件了。

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_status.png)

## 其它命令
`git add .` ：对所有已修改的文件都执行一遍 `git add` 操作。
