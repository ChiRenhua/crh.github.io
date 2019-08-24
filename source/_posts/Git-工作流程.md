---
title: Git-工作流程
date: 2019-08-24 11:54:49

categories:
- Git
tags:
- Git
---
![git_logo](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_logo.png)

## 工作流程
Git 的本地仓库主要由三部分构成：

1. 工作目录，持有实际文件。
2. 暂存区，类似于缓存区域，存放临时修改。
3. HEAD 指针，一般指向最后一次提交。

![git_workflow](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_workflow.png)

整体流程就是：在工作目录对文件进行修改，修改后的文件通过 `git add` 命令添加到暂存区。所有修改完毕后，通过`git commit` 命令将暂存区内的所有文件生成一个新的版本记录，同时 HEAD 指针移动到最新的版本。

<!-- more -->

## Git 文件生命周期
Git 管理下的文件共有四种状态：

1. untracked（未跟踪）
2. unmodified（未修改）
3. modified（已修改）
4. staged（暂存）

在平时的开发过程中，文件会在这四种状态中来回扭转。

![git_fileLife](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_fileLife.jpg)

新创建的文件是 untracked（未跟踪） 的状态，此时 Git 还没有开始追踪这个新文件，通过 `git add fileName` 命令告诉 Git 开始追踪这个文件，并将文件加入到暂存区，此时文件状态扭转为 staged（暂存）。

对于 Git 已经开始跟踪的文件，如果没有改动，文件状态就是 unmodified（未修改）。

当对已经跟踪的文件进行修改时，文件的状态就会扭转到 modified（已修改）的状态，此时通过 `git add fileName` 可以将文件当前的改动存到暂存区，同时文件状态扭转为 staged（暂存）。

已经暂存的文件通过 `git commit` 命令生成一个新的版本。生成新版本后，所有文件都恢复到 unmodified（未修改）的状态。

## 小结
从上面的流程中可以发现两个经常出现的命令：

1. `git add`    // 将文件加入到暂存区
2. `git commit` // 将暂存区中的文件提交成一个新的版本

这是版本管理中最常用到的两个命令，后面会详细介绍。