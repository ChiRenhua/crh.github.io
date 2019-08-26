---
title: Travis-CI 自动化部署网站
date: 2018-10-30 21:42:47

categories:
- 网站搭建
tags:
- Travis-CI
---
## Travis-CI简介
![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Travis-CI%20%E8%87%AA%E5%8A%A8%E5%8C%96%E9%83%A8%E7%BD%B2%E7%BD%91%E7%AB%99/Travis-CI.png)

Travis CI是一个托管的，分布式的持续集成服务，用于构建和测试在GitHub上托管的软件项目。

利用 Travis CI ，可以实现每次 push 新文章到 git 后，Travis CI 会自动检测到提交，然后根据配置文件 “.travis.yml”，按照设定好的步骤帮我们自动部署网页。

<!-- more -->

## 实现
### 1.Travis CI 关联 GitHub
下面的步骤都是在已经成功搭建好了 GitPages 网站的基础上进行的。
使用 GitHub 账户登录 [Travis CI](https://travis-ci.org) ，Travis CI 会自动加载 GitHub 账户下的所有仓库，选择 ``yourname/yourname.github.io`` 项目进行关联。

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Travis-CI%20%E8%87%AA%E5%8A%A8%E5%8C%96%E9%83%A8%E7%BD%B2%E7%BD%91%E7%AB%99/Travis-CI01.png)

点击 “setting” 进入设置页面，在 General 下打开 “Build pushed branches” 选项。

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Travis-CI%20%E8%87%AA%E5%8A%A8%E5%8C%96%E9%83%A8%E7%BD%B2%E7%BD%91%E7%AB%99/Travis-CI02.png)

### 2.配置 Travis CI
Travis CI 在部署网页时，需要于之关联的 GitHub 上仓库的读写权限。GitHub 提供了 Token 方式供外部访问对应的仓库。

进入到 GitHub 的[设置](https://github.com/settings/tokens)页面，创建一个新的 Token 。注意要勾选对应的权限。创建完毕后，务必要记下 Token 的值，下个步骤要用到，这个值只会在创建完展示一次，所以一定要记好。

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Travis-CI%20%E8%87%AA%E5%8A%A8%E5%8C%96%E9%83%A8%E7%BD%B2%E7%BD%91%E7%AB%99/Travis-CI03.png)

回到 Travis CI 的设置页面，在 Environment Variables 选项下，添加我们刚刚在 GitHub 中生成的 Token 。

![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Travis-CI%20%E8%87%AA%E5%8A%A8%E5%8C%96%E9%83%A8%E7%BD%B2%E7%BD%91%E7%AB%99/Travis-CI04.png)

### 3.创建  .travis.yml 文件
 .travis.yml 文件作用是告诉 Travis CI 在监听到 Push 操作后都需要做什么。在博客的根目录下创建 .travis.yml 文件，文件示例如下：
 
 ```
# 指定语言环境
language: node_js
# 指定需要sudo权限
sudo: required
# 指定node_js版本
node_js: 
  - 7.9.0
# 指定缓存模块，可选。缓存可加快编译速度。
cache:
  directories:
    - node_modules

# 指定博客源码分支，因人而异。hexo博客源码托管在独立repo则不用设置此项
branches:
  only:
    - hexo 

before_install:
  - npm install -g hexo-cli

# Start: Build Lifecycle
install:
  - npm install
  - npm install hexo-deployer-git --save

# 执行清缓存，生成网页操作
script:
  - hexo clean
  - hexo generate

# 设置git提交名，邮箱；替换真实token到_config.yml文件，最后depoy部署
after_script:
  - git config user.name "yourName"
  - git config user.email "yourEmail"
  # 替换同目录下的_config.yml文件中gh_token字符串为travis后台刚才配置的变量，注意此处sed命令用了双引号。单引号无效！
  - sed -i "s/gh_token/${GH_TOKEN}/g" ./_config.yml
  - hexo deploy
# End: Build LifeCycle
 ```
 
 同时，还要修改 _config.yml 文件的 deploy 节点：
 
 ```
deploy:
- type: git
  # 下方的gh_token会被.travis.yml中sed命令替换
  repo: https://gh_token@github.com/xiong-it/xiong-it.github.io.git
  branch: master
 ```
 
 最后将 yml 文件 push 到 GitGub 上就可以在 Travis CI 上看到构建了。
 
 ![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Travis-CI%20%E8%87%AA%E5%8A%A8%E5%8C%96%E9%83%A8%E7%BD%B2%E7%BD%91%E7%AB%99/Travis-CI05.png)
 
## 最后
以后在发布新博客，只需要将写好的博客放到 hexo/source/_post/ 文件夹下，Push 到 GitHub 上即可，后续的的构建操作 hexo clean、hexo g、hexo d 都会由 Travis-CI 自动完成。


