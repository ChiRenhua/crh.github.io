---
title: Hexo常用命令整理
date: 2016-08-27 19:11:39

categories:
- 网站搭建
tags:
- 命令
---
![](https://raw.githubusercontent.com/ChiRenhua/Resource/master/WebImage/Hexo常用命令整理/hexo.jpg)  

## 安装
安装  
``npm install hexo -g``  
升级  
``npm update hexo -g``  
初始化  
``hexo init``

<!-- more -->

## 发表文章
新建文章  
``hexo n "我的博客" == hexo new "我的博客"``  
发表草稿  
``hexo p == hexo publish``  
生成静态文件  
``hexo g == hexo generate``   
启动服务预览  
``hexo s == hexo server``   
部署到远程  
``hexo d == hexo deploy``
## 清理缓存  
清除缓存文件 (db.json) 和已生成的静态文件 (public)  
``hexo clean``
## 版本
查看Hexo运行版本  
``hexo version``


