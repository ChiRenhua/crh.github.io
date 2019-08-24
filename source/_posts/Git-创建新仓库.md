---
title: Git-创建新仓库
date: 2019-08-24 11:16:42

categories:
- Git
tags:
- Git
---
![git_logo](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_logo.png)

## 简介
仓库是 Git 中的一个概念，实际上就是一个被 Git 标记的本地文件目录，目录中文件的增、删、改都能被 Git 捕捉到。

Git 创建新仓库有两种方式，一种是本地初始化一个新仓库，另外一种是从服务器上 clone 一个仓库到本地。

## 本地初始化仓库
类似于把大象放冰箱分为三步，本地创建一个 Git 仓库只需要两步：

1. 创建一个本地文件夹。
2. 在当前文件夹目录下执行 `git init`。

以下图 GitExample 文件夹为例，执行完 `git init` 后，会在 GitExample 文件夹下生成一个 .git 的隐藏文件夹，这就表明 git 仓库初始化成功。

![gitExample](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/gitExample.png)

.git 文件夹中的文件就是 git 平时运作所需的必要文件，后面会详细介绍这些文件的作用。

## 从服务器 clone 仓库
这种方式可以简单的理解为，把服务器上的仓库拷贝一份到本地。

以 git 上比较出名的 AFNetworking 举例，如果你想把这个仓库拷贝到本地，总共也分两步：

1. 在 GitHub 上找到 AFNetworking 这个项目，并复制仓库地址。
2. 选好本地目录，并执行 `git clone 仓库地址`

GitHub 上的仓库地址：

![gitHub_server](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/gitHub_server.png)

执行完 `git clone 仓库地址` 命令后，本地目录就会多出来一个文件夹，这个就是拷贝下来的 git 仓库，在仓库中同样可以看到 .git 隐藏文件夹。

![git_clone](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_clone.png)
