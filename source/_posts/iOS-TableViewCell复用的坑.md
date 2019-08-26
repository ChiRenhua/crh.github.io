---
title: iOS-TableViewCell复用的坑
date: 2018-01-29 17:21:40

categories:
- iOS 开发总结
tags:
- iOS
---
最近在解决Bug时，偶然间发现了一个TableViewCell复用的坑，在这里总结下。

<!-- more -->

## 起因
在项目中，有些Cell会监听一些通知来执行特定的操作，举个简单的例子：  
在CellA和CellB中监听一个通知，当收到通知时会分别将Label的文案设置为“A”和“B”。这时数据发生改变，CellA被CellB取代。此时看起来CellA已经被干掉，但是当发起通知的时候，CellA竟然仍能够响应，这就导致Label的文案有时是“A”，有时是“B”。查看视图层级，并没有看到CellA的存在，一时有点懵逼，因为至少从视觉上看，CellA已经消失了。
## 排查
在CellA的“dealloc”方法里打断点，数据刷新后，CellA消失，但是并没有走到对应“dealloc”方法里，可见CellA并没有被真正的干掉，它应该还存在于内存中。初步推断是TableViewCell的复用机制导致的。一般出于性能考虑，在创建TableViewCell的时候，都会使用“reuse”的方式创建，避免重复创建的开销。但是究竟是不是这个原因导致的，还要深入排查下。
## 深究
先用一个简单Demo来验证下上面的推断:  
创建三个不同的Cell均继承自“UITableViewCell”。

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/iOS-TableViewCell%E5%A4%8D%E7%94%A8%E7%9A%84%E5%9D%91/002.png)

点击Demo中的按钮会将TableView中第一行Cell刷新为对应的“CellA / CellB / CellC”。

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/iOS-TableViewCell%E5%A4%8D%E7%94%A8%E7%9A%84%E5%9D%91/001.png)

依次点击三个按钮，观察下内存的变化。
点击“Button A”时，可以看到内存里创建了一个“A Test TableViewCell”。

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/iOS-TableViewCell%E5%A4%8D%E7%94%A8%E7%9A%84%E5%9D%91/003.png)

再点击“Button B”，内存里新创建了一个“B Test TableViewCell”，但是可以发现，“A Test TableViewCell”并没有被销毁，虽然此时在View上已经看不到了。

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/iOS-TableViewCell%E5%A4%8D%E7%94%A8%E7%9A%84%E5%9D%91/004.png)

同理，点击“Button C”，内存里新创建了一个“C Test TableViewCell”，“A Test TableViewCell”和“B Test TableViewCell”依然没有被销毁。  

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/iOS-TableViewCell%E5%A4%8D%E7%94%A8%E7%9A%84%E5%9D%91/005.png)

可见确实是因为Cell的复用导致旧的Cell一直存在于内存中，但是视图层级里却找不到它。那是否可以通过判断Cell的superView是否存在来确定当前Cell是否在复用池中呢？我们再做个实验，在点击Button的时候，发出通知，Cell在收到通知后，打印其SuperView。结果如下：

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/iOS-TableViewCell%E5%A4%8D%E7%94%A8%E7%9A%84%E5%9D%91/006.png)

很神奇，虽然Cell已经不见了，但是看起来它并没有从TableView中移除，也就是说复用池中的Cell仍在存在于TableView上。既然这样我们不妨把TableView的子View都打印出来，看下结果：

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/iOS-TableViewCell%E5%A4%8D%E7%94%A8%E7%9A%84%E5%9D%91/007.png)

果真3个Cell都在，但是其中AB两个在复用池中的Cell是被隐藏的！！！所以在视图层级里是看不到它。

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/iOS-TableViewCell%E5%A4%8D%E7%94%A8%E7%9A%84%E5%9D%91/008.png)

终于找到原因，我的问题也得以解决，在收到通知时判断下当前Cell的隐藏状态，如果隐藏就不响应通知即可。

## 总结
看来苹果对Cell复用的处理也是简单粗暴，直接隐藏，这也是导致被替换掉的Cell仍然能够响应通知的原因。所以以后在处理Cell时，可以考虑下以下两点建议：  
1.和Cell通信时，尽量避免使用通知，因为很难追踪到谁会响应通知去做些什么事情，很有可能触发一些奇怪的Bug。  
2.如果Cell占用很多内存的话，Cell复用会导致内存不能及时释放，可以考虑在内存过高时，清理TableView中处于Hidden的Cell。