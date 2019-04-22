---
layout:     post   				    # 使用的布局（不需要改）
title:      Docker初探				# 标题 
subtitle:   初步学习Docker及相关知识 #副标题
date:       2019-04-18				# 时间
author:     Haiming 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - 编程
    - Docker
    - 记录
    - 学习
---
# 1. 什么是Docker
Docker的英文定义是：Docker is a computer program that performs operating-system-level virtualization.

在阅读了资料之后，我认为Docker最开始的版本是一个使用了Linux Kernel的特性，例如cgroups和namespaces，来进行资源虚拟和封装的软件。

## 1.1 Cgroups
先上维基百科定义：cgroups (abbreviated from control groups) is a Linux kernel feature that limits, accounts for, and isolates the resource usage (CPU, memory, disk I/O, network, etc.) of a collection of processes.

即：cgroups是一个Linux Kernel的特性，可以限制，管理和独立一系列进程对于资源的使用（CPU，memory，disk I/O，网络，等等）