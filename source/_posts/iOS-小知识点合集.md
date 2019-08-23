---
title: iOS-小知识点合集
date: 2019-03-11 10:49:32
categories:
- iOS 开发总结
tags:
- iOS
---
![ios_logo](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/iOS-%E5%B0%8F%E7%9F%A5%E8%AF%86%E7%82%B9%E5%90%88%E9%9B%86/iOS-7-10-Emblem.jpg)
## Crash 相关
1. 针对 iOS 8.4 及以下系统，在 layoutSubViews 方法中，不要删除一个 View（view = nil）。
