---
title: Git-安装
date: 2019-08-23 15:34:40

categories:
- Git
tags:
- Git
---
![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_logo.png)

## 安装

1. 直接官网下载安装包安装（[传送门](https://git-scm.com/downloads)），一路点下一步就行。
2. 喜欢图形化界面的朋友，可以下载安装 SourceTree（[传送门](https://www.sourcetreeapp.com/)），同时也会安装好 Git，一路点下一步即可。

<!-- more -->

## Mac
Mac 上可以通过 [homebrew](https://brew.sh/) 安装 git，一行命令搞定：

``
brew install git
``

## 安装成功
安装完毕后，在终端上输入命令 git ，如果出现下面的提示就表明安装成功。

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_load_success.png)

## 配置 Git
安装完毕后，需要简单配置下 git 才能使用，在终端输入下面两个命令：

```
git config --global user.name "your name"
git config --global user.email "email@example.com"
```

在使用 git 时，git 需要知道是谁做出了修改，这两个命令就是告诉 git 这台机器的作者和 email。这两个信息会体现在提交的 commit 中。

`--global` 这个参数表示在全局生效，也就是这台机器所有的 git 仓库都会使用这个用户名和 email。如果只想在当前仓库生效，则去掉 `--global`即可。


