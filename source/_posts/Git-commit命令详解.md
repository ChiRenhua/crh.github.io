---
title: Git-commit命令详解
date: 2019-08-24 18:14:57

categories:
- Git
tags:
- Git
---
![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_logo.png)

`git commit` 会将暂存区的所有文件拿出来，生成一个新的版本。那么在背后 `git commit` 都做了哪些操作呢，下面详细介绍下。

<!-- more -->

## 保存当前目录结构
首先 Git 会执行 `git write-tree` 命令保存当前的目录结构，这个操作会生成一个 Git 对象，同时也会将当前的目录结构生成一个哈希值，哈希值的前 2 位作为文件夹名，后 38 位作为 Git 对象名，存入 .git/objects 目录下。这个新生成的 Git 对象就是前文 [《Git-版本库目录结构解析》](http://chirenhua.com/2019/08/24/Git-%E7%89%88%E6%9C%AC%E5%BA%93%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84%E8%A7%A3%E6%9E%90/) 提到过的 tree 类型的文件。

这个 Git 对象存储了当前的目录结构以及对应目录结构下的 blob 文件，而 blob 文件就是暂存区中的文件。

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_treeObject.png)

## 生成新的版本
一个新的版本，就是保存当前的目录结构，以及目录结构下的文件对应的二进制对象，上一步操作已经将目录结构保存下来，下面就要将保存好的目录结构生成一个新的版本。

Git 执行 `git commit-tree` 命令，将 tree 对象生成新的版本。这个操作同样会生成一个 Git 对象，并存在 .git/objects 目录下。这个新生成的 Git 对象就是前文 [《Git-版本库目录结构解析》](http://chirenhua.com/2019/08/24/Git-%E7%89%88%E6%9C%AC%E5%BA%93%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84%E8%A7%A3%E6%9E%90/) 提到过的 commit 类型的文件。

这个 Git 对象存储了 tree 文件和作者信息。

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_commitObject.png)

## 其它命令

### git log
commit 信息中不仅存储了改动的文件和目录，还存储了作者信息以及提交的时间，查询一个项目的提交历史时，其实就是查询这些数据，可以通过 `git log` 命令来查看项目的提交历史。

### git checkout 2a18f9bd7f2caa...
每个 commit 都有一个唯一的哈希 id ，通过这个哈希 id 可以迅速找到某一次提交，基于这个特性，Git 可以方便的切换到某一次提交。

### git show 2a18f9bd7f2caa...
用于展示 2a18f9bd7f2caa... 这次提交的所有改动。
