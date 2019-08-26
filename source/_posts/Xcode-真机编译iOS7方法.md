---
title: Xcode-真机编译iOS7方法
date: 2017-05-15 21:00:30  

categories:
- XCode 工具
tags:
- 工具
- XCode
- iOS7
---
![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Xcode-%E7%9C%9F%E6%9C%BA%E7%BC%96%E8%AF%91iOS7%E6%96%B9%E6%B3%95/ios-7-logo.png)  
## 真机编译iOS7的方法  
步骤炒鸡简单：   
1. 将iOS7.0/iOS7.1的SDK放到Xcode这个目录下(iOS7 SDK请百度自行下载)：  
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport；
2. 进入目录：  
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk  打开“iPhoneOS.sdk”文件夹和“SDKSettings.plist”文件的读写权限；  
3. 打开“SDKSettings.plist”文件，按图添加7.0/7.1（⚠️一定要按照从小到大的顺序添加）然后重启Xcode就可以了。  
![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Xcode-%E7%9C%9F%E6%9C%BA%E7%BC%96%E8%AF%91iOS7%E6%96%B9%E6%B3%95/iOS7_debug.png)
