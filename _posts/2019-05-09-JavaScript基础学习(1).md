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

<https://github.com/qianguyihao/Web/wiki>

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

在 JS 之中是大小写敏感的。

#### JS之中的数据类型

基本数据类型（值类型）：String，Number，Boolean，Null，Undefined

引用数据类型（引用类型）：Object

#### String

String 之中单引号可以嵌套双引号

###### typeof运算符

`console.log(typeof(null))` 返回结果是 `object`

#### Number

由于内存的限制，ECMAScript 并不能保存世界上所有的数值。

- 最大值：`Number.MAX_VALUE`，这个值为： 1.7976931348623157e+308
- 最小值：`Number.MIN_VALUE`，这个值为： 5e-324

如果使用Number表示的变量超过了最大值，则会返回Infinity。

- 无穷大（正无穷）：Infinity
- 无穷小（负无穷）：-Infinity

注意：`typeof Infinity`的返回结果是number。

###### NaN 和 isNan() 函数

NaN: Not a number ，表示其是非数值。

​	typeof(NaN) 的结果是 number。个人认为，NaN是对于不是数字的事物做了数字运算，因此在运算之后其类型一定是 number

​	NaN和任何数值都不等，包括NaN

`        console.log(NaN==NaN);`返回 false

isNaN():任何不可以转换成数值的值，都会让其返回 true

```javascript
	isNaN(NaN);// true
	isNaN("blue"); // true
	isNaN(123); // false
```

#### 浮点数的运算

在JS中，整数的运算**基本**可以保证精确；但是**小数的运算，可能会得到一个不精确的结果**。所以，千万不要使用JS进行对精确度要求比较高的运算。

原因为二进制并不能精确的表示某些小数(位数总有限制)，所以存在着小数计算不精确的问题。

#### 隐式转换

`        console.log("2"+1,"2"-1) //21  1`

原因？就在于隐式转换。

隐式转换的符号：

`-`、`*`、`/`、`%`

加号不在其中个人认为是因为还有连接字符串的作用，所以不做转换。

#### null 和 undefined 

`null`： Null 类型的值只有一个，就是 null

- 但是使用 typeof 检查一个 null 值的时候，会返回 object

`undefined`： 声明了变量但是还没有赋值的情况，此时值就是 undefined

- 使用 typeof 检查一个 undefined 的时候，返回的就是 undefined



null 和 undefined 有着最大的相似性，在 null == undefined 的值为 true就可以证明。但是null === undefined的结果(false)。它们虽然相似，但还是有区别的，其中一个区别是：和数字运算时，10 + null结果为：10；10 + undefined结果为：NaN。

- 任何数据类型和undefined运算都是NaN;
- 任何值和null运算，null可看做0运算。

# 03-变量的强制类型转换

强制类型转换主要是将其他的数据类型转换为 String,Number,Boolean。不会做没意义的类似将数据类型转换成 null 或者 undefined 的事情。

#### 其他的简单类型 --> String

###### 方法1：变量+'' 或者 变量 +“abc”

```javascript
vat a = 123;  // Number 类型
console.log(a + '');  // 转换成 String 类型
console.log(a + 'haha');  // 转换成 String 类型
```

###### 方法2：调用 toString（）方法

该方法不会影响到**原有变量**。

null 和 undefined 这两个值没有 toString() 方法。

另外，Number类型的变量，在调用toString()时，可以在方法中传递一个整数作为参数。此时它将会把数字转换为指定的进制，如果不指定则默认转换为10进制。例如：

```javascript
        var a = 255;

        //对于Number调用toString()时可以在方法中传递一个整数作为参数
        //此时它将会把数字转换为指定的进制,如果不指定则默认转换为10进制
        a = a.toString(2);

        console.log(a);        // 11111111
        console.log(typeof a); // string
```

###### 方法3：使用 String（）函数

使用String()函数做强制类型转换时：

- 对于Number和Boolean而言，实际上就是调用toString()方法。
- 但是对于null和undefined，就不会调用toString()方法。它会将 null 直接转换为 "null"。将 undefined 直接转换为 "undefined"。

#### 其他数据类型-->Number

###### 方法一：使用 Number() 函数

