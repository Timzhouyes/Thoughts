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



阮老师在这篇文章之中还有提到，他认为软件架构的方向就是解耦，是模块化，各个服务之间通过接口相互链接，这样对于系统的部分升级会容易很多。刚巧，前几天看了陈皓的一篇博文[STEVEY对AMAZON和GOOGLE平台的吐槽](https://coolshell.cn/articles/5701.html)，其中提到了STEVEY认为Amazon只有三点做的比Google强，而最强的一点，是其将整个架构服务化，每个团队对外只是提供API，提供自己这部分的服务，这才是转型成为一个平台的标准。



下面是阮一峰老师的理解：

MVC之中的通信通路如下：

1. View传送指令到Controller
2. Controller完成业务逻辑之后，要求Model改变状态
   - 注意这里面的Model不仅仅是指数据库，而是指核心业务的状态
3.  Model将新的数据发送给View，用户得到反馈。

**所有通信都是单向的**



但是在其评论区，我发现大家对于MVC有很多自己的想法。很多人认为上面提到是前端的MVC，在Java之中，MVC的通信方向应该是 V->C->M->C->V

 



# 2.互动模式



接受用户指令之时，MVC可以分成两种模式：

- 一种是通过View接受指令，传递给Controller，和上面所涉及到的一样
- 一种是直接通过Controller接收指令：
  - 用户可以直接给Controller发送指令，类似于改变URL从而触发事件，再由Controller进行处理，最后由Model发送给View。



# 3. MVP

MVP模式之中，将Controller改名为Presenter，也改变了通信方向。



在MVP之中，Presenter是居于中间位置的，其既可以和View进行双向通信，也可以和Model进行双向通信。而Model和View之间是不发生联系的。

