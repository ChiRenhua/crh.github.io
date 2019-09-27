---
title: Git-LFS
date: 2019-09-27 19:52:03

categories:
- Git
tags:
- Git
---
![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_logo.png)

# 简介
LFS 全称 Large File Storage，顾名思义是用来存储大文件的。在 Git 中，每次提交都会将改动转为二进制文件保存起来，但是对于图片、视频、音频等本来就是二进制的文件，Git 无法识别出差异，所以每次提交都会生成一个新的拷贝。也就是说，一个 100M 的图片，哪怕只改了一点点，提交新版本后它都会在仓库里占用 200M 的空间。

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_lfs_image.png)

为了解决这个问题，GitHub 提出了 LFS，将图片、视频、音频等大文件存储到主仓库外的一个仓库里。

# 原理
LFS 的原理是将大型二进制文件存储到主仓库外的另外一个仓库里，本地仓库并不会存储这些大文件，仅仅是保留一个指向大文件的指针，当用户切换到某一分支时，LFS 会根据当前分支的情况下载对应的大文件。

# 使用
## 下载 LFS
可以[官网下载安装](https://git-lfs.github.com/)。

Mac 用户可以通过 homebrew 安装 ```brew install git-lfs```。

### 安装 LFS
进入目标仓库目录，然后运行 ```git lfs install``` 安装 LFS。

### 使用 LFS
通过下面命令选择你希望 LFS 管理的文件类型或者目录。

```git lfs track "*.psd"```

选择好后别忘了添加 .gitattributes。

```git add .gitattributes```

一切就绪，后面当我们提交的文件中包含 .psd 类型的文件时，Git 会自动使用 LFS 对其进行管理。

```
git add file.psd
git commit -m "Add design file"
git push origin master
```

# 结论
建议二进制文件都使用 LFS 进行管理，避免频繁的改动使仓库越来越大，如果当前 Git 仓库已经提交了很多大文件变得很大，还有个[后悔药](https://chirenhua.com/2019/08/29/Git-%E4%BB%93%E5%BA%93%E7%98%A6%E8%BA%AB/)可以吃。



