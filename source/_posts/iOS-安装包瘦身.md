---
title: iOS-安装包瘦身
date: 2018-05-20 20:34:41  
categories:
- iOS 开发总结
tags:
- iOS
---
随着 APP 版本的迭代，安装包无法避免的会变的越来越大。分析安装包的构成可以发现，安装包主要是由资源文件和代码构成的，所以从这两个点出手优化可以达到一个很明显的效果。同时编译选项的配置也会对安装包的大小有一定影响。

<!-- more -->

## 1. 资源文件
### 1. 删除无用图片
使用开源工具 [**LSUnusedResources**](https://github.com/tinymind/LSUnusedResources) 检查重复图片，但是可能会有误报，比如 ``[@"image%d", index]`` 这种引用方式无法检查到，需要人工在核对一边。

### 2. 图片文件压缩
使用开源工具 [**imageOptim**](https://github.com/ImageOptim/ImageOptim) 对所有图片压缩一遍。此工具会使用 git 上主流的图片压缩方法尝试一遍，选择最优方案。

### 3. 纯色图片使用代码生成
如果项目中纯色的图片比较多，可以考虑使用代码替代，生成后缓存到本地以供后期使用。

### 4. 不常用图片后台下发
对于项目中不常用的图片可以考虑由后台下发，但是此项收益可能不高，而且会影响使用体验，酌情使用。

### 5. 字体文件
字体文件一般都很大，如果项目中使用了多种字体文件，可以删掉不常用的字体文件。

## 2. 代码
### 1. 删除不使用的类
项目迭代很久后，会有很多历史遗留的代码，已经不会再调用到，可以使用开源工具 [**WHC_Scan**](https://github.com/netyouli/WHC_Scan) 检查没有使用的类，但是可能会有误报，而且 OC 语言的特性导致需要人工 check 一遍。

### 2. 删除不使用的方法
方法和类不在一个数量级，所以检查起来比较耗时，而且一样会有误报，如果人工核实一遍的话耗时不说收益也偏少。

### 3. 精简库文件
在编译选项中选择生成 LinkMap 文件，编译完成后会在对应目录下生成 LinkMap 文件。分析 LinkMap 文件可以知道每个库文件的大小。

分析工具可以使用 [**LinkMap**](https://github.com/huanxsd/LinkMap) ，效果如下图：

![LinkMap](https://raw.githubusercontent.com/huanxsd/LinkMap/master/ScreenShot2.png)

**1. 检查删除没有使用的库文件。**

**2. 对于过于庞大的库，检查下是否使用到了库中的所有功能，避免为了使用一小部分功能而引用了整个库。**

**3. 检查重复的库文件，删除多余的库。**

### 4. 删除无用功能代码
**1. 对于一次性 / 活动需求，确认不会再使用后及时清理掉。**

**2. 过时的功能，旧的 UI 样式，确实不再使用及时清理。**

## 3. 编译选项
### 1. 去掉符号信息
在 Release 版本把 Strip Debug Symbols During Copy 和 Symbols Hidden by Default 设为 YES，可以去除不必要的调试符号。

Symbols Hidden by Default 会把所有符号都定义成 ”private extern”。  

去掉符号信息会让安装包大小减少很多，但是代价是在 APP 里打印堆栈信息无法还原出对应的类名和方法名。

### 2. 编译选项优化
Release 版本的 Optimization Level 应该设置为 Fastest, Smalllest。


