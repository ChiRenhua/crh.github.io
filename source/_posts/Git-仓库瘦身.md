---
title: Git-仓库瘦身
date: 2019-08-29 10:10:46

categories:
- Git
tags:
- Git
---
![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_logo.png)

# 背景
随着项目的不断开发，仓库自然而然会变得越来越大，如果项目中只是纯文本文件，那仓库的大小增加的会非常缓慢。但是如果仓库中存放了视频、图片、音频等比较大的二进制文件，那这个仓库估计经历几次提交就爆炸了。尤其 clone 仓库时，如果你面对几十个 G 的仓库，估计还没开发心态就崩了。

<!-- more -->

# 分析
前文我们有了解过，Git 提交版本时，是将改动的文件压缩成二进制文件存储的，但是对于已经是二进制文件的视频、图片等文件，Git 无法对其进行压缩，也没有办法对比出差异，所以每次改动这些文件时，Git 都会完整的拷贝一份。也就是说一个 100M 的图片，哪怕只改了一点点，提交新版本后他都会在仓库里占用 200M 的空间。

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_lfs_image.png)

既然视频、图片等类型的二进制文件会造成仓库膨胀，有人会认为那把这些文件从项目中删掉不就可以了。too young！从项目中删除这些文件只是让项目工程变小了，实际上 Git 仓库里还保留着这些文件的提交历史，所以仓库本质上并没有变小。

# 方案

## 方案一
为了解决这个问题，Git 提供了一个底层命令 `git verify-pack`，可以先找出占用空间比较大的文件。

```
$ git verify-pack -v .git/objects/pack/pack-3f8c0...bb.idx | sort -k 3 -n | tail -3
e3f094f522629ae358806b17daf78246c27c007b blob   1486 734 4667
05408d195263d853f09dca71d55116663690c27c blob   12908 3478 1189
7a9eb2fba2b1811321254ac360970fc169ba2330 blob   2056716 2056872 5401
```

然后通过 `filter-branch` 命令从提交历史中彻底删除这个文件。

```
$ git filter-branch --index-filter \ 'git rm --cached --ignore-unmatch git.tbz2' -- 6df7640^..
Rewrite 6df764092f3e7c8f5f94cbe08ee5cf42e92a0289 (1/2)rm 'git.tbz2'
Rewrite da3f30d019005479c99eb4c3406225613985a1db (2/2)
Ref 'refs/heads/master' was rewritten
```
现在历史记录中已经不包含对那个文件的引用了。不过 `reflog` 以及运行 `filter-branch` 时 Git 往 .git/refs/original 添加的一些 refs 中仍有对它的引用，因此需要将这些引用删除并对仓库进行 `repack` 操作。在进行 `repack` 前需要将所有对这些 commits 的引用去除：

```
$ rm -Rf .git/refs/original
$ rm -Rf .git/logs/
$ git gc
Counting objects: 19, done.
Delta compression using 2 threads.
Compressing objects: 100% (14/14), done.
Writing objects: 100% (19/19), done.
Total 19 (delta 3), reused 16 (delta 1)
```

## 方案二
使用 BFG Repo-Cleaner，这是 git-filter-branch 的替代方案，优点是快，超级的快，使用起来也很简单，推荐使用此方案。

具体使用方法可以参考[官方文档](https://rtyley.github.io/bfg-repo-cleaner/)。

注意事项：

1. 在优化完仓库准备 push 到远端时，记得先删除远端的 tag。因为 BFG Repo-Cleaner 会对 tag 进行清洗，而远端的 tag 不支持写入，所以需要先删除远端的 tag，push 时会将清洗后的 tag 提交上去。
2. 清洗完的仓库提交到远端后，要通知其他合作的同事废弃掉本地的旧仓库，重新 `clone` 一份新的。避免其他人的提交将已经删除的数据又提交回来。