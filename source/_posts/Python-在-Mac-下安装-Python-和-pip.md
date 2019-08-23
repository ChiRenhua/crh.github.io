---
title: Python-在 Mac 下安装 Python 和 pip
date: 2018-04-13 10:55:26  
categories:
- Python 开发
tags:
- Python
---
![Python_logo](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Python-%E5%9C%A8%20Mac%20%E4%B8%8B%E5%AE%89%E8%A3%85%20Python%20%E5%92%8C%20pip/Python_Logo.jpg)  

Mac 系统默认有安装 Python2.7 ，但是这个版本已经过时了，Python3 作为一次革命性的更新，取代 Python2.x 版本只是时间问题。作为一个开发人员，我们一定要跟上时代的潮流！

**注意⚠️：网上有很多方法可以将系统的 Python2.7 升级为 Python3，但是这样可能会引发奇怪的错误，因为你不知道系统的哪些功能或者 App 是依赖 Python2.7 的。所以建议 Python2.7 和 Python3 共存。开发时只需要用 Python3 命令去打开 .py 即可。**

## 1. 安装 Python3 

**1. 先安装 C 编译器，打开终端运行：``xcode-select --install`` 来安装 Xcode 命令行工具（前提是有安装 Xcode ）**  

**2. 安装 Homebrew，打开终端运行：**  
**``/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"``**  

**3.安装 Python3，打开终端运行：**  
**``brew install python``**

等一会儿就安完了～  
此时在终端输入 Python 会展示系统默认的 Python 2.7 信息  
输入 Python 3 会展示刚刚安装的 Python 3 信息。  

## 2. 安装 pip
pip 是啥：

>pip 是一个以 Python 计算机程序语言写成的软件包管理系统，他可以安装和管理软件包，另外不少的软件包也可以在“ Python 软件包索引”     网站：www.pip-installer.org 源代码库：github.com/pypa/pip  
维基百科

通俗地说就是用来管理 Python 包和第三方库的工具，很方便。

如果是使用 Python.org 或 Homebrew 的安装程序来安装 Python，此时应该已经有 pip 了
在终端输入 pip 应该就会展示 pip 的信息了，如果没有，说明没有安装，可以通过下面的方法安装。

Python 有自带 " easy_install "  
直接在在 Terminal 里面输入 ``sudo easy_install pip`` 进行安装即可。  
等几秒就OK了。