---
layout:     post   				    # 使用的布局（不需要改）
title:      Daily record (Angular)				# 标题 
subtitle:   To study Angular #副标题
description: Try it now
date:       2019-03-19 				# 时间
author:     Haiming 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - 编程
    - 日常
    - 记录
    - 学习
    - Angular
---
# 0. 今日工作遇到问题
1.   mongoDB要在mongoDB的Bin文件夹下面开，而不是在所要开启的项目文件夹下面开启
2.   
# 1. 梳理一下基本的Angular传值步骤
  如何在Angular之中做到同步传值？按照之前所说，Angular是双向绑定，因此只要在程序之中说明其需要什么就好。
  ```<input type="xxx" ng-model="this.maxPrice" /> ```
  这句话指定了输入的类型（xxx）和 maxPrice 这个变量，那么只要在contoller.js之中定义一个一样的变量(this.maxPrice) ，然后对其进行操作就可以实时的同步view和model的值。
  
  

# 2.什么是Owin
Owin全程是Open Web Interface for .NET.个人理解其就是一个中间件，是用来使 .NET 的程序脱离某个具体的服务器环境来设置的。
我们都知道Web应用程序是运行在Web服务器之中的，应用程序的请求接收和数据发送都是通过web
服务器，这样就给应用程序的开发造成了一些困难。这种情况之下，如果可以将Web程序对于服务器的要求统一抽象出来，针对这个抽象接口编程，那么会容易很多。

Owin就是这样的一个接口。