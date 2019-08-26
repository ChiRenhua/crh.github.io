---
title: iOS-AFNetworking 使用遇到的问题汇总
date: 2018-10-17 16:08:39
categories:
- 踩过的坑
tags:
- iOS
- AFNetworking
---
![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/iOS-AFNetworking%20%E4%BD%BF%E7%94%A8%E9%81%87%E5%88%B0%E7%9A%84%E9%97%AE%E9%A2%98%E6%B1%87%E6%80%BB/3509814878-58e86f67a028b.png)
## 1. unacceptable content-type 错误
这个错误表示回包类型无法解析，一般这种情况请求回包的状态码是：200，表示错误并不在后台。详细错误描述为：

``Request failed: unacceptable content-type: text/plain（或者 text/html）
``

可以看出错误的原因是 AFNetworking 框架并不支持解析 ``plain`` 和 ``html`` 类型的数据。

**解决办法：**在 ``AFURLResponseSerialization.h`` 类里面找到 ``self.acceptableContentTypes = [NSSet setWithObjects:@"application/json", @"text/json", @"text/javascript", nil];`` 
然后在后面追加 ``@"text/html", @"text/plain"``两个参数即可。
``self.acceptableContentTypes = [NSSet setWithObjects:@"application/json", @"text/json", @"text/javascript", @"text/html", @"text/plain", nil];``