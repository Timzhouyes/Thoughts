---
layout:     post   				    # 使用的布局（不需要改）
title:      《Spring Boot 42讲》学习笔记（2） 				# 标题 
subtitle:   构建基本项目  #副标题
date:       2019-04-24				# 时间
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



# 第2-1课：Spring Boot对基础Web开发的支持（上）



Spring Boot对于Web的开发的支持很全面，包括开发，测试和部署都有支持。Spring-boot-starter-wew是spring Boot对于Web开发提供支持的组件，其主要包括RESTful，参数校验，使用Tomcat作为内嵌容器。下面是介绍:



#### JSON的支持



JSON(JavaScript Object Notation) 是一种轻量级的数据交换格式，易于阅读和编写。JSON的出现，改变了早期人们使用XML进行数据交互的方式。在现在的Spring Boot之中，在Web层的使用只需要加入一个注解就可以。



在`pom.xml` 之中加入以下代码：



```xml
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-web</artifactId>
</dependency>
```



> 在本教程中，“根目录”所指为Main()所在程序的目录。



