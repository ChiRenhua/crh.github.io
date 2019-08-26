---
title: Git-远端
date: 2019-08-26 17:25:37

categories:
- Git
tags:
- Git
---
![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_logo.png)

# 远端

根据 Git 的特点，每个人本地都是一个完整的版本库，没有服务器其实也是可以工作，只要两台电脑能够互相连接上，就可以进行数据的交换。但是在实际使用时，没人会通过这种方式互相推送修改，因为两台电脑很有可能不在一个局域网下。所以 Git 一般也会有一个中央服务器，他的存在就是为了方便大家进行数据的交换。

远端指的就是这台中央服务器上的版本库。

## 添加远端
本地已经创建好的仓库可以通过 `git remote add origin serverPath` 命令添加远端。

`origin` 是远端在本地的名称，可以修改为其它名称。

`serverPath` 对应远端的地址。

如果是从远端 clone 仓库，那在 clone 下来时，远端就已经添加好了，默认名称是 origin。

## 查看远端
远端添加完后，可以通过 `git remote show` 命令查看远端。

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_remote_show.png)

`git remote -v` 用于查看远端的详细信息。

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_remote_v.png)

## 删除远端
`git remote remove origin` 用于删除远端，`origin` 是要删除远端的名字。

## 重命名远端
`git remote rename origin origin_new` 如果添加远端时起了一个不合适的名字，可以通过这个命令修改远端的名字。`origin` 是当前远端的名字，`origin_new` 是远端的新名字。

# 与远端交互
远端关联完成后，就可以与远端进行数据交互了。假设本地我们已经提交了一个新的版本，如果想把本地的提交同步到远端，需要进行两步操作：

1. 先执行 `git pull` 命令，将远端数据同步到本地，如果此时有冲突，则需要解决冲突。
2. 执行 `git push` 命令，将本地的修改同步到远端仓库。

## git pull
`git pull` 命令会将远端的数据同步到本地，`git pull` 命令实际上是执行了两步操作：

1. 执行 `git fetch` 命令，将远端仓库的数据同步到本地的远端仓库副本里。
2. 执行 `git merge` 命令，将远端仓库副本里的数据和当前分支的数据进行合并，如果没有冲突，合并完成。如果有冲突则需要先解决冲突。

完成后，远端的数据就已经同步到本地了。

## git push
上一步我们已经将远端的数据同步到了本地，但是我们本地提交的版本还没有同步到远端。

`git push` 命令会将本地提交的所有版本同步到远端以及远端的本地副本。

# Git 提交流程
到这里，一个完整的 Git 流程就浮出水面了，通过下面的图可以回顾下整个 Git 的提交流程：

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Git/git_push.png)

1. `git add` 将修改的文件添加到暂存区。
2. `git commit` 将暂存区中的所有文件提交成一个新的版本。
3. `git pull` 从远端同步数据到本地。
4. `git push` 将本地新的版本同步到远端。

