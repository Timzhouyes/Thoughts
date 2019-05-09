---
layout:     post   				    # 使用的布局（不需要改）
title:      JavaScript基础学习（1）			# 标题 
subtitle:    以GitHub上一个项目为基础                 #副标题
date:       2019-05-09 				# 时间
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

作为一个前端，要是不会 JavaScript ，那就是个笑话。
不说了，开冲

# 01-JS简介

#### JavaScript 的组成

JavaScript 分为三个部分：

- ECMAScript： JavaScript 的语法标准。
- DOM：文档对象模型，操作**网页上的元素**，例如让盒子移动，变色，轮播图
- BOM：浏览器对象模型，操作**浏览器部分功能** 的API，比如自动滚动浏览器。

#### alert 和 prompy 的区别

```javascript
	alert("从前有座山");                //直接使用，不需要变量
	var a = prompt("请输入一个数字");   // 必须用一个变量，来接收用户输入的值
```

# 02-变量

