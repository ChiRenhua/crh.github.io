---
title: WWDC-2017笔记—Vision
date: 2017-09-07 16:55:42
categories:
- iOS 开发总结
tags:
- iOS
- WWDC
---
![Vision Logo](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/2017%20WWDC%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0%E2%80%94Vision/Vision.jpg)  
# Vision
Vision是iOS11发布的一个新的框架，可以用来对图片信息进行处理。其内部集成了Core ML，可以高效的返回图片中的信息。同时开发者也可以在Vision中集成自己的Core ML Model，所以其灵活性非常高。 
## 应用场景 
目前Vision可以支持的场景有：  
**1. 人脸识别（给出人脸所在的矩形区域信息）**  
**2. 人脸面部特征识别（给出眼睛、鼻子、嘴以及人脸中轴线的位置信息）**  
**3. 文字识别（给出文字所在的矩形区域信息）**  
**4. 二维码识别（识别二维码并返回二维码的信息）**  
**5. 目标追踪（对于运动的目标，实时返回其位置信息）**  
## 集成
Vision使用起来非常简单，只需三步就可将Vision集成到项目中：  
### 1. 初始化VNDetctRequest
根据使用场景的不同，可以选择初始化不同的Request，以人脸识别为例：  
``
VNDetectFaceRectanglesRequest * faceRequest = [[VNDetectFaceRectanglesRequest alloc] init];
``  
这里初始化了“VNDetectFaceRectanglesRequest”，表示希望返回人脸所在区域的信息。  
### 2. 初始化VNImageRequestHandler  
用于承接数据源，目前支持的数据源类型有：Data、URL、CGImage、CIImage、CVPixelBuffer。以CGImage为例：
``
VNImageRequestHandler *requestHandler = [[VNImageRequestHandler alloc] initWithCGImage:imageRef options:@{}];
``  
### 3. 发起请求  
调用“handler”下的“performRequests”方法开始进行识别，“performRequests”的参数是数组类型，可以同时传递多个“request“给“handler”一起处理。
``
[requestHandler performRequests:@[faceRequest] error:&error];
``  
识别完成后通过“request.results”来拿到识别结果，"results"是数组类型，如果为空则表示当前图片中没有人脸的信息。  
## Demo  
利用Vision，我们可以做一些有趣的事情，比如为视频添加挂件，效果如下图：  
![Vision video](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/2017%20WWDC学习笔记—Vision/Visionvideo.gif)   
具体代码已经上传git：[视频挂件](https://github.com/ChiRenhua/VisionDemo)