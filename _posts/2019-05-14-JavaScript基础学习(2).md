---
layout:     post   				    # 使用的布局（不需要改）
title:      JavaScript基础学习（2）			# 标题 
subtitle:    从第十一节-this 开始                #副标题
date:       2019-05-14 				# 时间
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

# 11-this

解析器在调用函数每次都会向函数内部传递进一个隐含的参数，这个隐含的参数就是this，this指向的是一个对象，这个对象我们称为函数执行的 上下文对象。

根据函数的调用方式的不同，this会指向不同的对象：【重要】

- 1.以函数的形式调用时，this永远都是window。比如`fun();`相当于`window.fun();`
- 2.以方法的形式调用时，this是调用方法的那个对象
- 3.以构造函数(constructor)形式调用时，this是新创建的那个对象
- 4.使用call和apply调用时，this是指定的那个对象

**针对第1条的举例**：

```
    function fun() {
        console.log(this);
        console.log(this.name);
    }

    var obj1 = {
        name: "smyh",
        sayName: fun
    };

    var obj2 = {
        name: "vae",
        sayName: fun
    };

    var name = "全局的name属性";

    //以函数形式调用，this是window
    fun();       //可以理解成 window.fun()
```

打印结果：

```
    Window
    全局的name属性
```

上面的举例可以看出，this指向的是window对象，所以 this.name 指的是全局的name。

**第2条的举例**：

```
        function fun() {
            console.log(this);
            console.log(this.name);
        }

        var obj1 = {
            name: "smyh",
            sayName: fun
        };

        var obj2 = {
            name: "vae",
            sayName: fun
        };

        var name = "全局的name属性";

        //以方法的形式调用，this是调用方法的对象
        obj2.sayName();
```

打印结果：

```
    Object
    vae
```

上面的举例可以看出，this指向的是 对象 obj2 ，所以 this.name 指的是 obj2.name。

#### 类数组 arguments

在调用函数时，浏览器每次都会传递进两个隐含的参数：

- 1.函数的上下文对象 this
- 2.**封装实参的对象** arguments

例如：

```
    function foo() {
        console.log(arguments);
        console.log(typeof arguments);
    }

    foo();
```

[![img](https://camo.githubusercontent.com/1fdfe8a7bc24efa30c2721aabd28ff767c9d898d/687474703a2f2f696d672e736d79687661652e636f6d2f32303138303331355f303930332e706e67)](https://camo.githubusercontent.com/1fdfe8a7bc24efa30c2721aabd28ff767c9d898d/687474703a2f2f696d672e736d79687661652e636f6d2f32303138303331355f303930332e706e67)

arguments是一个类数组对象，它可以通过索引来操作数据，也可以获取长度。

**arguments代表的是实参**。在调用函数时，我们所传递的实参都会在arguments中保存。有个讲究的地方是：arguments**只在函数中使用**。

### 1、返回函数**实参**的个数：arguments.length

arguments.length可以用来获取**实参的长度**。

举例：

```
    fn(2,4);
    fn(2,4,6);
    fn(2,4,6,8);

    function fn(a,b) {
        console.log(arguments);
        console.log(fn.length);         //获取形参的个数
        console.log(arguments.length);  //获取实参的个数

        console.log("----------------");
    }
```

打印结果：

[![img](https://camo.githubusercontent.com/e7fbc0fbf771f8a378760bb7c1669873f175a8e6/687474703a2f2f696d672e736d79687661652e636f6d2f32303138303132355f323134302e706e67)](https://camo.githubusercontent.com/e7fbc0fbf771f8a378760bb7c1669873f175a8e6/687474703a2f2f696d672e736d79687661652e636f6d2f32303138303132355f323134302e706e67)

可见其哪怕传入了多余形参的个数，其也可以识别出来。但是形参的个数并不随着实参个数的改变而改变。

我们即使不定义形参，也可以通过arguments来使用实参（只不过比较麻烦）：arguments[0] 表示第一个实参、arguments[1] 表示第二个实参...

### 2、返回正在执行的函数：arguments.callee

arguments里边有一个属性叫做callee，这个属性对应一个函数对象，就是当前正在指向的函数对象。

```
    function fun() {

        console.log(arguments.callee == fun); //打印结果为true
    }

    fun("hello");
```

在使用函数**递归**调用时，推荐使用arguments.callee代替函数名本身。

### 3、arguments可以修改元素

之所以说arguments是伪数组，是因为：**arguments可以修改元素，但不能改变数组的长短**。举例：

```
    fn(2,4);
    fn(2,4,6);
    fn(2,4,6,8);

    function fn(a,b) {
        arguments[0] = 99;  //将实参的第一个数改为99
        arguments.push(8);  //此方法不通过，因为无法增加元素
    }
```