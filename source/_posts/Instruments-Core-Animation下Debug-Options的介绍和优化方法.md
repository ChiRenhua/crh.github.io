---
title: Instruments--Core Animation下Debug Options的介绍和优化方法
date: 2017-02-12 23:16:52  

categories:
- XCode 工具
tags:
- 工具
- XCode
- Instruments
---
![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Instruments--Core%20Animation下Debug%20Options的介绍和优化方法/Instruments_Logo.png)  
## 关于视图的优化
在开发一个产品的过程中，我们不仅仅要实现功能，其实使用的体验也是很重要的一部分，那么使用过程中非常影响用户体验的就是流畅度，所以如何提高流畅度，避免卡顿是一个必须要重视的问题。好在强大的苹果为开发者提供了一个工具--Instruments，通过它我们可以轻松的找出拖慢我们产品的元凶。

<!-- more -->

首先打开Instruments，选择Core Animation，然后打开一个目标程序，点击Record，此时Instruments就开始监控你的程序，并且实时展示帧数。在屏幕右下侧部分点击设置按钮，会展开一个页面，上面有Debug Options各个选项，下面介绍各个选项所展示的性能问题和优化方法。 
 
![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Instruments--Core%20Animation下Debug%20Options的介绍和优化方法/Instruments_DebugOptions.png)  

## Color Blended Layers（混合图层）
混合图层的概念其实就是多个图层盖在了一起，我们知道屏幕是一个一个像素点组合在一起的，而像素点展示什么颜色是由R、G、B、alpha四个部分组成，所以如果多个图层盖在一起，那么其最终的颜色，会受到每个图层的影响。而在计算最终颜色的过程中，会消耗GPU的资源，所以如果你只想显示最上层图层的颜色，那么可以将它设置为不透明，这样的话GPU会自动忽略下面的所有View，从而减少GPU的压力。 
 
### UILable避免混合图层的办法：
为UILable设置非透明背景色：
``lable.backgroundColor = [UIColor whiteColor];``
如果UILable展示的是中文，设置  ``layer.masksToBounds = YES;``  
不要慌，这里不会触发离屏渲染，在iOS8以后，展示中文时，UILable外围会有一圈透明的边，所以才要加这个，看下图）  

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Instruments--Core%20Animation下Debug%20Options的介绍和优化方法/Instruments_03.png)

### UIImageVIew避免混合图层的办法：
很简单，只要图片本身没有透明的地方就可以啦，然后代码保证没有设置alpha值一类的属性就没问题。  

## Color Hits Green and Misses Red (光栅化)  
如果shouldRasterize被设置成YES，对应的渲染结果会被缓存，如果图层是绿色，就表示这些缓存被复用；如果是红色就表示缓存会被重复创建，这就表示该处存在性能问题了。  

所以一般在图像内容不变的情况下才使用光栅化，例如设置阴影耗费资源比较多的静态内容，如果使用光栅化对性能的提升有一定帮助。反之如果图像内容总会变就不要设置这个属性。  

如果你在一个界面中使用了光栅化，刚进去这个页面的所有使用了光栅化的控件layer都会是红色，因为还没有缓存成功，如果上下滑动你会发现，layer变成了绿色。但是如果你滑动幅度较大会发现，新出现的控件会是红色然后变成绿色，因为刚开始这些控件的layer还没有缓存。  

注意点：  
（1）系统给光栅化缓存分配了一个固定的大小，因此不能过度使用，如果超出了缓存也会造成离屏渲染。  
（2）缓存的时间为100ms，因此如果在100ms内没有使用缓存的对象，则会从缓存中清除。  

## color copied images（图片颜色格式）
苹果给出的官网解释：  
”If an image is in a color format that the GPU can not directly work with, it will be converted in the CPU.” 
 
翻译过来就是：苹果的GPU只解析 32bit 的颜色格式。  

如果你放一张图片，而它的颜色格式不是 32bit ，CPU会先进行颜色格式转换，再让GPU渲染。这样无形中就消耗了CPU部分性能。  

解决办法：  

1. 让设计师给图的时候注意要是32bit的。  
2. 再开一个异步线程去处理颜色格式的转换。  


（还是第一个办法好）  
## Color Non-Standard Surface Formats（颜色非标准表面格式）
没查到时嘎哈的，只知道大概跟文本的显示有关。
## Color Immediately（立即执行颜色刷新）
默认的颜色刷新有10ms延迟，这个选项可以去掉这个延迟，某些情况下调试可能会用到。
## Color Misaligned Images(图片大小不符)
当展示的图片和容器的大小不匹配时image会被标黄，尽量用大小符合的图片，不然会消耗资源对图片进行压缩或放大。  
## Color Offscreen-Rendered Yellow（离屏渲染）
参考文章：[iOS-离屏渲染带来的性能问题和解决办法](https://chirenhua.github.io/2016/10/22/iOS-离屏渲染带来的性能问题和解决办法/)
## Color Compositing Fast-Path Blue (快速路径)
标记由硬件绘制的路径为蓝色，蓝色越多越好.
## Flash Updated Regions (重绘区域)
这个选项会对重绘的内容高亮成黄色,重绘就是指使用Core Graphics绘制，绘制会损耗一定的性能，因此重绘区域应该越小越好，对于未发生变化的内容则不应该重绘。