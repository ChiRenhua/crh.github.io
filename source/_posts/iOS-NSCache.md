---
title: iOS-NSCache
date: 2018-09-17 20:24:25
categories:
- iOS 开发总结
tags:
- iOS
---
## NSCache 是什么
简单的理解，NSCache 就是一个可变的键值对集合，类似于 NSMutableDictionary，用于存储一些临时数据。在内存吃紧时 NSCache 会自动释放存储的临时数据。
## NSCache 特点
* 在内存吃紧的时候，NSCache 会删除部分缓存的临时数据，最大限度的减少内存的占用。
* NSCache 是线程安全的，增删改查操作无需加锁。
* 与 NSMutableDictionary 不同，NScache 的键（key）不会被复制。键（key）不需要实现 NSCopying 协议。

一般对于初始化开销比较大或者重复占用内存的对象，可以考虑用 NSCache 存储。但是这些对象不应该影响程序的运行，因为系统可能会在内存不足的时候清理掉这些对象，此时就需要在必要时重新创建这些对象。

NSCache 中的对象是被缓存强引用的，如果创建的对象希望在不需要它时能够被缓存及时释放掉，可以实现 NSDiscardableContent 协议。这样当此对象已经不被需要时，缓存也会丢弃这个对象以节省内存。

## NSCache 介绍
### 变量
``@property(copy) NSString *name; 
``

缓存名称（默认值为空 “”）

``@property NSUInteger countLimit;
``

最多支持缓存对象的数量（0 代表无限制，默认是 0），如果缓存中的对象数量超过了这个限制，会清除部分对象直至缓存数量低于 countLimit。

``@property NSUInteger totalCostLimit;
``

缓存对象占用的内存上限。（0 代表无限制，默认是 0）。

在将对象添加到缓存时，如果此对象的添加会导致缓存占用的内存大小超过 totalCostLimit 限制，缓存可能会直接抛弃此对象。

### 协议

**1. NSDiscardableContent**

如果对象希望在不被使用时，缓存能够及时释放掉自己，可以实现此协议以节省更多内存。实现此协议的对象会有一个 counter 值来管理其生命周期，当此对象正在使用时，counter 值大于等于 1 。当此对象已经不被使用时，counter 值为 0 ，此时缓存策略会将此对象移除缓存。

``@property BOOL evictsObjectsWithDiscardedContent;
``

缓存是否会自动删除已经被废弃的对象。默认是 YES。

``- (BOOL)beginContentAccess;
``

返回一个 BOOL 值用来表示当前对象是否正在使用。此方法会使 counter 值 +1。

``- (void)endContentAccess;
``

此方法会使 counter 值 -1，当 counter 值减少到 0 时，缓存策略会及时清除此对象。

``- (void)discardContentIfPossible;
``

当对象的 counter 值为 0 ，缓存策略开始清除此对象时会调用此方法。

``- (BOOL)isContentDiscarded;
``

此对象是否被清除。

**2. NSCacheDelegate**

``- (void)cache:(NSCache *)cache willEvictObject:(id)obj;
``

当此对象（obj）准备从缓存（cache）中移除时，会调用此方法。

### 方法
``- (ObjectType)objectForKey:(KeyType)key;
``

通过 key 取缓存的对象。

``- (void)setObject:(ObjectType)obj forKey:(KeyType)key;
``

通过 key 将对象（obj）存到缓存中。

``- (void)setObject:(ObjectType)obj forKey:(KeyType)key cost:(NSUInteger)g;
``

这个方法跟 “setObject:forKey:” 类似，但是多了一个 cost 值，这个值代表此对象占用空间的成本，但是这个值具体会有什么作用，苹果官方文档并没有说明。文档中有提到，当内存不足时，会移除部分对象释放内存，但是释放对象的过程并不会按照某种特定顺序来，所以如果想通过 cost 值来影响释放行为是没用的。如果计算这个 cost 值需要额外开销，cost 值可以直接传 0，或者简单点，使用 “setObject:forKey:” 方法。

``- (void)removeObjectForKey:(KeyType)key;
``

删除缓存中 key 对应的对象。

``- (void)removeAllObjects;
``

删除缓存中所有的数据。

## 总结
NSCache 用法比想象中简单的多，因为缓存清理的策略已经由系统帮我们处理。

