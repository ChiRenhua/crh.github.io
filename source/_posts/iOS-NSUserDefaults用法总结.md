---
title: iOS--NSUserDefaults用法总结
date: 2016-09-16 21:00:41  
categories:
- iOS 开发总结
tags:
- iOS
---
## NSUserDefaults原理  
NSUserDefaults是iOS为我们提供的一种轻量级的本地存储方式，其工作原理非常简单，就是将数据存储到本地的plist文件中。其实建立plist文件我们自己也可以完成，只不过自己去操作文件的创建、读取等会比较麻烦，使用“NSUserDefaults”则可以完全省略掉这些步骤，只需要进行简单的读写就可以了。

<!-- more -->

## NSUserDefaults支持的数据类型
因为NSUserDefaults是创建的plist类型的文件，所以它支持的数据类型也必须是plist文件能够存储的数据类型，分别是：**NSNumber（NSInteger、float、double），NSString，NSDate，NSArray，NSDictionary，BOOL**。需要注意的是，就算是存储的对象是“NSArray”类型的，那也要保证“NSArray”中存储的数据必须是plist文件支持的类型。如果需要存储plist不支持的数据类型，比如说一张图片或者一个自定义对象（下文会提到），可以先将其归档为“NSDate”类型，在存储到plist文件中。
## NSUserDefaults用法  
NSUserDefaults是一个单例，我们可以在程序的任何地方获取到它的唯一实例化对象：
<pre>NSUserDefaults *userDefault = [NSUserDefaults standardUserDefaults];</pre>  
存储一个数据：  
<pre>[userDefault setObject:@"测试" forKey:@"test"];</pre>  
由于NSUserDefaults是隔一段时间将数据写到本地，所以为了保险起见，建议加上下面的代码，确保数据及时的存储。  
<pre>[userDefault synchronize];</pre>  
取出一个数据也是非常的简单：  
<pre>NSString *str = [userDefault objectForKey:@"test"];</pre>

## 使用NSUserDefaults存储自定义对象  

### 1.将自定义类型转换为NSDate类型
其实不建议将数据这样存储，多数据复杂类型还是建议存到数据库中，如果非要这样存储的话，可以先将自定义类型归档为NSDate类型，因为plist文件是支持存储NSDate类型数据的，然后再通过NSUserDefaults进行存储。下面举个例子：  
假如我们要把书籍的对象存储起来。首先要在“.h”文件中声明“NSCoding”协议，并在“.m”文件中实现“encodeWithCoder”和“initWithCoder”两个方法。  
.h文件：
<pre>//
//  Book.h
//  Book
//
//  Created by Renhuachi on 16/3/22.
//  Copyright © 2016年 software. All rights reserved.
//

＃import < Foundation/Foundation.h >

@interface Book : NSObject <NSCoding>
＃pragma mark - 属性
@property (nonatomic,copy) NSString *authorName;
@property (nonatomic,copy) NSString *bookName;

@end</pre>  
.m文件  
<pre>//
//  Book.m
//  Book
//
//  Created by Renhuachi on 16/3/22.
//  Copyright © 2016年 software. All rights reserved.
//

＃import "Book.h"

@implementation Book

- (id)initWithCoder:(NSCoder *)aDecoder {
    if (self = [super init]) {
        _authorName = [aDecoder decodeObjectForKey:@"authorName"];
        _bookName = [aDecoder decodeObjectForKey:@"bookName"];
    }
    return self;
}

- (void)encodeWithCoder:(NSCoder *)aCoder {
    [aCoder encodeObject:_authorName forKey:@"authorName"];
    [aCoder encodeObject:_bookName forKey:@"bookName"];
}
@end</pre>

### 2.利用NSUserDefaults存储自定义类  
首先创建一个Book类型的对象，并将其归档为NSDate类型，然后存储到NSUserDefaults中。  
<pre>Book *book = [[Book alloc]init];
book.authorName = @"书名";
book.bookName = @"作者";

NSData *data = [NSKeyedArchiver archivedDataWithRootObject:book];
NSUserDefaults *userDefaults = [NSUserDefaults standardUserDefaults];
[userDefaults setObject:data forKey:@"book"];</pre>  
从NSUserDefaults中还原数据也很简单。  
<pre>NSUserDefaults *user = [NSUserDefaults standardUserDefaults];
NSdData *data = [user objectForKey:@"book"];  
Book *book = [NSKeyedUnarchiver unarchiveObjectWithData:data];</pre>