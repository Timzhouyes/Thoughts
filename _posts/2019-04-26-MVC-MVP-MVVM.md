---
layout:     post   				    # 使用的布局（不需要改）
title:      浅谈MVC，MVP和MVVM 				# 标题 
subtitle:   对重点和差异点做一些分析  #副标题
date:       2019-04-26				# 时间
author:     Haiming 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - 编程
    - MVC
    - 记录
    - 学习
    - MVVM
	- MVP
	- 软件架构
---

在寻找Angular教程的过程之中，发现网上有声音认为Angular是MVVM，突然觉得自己对于这些架构的区别还不够明晰，因此在这里系统的学习和总结一下，MVC，MVP，MVVM三者之间的区别和重点。


# 1 . MVC



MVC，也就是 Model, View, Controller 三层。

[谈谈MVC模式](http://www.ruanyifeng.com/blog/2007/11/mvc.html)，下面是对于M，V，C的具体介绍：



>1）最上面的一层，是直接面向最终用户的"视图层"（View）。它是提供给用户的操作界面，是程序的外壳。
>
>2）最底下的一层，是核心的"数据层"（Model），也就是程序需要操作的数据或信息。
>
>3）中间的一层，就是"控制层"（Controller），它负责根据用户从"视图层"输入的指令，选取"数据层"中的数据，然后对其进行相应的操作，产生最终结果。



