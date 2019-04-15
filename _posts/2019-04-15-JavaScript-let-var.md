---
layout:     post   				    # 使用的布局（不需要改）
title:      浅析JavaScript之中的let和const			# 标题 
subtitle:    #副标题
date:       2019-04-15 				# 时间
author:     Haiming 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - 编程
    - 日常
    - 记录
    - 学习
    - JavaScript
---
在ES6之中，新增了let和const的变量形式，在阅读了[阮一峰老师的ES6](http://es6.ruanyifeng.com/#docs/let)教程之后，做一点基础的分析和总结。

先上一张表格：


|声明方式|变量提升|暂时性死区|重复声明|初始值|作用域|
|--------|--------|----------|----|---|---|
|var|允许|不存在|允许|不需要|除块级|
|let|不允许|存在|不允许|不需要|块级|
|const|不允许|存在|不允许|需要|块级|


# 1. Let
## 1.1 基本用法

