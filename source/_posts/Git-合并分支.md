---
title: Git-合并分支
date: 2019-09-26 19:49:57

categories:
- Git
tags:
- Git
---
![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_logo.png)

在 develop 分支开发完成后，需要将 develop 分支的代码同步到 master，这时 merge（合并）就派上了用场。

merge 是 Git 中非常常用的一个命令，并行开发中各个分支代码的同步基本都要使用 merge。

<!---more--->

## 流程

### 1. 切换到目标分支

使用```git checkout master```命令，切换到 master 分支，master 可以替换为任何目标分支。

### 2. 合并

使用```git merge develop```命令，将 develop 代码合并到 master，如果没有冲突则合并完成，同时会生成一个新的 commit 提交，commit 的内容就是 merge 的内容。

### 3. 解决冲突

如果产生了冲突，则需要先解决冲突，冲突解决完毕后，使用```git add```命令跟踪解决完的改动，同时也会去掉该文件的冲突标记，最后使用```git commit```命令提交这次 merge。

## 结语
合并完成后，临时分支就可以删除了。

比如，开发者在 develop 分支完成开发，并将 develop 分支的代码合并回 master 分支，合并完成后，develop 分支就完成了它的使命，可以删除。

还有一种情况是开发过程中同步主线代码到开发分支，也是使用 merge 命令进行操作，不同的是这次可不要把 master 分支删掉哦。

下图展示的是将 master 分支代码同步到 develop 分支的过程：

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_merge.gif) 


