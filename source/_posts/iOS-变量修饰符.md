---
title: iOS-变量修饰符
date: 2019-05-26 17:48:20
categories:
- iOS 开发总结
tags:
- iOS
---
# atomic
默认属性，系统在生成 get / set 方法时通过自旋锁保证 get / set 操作的完整性，当多个线程同时访问属性时，能够保证操作按顺序执行。

如果重写了 get / set 方法，那 atomic、nonatomic、copy 等修饰符都将毫无卵用。

锁会产生额外开销，所以使用 atomic 修饰的变量读写速度不快。

atomic 只能保证多线程下的读写安全，当其它线程释放了这个对象时，就可能会造成 crash，所以 atomic 并不是线程安全的。

<!-- more -->

# nonatomic
非读写安全，好处是没有锁，速度快。

# assign
assign 一般用来修饰基础数据类型，尽量不用它修饰对象，因为 assign 修改对象并不会使引用计数 +1，会被 ARC 及时清除，但此时 assign 的指针地址依然存在，就会造成野指针。

# weak
一般用来修改对象，使用它修饰时，引用计数不会 +1，是当它被 ARC 自动释放时，其对应的指针也会置为 nil，避免了野指针的问题。

weak 指针在引用的对象被回收的时候会自动置为 nil，原理是 RunTime 维护了一个 weak 表，用来存储 weak 指针。weak 表是一个 hash 表，key 是对象地址，value 是 weak 指针地址数组。当对象被置为 nil 时，会通过对象地址找到 weak 指针地址数组，然后将数组中每个地址的数组置为 nil。然后将其从 weak 表中删除。由此可见，weak 指针涉及 hash 表的增删改查，所以也有一定的性能开销。

# strong
用来修饰对象，引用计数 +1，只有当引用计数为 0 时才会被回收，也可以将对象设置为 nil 来销毁它。

# copy
copy 和 strong 都是强引用，不同的是，strong 的复制是多个指针指向同一个地址，而 copy 是复制对象本身，指针会指向不同的地址。

copy 使用时要注意不能修饰可变对象，例如：NSMutableArray、NSMutableDictionary 等，因为使用 copy 修饰后，此对象会变为不可变的，如果此时使用可变对象的方法，会导致 crash。

# readonly
只读，只有 get 方法，没有 set 方法。

# readwrite
可读可写，有 get 方法，也有 set 方法。

# __block
用在 block 中，block 可以访问局部变量，但是不能修改，如果要修改就要用 `__block` 修饰。  

# __weak
用在 block 中，用来修饰变量，weak 用来修饰属性。

# __strong
用在 block 中，防止对象被提前释放。  
举例：

```
__weak typeof(self) weakSelf = self;
[self setBlock:^{
	__strong typeof(weakSelf) self = weakSelf;
	self.string = @"hello world";
}];
```
