---
title: Git-版本库目录结构解析
date: 2019-08-24 16:01:58

categories:
- Git
tags:
- Git
---
![git_logo](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_logo.png)

## 版本库
在前面的文章 [《Git-创建新仓库》](http://chirenhua.com/2019/08/24/Git-%E5%88%9B%E5%BB%BA%E6%96%B0%E4%BB%93%E5%BA%93/)中我们了解到，每个仓库下面都有一个名为 .git 的隐藏文件夹，这个文件夹其实就是版本库，这里面就藏了 Git 版本管理的秘密。

<!-- more -->

## .git 目录结构
.git 的目录结构如下图：

![gitExample](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/gitExample.png)

### config
当前仓库的配置文件

### description
仓库的描述信息，一般 GitHub、GitLab 等托管平台会用到。

### HEAD
前文有提到，是 Git 本地仓库的三大组成部分之一。HEAD 文件存储了当前分支的关联。

### hooks
hooks 文件夹用来存放 Git 脚本，这些脚本一般用于规范 Git 操作。可以设定在执行某些 Git 操作时，触发相应的脚本。比如提交前检查提交信息等。

### info
存放 Git 仓库的一些信息。

### objects

存放所有的 Git 对象。

在生成 Git 对象时，会同时生成一个 40 位的哈希值，哈希值的前 2 位用作文件夹名，后 38 位用作 Git 文件名。

![git_objects](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_objects.png)

Git 对象共有三种类型：

1. blob 实际修改文件的二进制文件。
2. tree 当前的目录结构，以及对应目录结构下的 blob 文件。
3. commit 版本信息，包含 tree 文件、作者信息、上一个 commit ID 等信息。（注：commit ID 就是 commit 文件的哈希值）

这三个文件之间的关系如下：

![git_commit](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_commit.png)

### refs
存放远端以及各个分支的信息、版本信息、本地所有分支的 HEAD 指针。

refs 文件夹下有三个子文件夹：

![git_refs](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_refs.png)

heads 文件夹存放的是所有本地分支的 HEAD 指针，指针中存放的是对应分支最新的 commit ID。

remotes 文件夹存放了所有远端的信息，包括每个远端下分支的信息。

tags 文件夹存放了所有版本的信息。

### logs
存放了所有的提交历史和操作历史。