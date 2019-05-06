---
layout:     post   				    # 使用的布局（不需要改）
title:      《Spring Boot 42讲》学习笔记（4） 				# 标题 
subtitle:   构建RESTful API服务  #副标题
date:       2019-05-06				# 时间
author:     Haiming 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - 编程
    - Spring Boot
    - 记录
    - 学习
    - Java
---

# 第2-8课：Spring Boot 构建一个 RESTful Web 服务

RESTful 的核心概念是“资源”，从 RESTful 的角度来看，网络之中，无论一段文本，图片，歌曲，服务等等都对应一个特定的URI，并且用URI进行表示，访问这个URI就可以得到这个资源。

资源可以有多种表现形式，也就是资源的“表述（Representation）。

此处教程所举的例子是一张图片，既可以是 JPEG 格式，也可以是 PNG 格式，但我个人认为不是这样。下面是个人对于“表述”的观点：



