---
title: 安装CocoaPods过程以及踩过的坑
date: 2016-08-27 20:18:29

categories:
- CocoaPods
tags:
- 类库管理
- 工具
- Xcode
---
![cocoaPods logo](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/安装CocoaPods过程以及踩过的坑/cocoapods-stickers.jpg)  
## 为什么要用到CocoaPods
&emsp;&emsp;以往在写iOS程序的时候，遇到好的第三方开源库都是将开源库下载下来，拷贝到项目中，然后再去使用，而且遇到开源库更新，我们还要重新执行一边操作。如果开源库数量很多，这个操作就显得非常麻烦了。  
&emsp;&emsp;现在有了CocoaPods，这些问题就都迎刃而解了，CocoaPods可以帮助我们下载，管理第三方的开源库，包括更新、系统依赖和编译参数等都能帮助我们处理。简直不能再方便，设置完CocoaPods后，以后我们只需要专心的敲代码就好了。  
## CocoaPods的原理
&emsp;&emsp;CocoaPods是将所有的依赖库放到另一个名为Pods的项目中，然后让主项目依赖Pods项目，这样，源码管理工作都从主项目移到了Pods项目中。Pods项目最终会编译成一个名为libPods.a的文件，主项目只需要依赖这个.a文件即可。
## 安装CocoaPods
&emsp;&emsp;现在Mac上似乎已经自带了Ruby，如果没有的话可以通过“[如何在Mac OS X上安装 Ruby运行环境](http://www.cnblogs.com/daguo/p/4097263.html)”这篇文章来安装。安装完ruby后，打开Terminal终端，输入以下命令：  
&emsp;&emsp;``$ sudo gem install cocoapods``（注意：在执行这条命令前请保证你的gem为最新版）  
&emsp;&emsp;然后执行安装操作，输入命令：  
&emsp;&emsp;``$ pod setup``  
&emsp;&emsp;此时Terminal终端会停留在“Setting up CocoaPods master repo”这个状态一段时间，因为这个时候是在下载安装开源库的文件，安装时间的长短由你的网速等决定。注意，在这里是一个很大的坑，我在这里折腾了一整天，最后才弄好。为了节省大家时间我直接说我的解决办法。
&emsp;&emsp;如果你也是用了很多办法都下不来安装文件，或者下载一会儿就报错要重下的话，那么可以参考下这个办法：  
&emsp;&emsp;1. 访问 [https://github.com/CocoaPods/Specs](https://github.com/CocoaPods/Specs)，然后将Specs项目fork到自己的github账户上。  
&emsp;&emsp;2. 下载GitHub Desktop, 然后clone Specs项目。  
&emsp;&emsp;3. 将clone的Specs项目的文件夹改名为master，然后拖到/Users/用户名/.cocoapods/repos目录下。  
&emsp;&emsp;4. 运行pod setup  
&emsp;&emsp;pod setup的本质就是将[https://github.com/CocoaPods/Specs](https://github.com/CocoaPods/Specs)上的Specs项目clone到/Users/用户名/.cocoapods/repos目录下。若此目录下已有Specs项目，则会将项目更新到最新的状态。由于Specs很大，容易导致pod setup失败。这时就需要我们手动安装Specs。若直接从github上下载zip文件，由于缺少git文件，会导致cocoa pods不使用。若用git clone，由于文件过大，容易导致失败。但是使用GitHub Desktop软件，则会提高clone的成功率，并且会给出clone的进度，但是我也是下了三遍最后才clone下来。
## 使用CocoaPods
&emsp;&emsp;废了好大劲安装完毕后，就是如何使用了，举个栗子：假如说我们要在项目中导入非常火的“AFNetworking”类库，我们先要确定AFNetworking是否支持CocoaPods，所以我们先利用CocoaPods的搜索功能搜索下，在Terminal终端上输入以下命令：  
&emsp;&emsp;``$ pod search AFNetworking``  
&emsp;&emsp;如果出现错误：“[!] Unable to find a pod with name, author, summary, or description matching AFNetworking”，可以通过执行下面的命令来解决：  
&emsp;&emsp;``rm ~/Library/Caches/CocoaPods/search_index.json``  
&emsp;&emsp;如果执行成功，会出现“AFNetworking”的相关信息，如下图。  
![searchResult](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/安装CocoaPods过程以及踩过的坑/searchResult.png)
&emsp;&emsp;这说明“AFNetworking”是支持CocoaPods的。接下来我们就可以将“AFNetworking”导入到我们的项目中去。  
&emsp;&emsp;首先，我们通过Terminal终端进入到们已经创建好的项目目录下，执行下面的命令：  
&emsp;&emsp;``$ vim Podfile``  
&emsp;&emsp;这个命令的作用是在项目目录下生成一个名为“Podfile”的文件，这个文件的作用就是用来告诉CocoaPods需要下什么类库，然后我们在Podfile文件中添加以下文字：（注意：不要用文本编辑器进行编辑！）  
<pre>&emsp;&emsp;platform :ios, '9.0'  
&emsp;&emsp;&emsp;&emsp;target 'MyApp' do  
&emsp;&emsp;&emsp;&emsp;pod 'AFNetworking', '~> 3.1'  
&emsp;&emsp;end</pre>  
&emsp;&emsp;然后保存退出，这时候在项目的目录里就会出现一个名为Podfile的文件，然后我们执行以下命令来将“AFNetworking”下载到项目中。（注意：下面的命令需要在项目目录中执行。）  
&emsp;&emsp;``$ pod install``  
&emsp;&emsp;执行成功后，项目中会多出来一个.xcworkspace类型的文件，如下图：  
![project](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/安装CocoaPods过程以及踩过的坑/project.png)  
&emsp;&emsp;以后需要用它来打开工程才会配置类库的相关文件。  
&emsp;&emsp;以上就是CocoaPods的安装和使用方法介绍，如果想要了解更多的使用方法，可以参考[官方文档](https://github.com/CocoaPods/CocoaPods/wiki)。
