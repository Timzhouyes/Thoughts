---
layout:     post   				    # 使用的布局（不需要改）
title:      《网络是怎样连接的》书摘与笔记（一）				# 标题 
subtitle:   网络是怎样连接的 #副标题
description: Try it now
date:       2019-04-03 				# 时间
author:     Haiming 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - 编程
    - 日常
    - 记录
    - 学习
    - 网络
---

今天开始看《网络是怎样连接的》，有所感想的地方，记录在此，以备复习。

# 1. 浏览器生成消息——探索浏览器内部
## 热身问答
1. 浏览器等网络应用程序并不具备网络控制功能，而是委托操作系统来控制。

## 本章内容
1. 生成HTTP请求信息
2. 向DNS服务器查询Web服务器的IP地址
3. DNS服务器的接力查询
4. 委托协议栈（操作系统之中）发送消息

## 1.1 生成HTTP请求消息
### 1.1.1 探索之旅从输入网址开始
#### 1.1.1.1 什么是URL？
URL：Uniform Resource Locator，统一资源定位符。
