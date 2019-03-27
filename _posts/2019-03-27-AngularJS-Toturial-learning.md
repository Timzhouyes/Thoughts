---
layout:     post   				    # 使用的布局（不需要改）
title:     学习AngularJS（一）：PhoneCat 与3月27日日常记录			# 标题 
subtitle:   官方Toturial的一点总结和体会，版本1.3.16 #副标题
date:       2019-03-27				# 时间
author:     Haiming 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - 编程
    - 学习
    - AngularJS
---

[教程地址在这](https://code.angularjs.org/1.3.16/docs/tutorial),此处放的是1.3.16版本

# 1. 初始化环境的准备

## 1.1 npm install有什么作用？

npm install这个命令读取了angular-phonecat的package.json文件（因此有很多时候提示package.json文件缺失报错），并且把以下的工具下载到```node_modules```目录之中。
1. Bower-客户端代码包管理器
2. Http-Server-本地静态Web服务器
3. Karma-单元测试运行器
4. Protractor-端到端测试运行器

之后可以使用```npm start```来启动一个本地Web服务器，```npm test```来启动Karma单元测试运行器

## 1.2 环境都去哪配置？

可以编辑package.json内部的"start"脚本，

1. 使用’-a‘ 设置地址（address）
2. 使用’-p‘设置端口（port）


## 1.3 在哪测试？

测试部分在karma之中，使用```npm test```来测试。

```karma.conf.js```是karma进行测试的初始配置文件。

下面对于```karma.conf.js```之中的字段进行解释：

1. ```module.exports = function(config)```：这部分指向一个接受一个参数的function
2. ```basePath```： 对于所有的```files```和```exclude```之中的文件路径的初始定义。
3. ```autoWatch```：Default是true,决定当文件有改动时候是否自动执行测试

若其在后台一直运行，可以针对代码操作进行实时反馈。

# 2. Bootstrapping

## 2.1 ```ng-app```

```<html ng-app>```

用来规定AngularJS的作用范围。可以选择整个页面或者某个部分。

## 2.2 AngularJS script tag

用于下载angular.js， 之后将其作为bootstrap应用到```ngApp```的范围之内。

## 2.3 Double-curly binding with an expression

双大括号里面的值在绑定之后替换回原来双大括号的位置。

## 2.4 Bootstrapping AngularJS apps

通常使用```ngApp```来自动做Bootstrap。

当Bootstrap完成之后，其会等待操作例如点击鼠标，若操作对model做出改变，则直接更新所有被影响的Binding

# 3. Static Template
1. 所有的操作，类似于```ng-repeat```和```ng-controller```,都是写在标签里面的。标签的范围也就是操作起作用的范围。
2. <ul>之中包含<li>, 其中<li>是<ul>的一个项。每一个<li>前面都会有<ul>的那个点。




# 3月27日日常记录

### 1. What is the difference between Authentication and Authorization?

Authentication的翻译为认证，而Authorization的翻译是授权。

简而言之，Authentication意思是”确定你是谁“，而Authorization的意思是”确定你有什么权限“

# 一些小Tips
1. 建议把脚本放在 <body> 元素的底部。这会提高网页加载速度，因为 HTML 加载不受制于脚本加载。
- 此处的脚本为```<script src="https://cdn.staticfile.org/angular.js/1.4.6/angular.min.js"></script>```


