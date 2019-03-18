---
layout:     post   				    # 使用的布局（不需要改）
title:      Angular study				# 标题 
subtitle:   To study Angular #副标题
date:       2019-03-18 				# 时间
author:     Haiming 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - 编程
    - 日常
    - 记录
    - 学习
    - Angular
---

# 1. What is Angular
Angular is a framework for people to do the data-binding and changes on website of UI more easily.

AngularJS is the 1st edition of Angular.

#### The difference between AngularJS and Angular is that
1. Angular is based on TypeScript and Angular is based on TypeScript, which is a opensourced language developed by Microsoft.
2. AngularJs uses terms of **Scope** and **Controller** .AngularJS also has a **rootScope** for whole application
- My own understand of **Scope** is a link between controller and view.When we add a *$scope* in controller, HTML file can get these attributes directlhy and just use it .
- The difference between one-way data binding and two-way databinding is that one-way data binding only can get data from model and post them on the Views, but if we use two-way databinding, if we change data on Views, it can also change data in models. So one is from model to view, one is two-directions view and models.

# 2. Study Angular toturial on its own website
1. Install node.js and npm ,after that we can set a workspace,which is the place for one or more projects. 
2. Use the 
> npm new my-app 
to get a new workspace, an initial skeleton app project, an end-to-end test project and one related configuration files.
3. After new a project, Angular already includes a server, which we can use to build and serve or test the app locally.
- Just use the command 'ng serve --open' and it will open the test website. Default is http://localhost:4200
4. **Components** are fundamental binding blocks of Angular applications.What the "fundemental" means is that it is the basic unit of whole program.Such as 
- display data on the screen
- listen for user input
- take action based on that input
5. so after new a project, CLI already created the first component for us.
# 3. Learn Tours of Heroes toturial
It is said on the website that the toturial can provide functions below:
- Acquiring and displaying a list of items

- Editing a selected item's detail

- Navigating among different views of the data

So I start learning it right now.
