---
title: Git-变基
date: 2019-09-26 20:29:20

categories:
- Git
tags:
- Git
---
![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_logo.png)

变基本名 rebase ，也不知道是谁给它起了这么拗口的名字。

上一篇[文章](https://chirenhua.com/2019/09/26/Git-%E5%90%88%E5%B9%B6%E5%88%86%E6%94%AF/)我们学习了 merge 操作，知道了两个分支之间同步代码可以使用```git merge```命令，但是 merge 命令有一个小缺点让一部分完美主义者无法接受，那就是每次 merge 完成之后，都会生成一个新的 commit。

假如频繁的从 master 分支同步代码到 develop 分支，那 develop 分支上会多出来很多 merge 节点，这些节点有些人觉得很多余，污染了提交历史，让它阅读起来很困难。也有一部分人觉得应该保留开发过程中的每一个操作，方便追踪问题，所以不应该去掉这些节点。两种想法没有对错，仅看个人习惯，如果想同步代码后不产生新的节点，那可以使用一个新的命令```git rebase```。

<!---more--->

## 流程
base 基本点，在 Git 中指分支开始分叉的那个节点。

re 重新设置。

rebase 可以理解为重新设置基本点，也就是修改分支分叉的位置。

### 1. 切换到目标分支

使用```git checkout develop```命令，切换到 develop 分支，develop 可以替换为任何目标分支。

### 2. 变基
使用```git rebase master```命令，将 master 代码 rebase 到 master，如果没有冲突则 rebase 完成。

### 3. 解决冲突
如果产生了冲突，则需要先解决冲突，冲突解决完毕后，使用```git rebase --continue```命令继续执行 rebase 操作。如果想终止此次 rebase 可以使用```git rebase --abort```命令。

## 原理
下图展示的是将 master 分支代码 rebase 到 develop 分支的过程：

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_rebase.gif)

从图中我们可以发现，在执行 rebase 之前，develop 分支是基于 C2 节点的，执行 rebase 操作后，develop 分支的基点移动到了 C4，而 develop 分支的 C5 和 C6 节点会重新在 C4 之后再重放一遍。

C5 和 C6 节点看起来是从 C2 节点后移动到了 C4 节点后，但实际上 Git 并没有移动 C5 和 C6 节点，而是在 C4 节点后，又提交了两个跟 C5 和 C6 相同的节点 C5' 和 C6'，C5 和 C5' 除了 commit id 不同，其余完全一样。

## rebase 黄金法则
**永远不要对一个公共分支执行 rebase 操作。**

在执行 rebase 操作前，请默念此法则三遍。一旦对公共分支执行了 rebase 操作，那整个分支将会变得不可维护。