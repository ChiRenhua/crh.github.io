---
title: iOS-NSCache
date: 2018-09-17 20:24:25
categories:
- iOS开发总结
tags:
- iOS
---
## NSCache 是什么
简单的理解，NSCache 就是一个可变的键值对集合，类似于 NSMutableDictionary，用于存储一些临时数据。在内存吃紧时 NSCache 会自动释放存储的临时数据。
## NSCache 特点
* 在内存吃紧的时候，NSCache 会删除部分缓存的临时数据，最大限度的减少内存的占用。
* NSCache 是线程安全的，增删改查操作无需加锁。
* 与 NSMutableDictionary 不同，NScache 的键（key）不会被复制。键（key）不需要实现 NSCopying 协议。

一般对于创建开销比较大或者重复占用内存的对象，可以考虑用 NSCache 存储。但是这些对象不应该影响程序的运行，因为系统可能会在内存不足的时候清理掉这些对象，此时就需要在必要时重新创建对象。

NSCache 中的对象是被缓存强引用的，如果创建的对象希望在不需要它时能够被缓存及时释放掉，可以实现 NSDiscardableContent 协议。这样当此对象已经不被需要时，缓存也会丢弃这个对象以节省内存。

## NSCache 使用
# 变量
``@property(copy) NSString *name; 
``

缓存名称（默认值为空 “”）

``@property NSUInteger countLimit;
``

最大缓存对象数量（0 代表无限制，默认是 0），如果缓存的对象数量超过了这个限制，会清除部分缓存中的对象。

``@property NSUInteger totalCostLimit;
``

缓存能够占用的最大内存。（0 代表无限制，默认是 0）。

在将对象添加到缓存时，如果此对象的添加会导致缓存占用的大小超过 totalCostLimit 限制，缓存可能会直接抛弃此对象。

# 协议

**1. NSDiscardableContent**

当对象希望在不被使用时缓存能够释放掉自己，可以实现此协议以节省更多内存。

``@property BOOL evictsObjectsWithDiscardedContent;
``

缓存是否会自动删除已经被废弃的对象。默认是 YES。

``- (BOOL)beginContentAccess;
``

返回一个 BOOL 值用来表示当前对象是否正在使用。

