---
title: CocoaPods-创建私有仓库
date: 2018-05-17 17:00:09

categories:
- CocoaPods
tags:
- 类库管理
- 工具
- Xcode
- CocoaPods  
---
![cocoaPods logo](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/安装CocoaPods过程以及踩过的坑/cocoapods-stickers.jpg)  
## 前言
之前有介绍过如何安装 CocoaPods（[**传送门**](http://chirenhua.com/2016/08/27/%E5%AE%89%E8%A3%85CocoaPods%E8%BF%87%E7%A8%8B%E4%BB%A5%E5%8F%8A%E8%B8%A9%E8%BF%87%E7%9A%84%E5%9D%91/)），对于CocoaPods的好处自然也不必多说。然而有些情况下，我们想要管理自己的库，但还不想把这些库开源，这时就需要构建一个私有的 CocoaPods 仓库。  

在前面安装 CocoaPods 的教程中有提到：pod setup 的本质就是将 https://github.com/CocoaPods/Specs 上的 Specs 项目 clone到 /Users/ 用户名 /.cocoapods/repos 目录下。同理，我们在 git 上创建一个私有的仓库，用于存储私有的库文件，然后将这个私有仓库添加到 CocoaPods 中即可。

## 1. 在 Git 上创建私有仓库
个人开发者可以在 GitHub 上购买一个私有仓库，公司的话一般都会有自己内部的 Git 平台。这里用GitHub举例，创建一个私有仓库用于存放私有的库文件:   

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/CocoaPods-%E5%88%9B%E5%BB%BA%E7%A7%81%E6%9C%89%E4%BB%93%E5%BA%93/cocoaPods01.png)  

创建完成后，在终端执行以下命令，将新创建的 MartinSpecs 仓库添加到 CocoaPods 中。（ MartinSpecs 是仓库名称，后面的域名是仓库地址）  

``pod repo add MartinSpecs https://github.com/ChiRenhua/MartinSpecs``  

至此我们第一步就完成了，来到 .cocoapods 目录下，我们可以看到刚刚创建的 MartinSpecs 仓库已经添加到了 CocoaPods 中了。  

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/CocoaPods-%E5%88%9B%E5%BB%BA%E7%A7%81%E6%9C%89%E4%BB%93%E5%BA%93/cocoaPods02.png)

## 2. 创建 CocoaPods 库文件
这一步需要按照 CocoaPods 的标准对库文件进行包装。  

**1. 在 GitHub 上创建一个新的项目**  
![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/CocoaPods-%E5%88%9B%E5%BB%BA%E7%A7%81%E6%9C%89%E4%BB%93%E5%BA%93/cocoaPods03.png)  

要注意 License 文件是一定要有的，因为CocoaPods要求 Pods 依赖库必须要有 License 文件，否则会导致验证失败。  

**2. 创建并配置 .podspec 文件：**  
将项目 clone 到本地，并在相应目录下通过以下命令创建 .podspec 文件  

``pod spec create MartinPodsTest``  

打开 MartinPodsTest.podspec 文件（这是个 ruby 类型的文件，“#” 代表被注释，所以这里大部分是没用的注释），删掉没用的注释，我们把重要的参数配置下即可。  

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/CocoaPods-%E5%88%9B%E5%BB%BA%E7%A7%81%E6%9C%89%E4%BB%93%E5%BA%93/cocoaPods08.png)  

**3. 发布 release 版本**  
至此库文件已经配置完毕，接下来要在 GitHub 上发布一个 release 版本即可。  
在相应目录下执行以下命令：  

``git tag '0.0.3'``  
``git push --tags``  
``git push``  

在 GitHub 上已经可以看到刚刚发布的 realse 版本了！  
![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/CocoaPods-%E5%88%9B%E5%BB%BA%E7%A7%81%E6%9C%89%E4%BB%93%E5%BA%93/cocoaPods09.png)

## 3. 添加到 CocoaPods 私有仓库中
**1. 验证库**  
通过命令 ``pod lib lint`` 来验证依赖库是否合法，如果合法就可以添加到 CocoaPods 的仓库中了。
如果出现了无关紧要的警告⚠️，如下图：  

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/CocoaPods-%E5%88%9B%E5%BB%BA%E7%A7%81%E6%9C%89%E4%BB%93%E5%BA%93/cocoaPods04.png)    

可以通过下面的命令规避这些警告。  

``pod lib lint --allow-warnings``

**2. 添加到 CocoaPods 私有仓库**  
如果验证通过会提示：  

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/CocoaPods-%E5%88%9B%E5%BB%BA%E7%A7%81%E6%9C%89%E4%BB%93%E5%BA%93/cocoaPods05.png)  

验证通过后通过下面的命令将项目提交到私有仓库中  

``pod repo push MartinSpecs MartinPodsTest.podspec``   

如果因为警告原因 push 失败，可以在 push 后添加 ``--allow-warnings``    

``pod repo pus --allow-warnings MartinSpecs MartinPodsTest.podspec``  

添加成功后就可以看到库文件已经被添加到私有仓库中了。  

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/CocoaPods-%E5%88%9B%E5%BB%BA%E7%A7%81%E6%9C%89%E4%BB%93%E5%BA%93/cocoaPods07.png)  

尝试搜索下刚刚新加的库，已经可以看到了！  

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/CocoaPods-%E5%88%9B%E5%BB%BA%E7%A7%81%E6%9C%89%E4%BB%93%E5%BA%93/cocoaPods06.png)

## 4. 在项目中引用依赖库
创建一个新的项目，在项目目录下执行  

``pod init``  

执行完毕后会在目录中添加 Podfile 文件，修改 Podfile 文件，如图：  

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/CocoaPods-%E5%88%9B%E5%BB%BA%E7%A7%81%E6%9C%89%E4%BB%93%E5%BA%93/cocoaPods10.png)  

**1. 添加两个 source 目录，对应官方的仓库地址和私有的仓库地址，一定要两个都加上，这样在搜索库时，会在两个目录下同时搜索。**    

``source 'https://github.com/CocoaPods/Specs.git'           #官方仓库``
``source 'https://github.com/ChiRenhua/MartinSpecs.git'     #私有仓库``  

**2. 添加 MartinPodsTest 库（顺便再加个 AFNetworking 来验证下公有库是否正常运行）**  

``pod 'MartinPodsTest'``  

**3. 最后执行 install 命令安装 MartinPodsTest 库**  

``pod install``  

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/CocoaPods-%E5%88%9B%E5%BB%BA%E7%A7%81%E6%9C%89%E4%BB%93%E5%BA%93/cocoaPods11.png)  

一切正常！完美。
## 5. 结语
这样我们就拥有一个私有的 CocoaPods 仓库了，可以将项目组件化，然后通过 CocoaPods 来管理。