---
layout:     post   				    # 使用的布局（不需要改）
title:      《Spring Boot 42讲》学习笔记（1） 				# 标题 
subtitle:   从Hello world开始……  #副标题
date:       2019-04-23				# 时间
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



# 第 1-1 课：Spring Boot产生的背景及其设计理念



Spring是Rod Johnson在不满当时的Java EE和EJB过于臃肿，不是所有的项目都需要这种大型框架的情况下自行开发的。在其interface21一炮走红之后，一对一的J2EE设计和开发开始流行起来。2003年，其和同伴在这个基础上开发了一个全新的框架，命名为Spring。随后Spring的发展进入了快车道。



## 到底啥是J2EE？



> #### J2EE(Java 2 Platform Enterprise Edition) 企业版
> J2EE是功能最丰富的一个版本，主要用于开发高访问量、大数据量、高并发量的网站，例如美团、去哪儿网的后台都是J2EE。
> 通常所说的JSP开发就是J2EE的一部分。
> J2EE包含J2SE中的类，还包含用于开发企业级应用的类，例如EJB、servlet、JSP、XML、事务控制等。
> J2EE也可以用来开发技术比较庞杂的管理软件，例如ERP系统（Enterprise Resource Planning，企业资源计划系统）。



下面贴一个自己觉得有用的知乎回答：



> Java各种标准的制定是通过Java Community Process - JCP进行的。JCP的成员可以根据需要提出Java Specification Request - JSR。每个JSR都要经过提交给JCP，然后JCP讨论，修订，表决通过，并最终定稿；当然，运气不好的就被拒绝/废弃了。
> **而JavaEE是一组被通过的JSR的合集**。JSR和JavaEE的关系就好像是文章和经过编辑整理过的文集。
> 作者：大宽宽
>
> 链接：https://www.zhihu.com/question/268742981/answer/341770209
>
> 来源：知乎
>
> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。





## Spring Boot的诞生



 