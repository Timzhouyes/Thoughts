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

#### alert 和 prompt 的区别

```javascript
	alert("从前有座山");                //直接使用，不需要变量
	var a = prompt("请输入一个数字");   // 必须用一个变量，来接收用户输入的值
```



**一点小技巧： 可以在VS Code 之中使用 snippet 来自定义快捷键，其例子就是如何快速输入 console.log()**

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

**NaN和任何数字相加返回都是 NaN**

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

##### 方法一：使用 Number() 函数

###### 情况一：字符串-->数字

- 字符串之中时纯数字，则将其转换为数字
- 字符串之中有非数字的内容，转换成NaN （此处也可看其局限，不能对字符串进行操作）
- 字符串是空或者全是空格，则转换为0。

###### 情况二：布尔-->数字

- true 变成1
- false 变成0

###### 情况三：null / undefined -->数字

null:0

undefined：NaN

原因在于 Null 是空，而 undefined 是没有定义的变量，只是无法识别，但是依然有值。

##### 方法二：`parseInt()` 字符串 --> 整数

parseInt() 作用为将字符串之中的**有效内容**转换为数字。特性为：

1. 只保留字符串最开头的数字，且只会取整数部分

2. 如果对非String使用parseInt() 或者 parseFloat(), 会先转换成 String 之后进行操作。

```javascript
    var a = true;
    console.log(parseInt(a));  //打印结果：NaN （因为是先将a转为字符串"true"，然后然后再操作）

    var b = null;
    console.log(parseInt(b));  //打印结果：NaN  （因为是先将b转为字符串"null"，然后然后再操作）

    var c = undefined;
    console.log(parseInt(c));  //打印结果：NaN  （因为是先将b转为字符串"undefined"，然后然后再操作）

    var d = 168.23;
    console.log(parseInt(d));  //打印结果：168  （因为是先将c转为字符串"168.23"，然后然后再操作）
```

3. 带两个参数时候代表进制转换：parseInt(string, radix)  有2个参数，第一个string 是传入的数值，第二个radix是 传入数值的进制，参数radix 可以忽略，默认为 10，各种进制的数转换为 十进制整数（如果不是整数，向下取整）。

   radix 的取值范围是 2~36，如果 radix 为 1 或 radix>36 ，转换结果将是 NaN ，如果 radix 为 0 或其它值将被忽略，radix 默认为 10 。

   ###### `parseFloat()` 字符串-->浮点数

   其和 parseInt() 作用相似，但是其是将字符串转换为浮点数。

   ##### 转换成Boolean

   使用 Boolean（）函数

   1. 数字-->布尔：除了0和 NaN，其他的都是 true
   2. String-->布尔：除了空字符串其他都是true，字符串之中时空格也是 true
   3. null 和 undefined 会转换成 false
   4. 对象也会转换成 true

   ##### 其他进制的数字

   - 16进制的数字以 `0x`开头
   - 8进制的数字，以0开头
   - 2禁止的数字，以 0b 开头，且IE不支持。

# 04-运算符 operator

#### 运算符的定义和分类

运算符是对一个值或者多个值进行运算，**并且返回结果**。

分类：

- 算术运算符
- 自增运算符
- 逻辑运算符
- 赋值计算符
- 关系计算符
- 三元运算符（条件运算符）

#### 算术运算符

`*`，`/`，和`%` 是一样的优先级。

###### 算数运算符的注意事项

1. 对于非 Number 型的值进行运算的时候，会将这些值先转换成 Number 再计算。但是 `字符串+字符串`，`字符串+Number`是特例。

```javascript
    result1 = true + 1;  // 2 = 1+ 1

    result2 = true + false; // 1 = 1+ 0

    result3 = 1 + null; // 1 = 1+ 0

    result4 = 100 - '1' // 99
```

2. 任何值和NaN做运算的结果都是NaN。

3. 任何值和字符串做加法运算，都会先转换成字符串，然后再做拼串操作。

```
    result1 = 1 + 2 + '3'  // 33

    result2 = '1' + 2 + 3; // 123
```

这也是上面转换为字符串的 `任何类型+""`的来源。也就是说，`c = c + ""` 等价于 `c = String(c)`。

4. 任何值做 `-`、`*`、`/`都会自动转换成 Number （前面提到的隐式转换）。所以，可以为一个值`-0`、`*1`、`/1`来将其转换为Number

###### 乘方：a的b次方

```
	Math.pow(a, b);
```

##### 一元运算符

一元运算符，也就是只有一个操作数的运算符

###### 正号 `+`：

1. 正号不会对数字产生任何影响，2和 +2 是一样的
2. 可以对其他的数据类型使用 + ，来将其转换成 Number

```
    var a = true;
    a = +a;   // 注意这行代码的一元运算符操作
    console.log('a：' + a);
    console.log(typeof a);

    console.log('-----------------');

    var b = '18';
    b = +b;   // 注意这行代码的一元运算符操作
    console.log('b：' + b);
    console.log(typeof b);
```

输出：

```
 a：1
 number
 -----------------
 b：18
 number
```

#### 逻辑运算符

1. JS 之中的 `&&` 属于短路的与，第一个值是false，则不会检查第二值。

```
    //第一个值为true，会检查第二个值
    true && alert("看我出不出来！！");  // 可以弹出 alert 框

    //第一个值为false，不会检查第二个值
    false && alert("看我出不出来！！"); // 不会弹出 alert 框
```

2. JS之中的 `||`也是短路的或。

##### 非布尔值的与或运算【重要】

在实际开发之中，经常使用这种情况做容错处理。非布尔值的逻辑运算符也有返回值。其返回结果是原值。几个非布尔值的运算如下：

```javascript
    var result = 5 && 6; // 运算过程：true && true;
    console.log('result：' + result); // 打印结果：6（也就是说最后面的那个值。）
```

**与运算**的返回结果：（以两个非布尔值的运算为例）

- 如果第一个值为true，则必然返回第二个值（所以说，如果所有的值都为true，则返回的是最后一个值）
- 如果第一个值为false，则直接返回第一个值

**或运算**的返回结果：（以两个非布尔值的运算为例）

- 如果第一个值为true，则直接返回第一个值
- 如果第一个值为false，则返回第二个值

实际开发中，我们经常是这样来处理容错的：

当成功调用一个接口后，针对返回的数据 result，假设我们用变量a 接收。通常的写法是这样的：（这里我只是举个例子）

```javascript
	if (result.resultCode == 0) {
		var a = result && result.data && result.data.imgUrl;
	}
```

也就是说**与运算** 返回的是**最有可能为false**的值，而**或运算** 返回的是**最有可能为 true**的值。

#### 关系运算符

###### 非数值的比较：

1. 非数值会先转换成数字在进行比较。任何值和 NaN 比较都是 false。（前面提到过 NaN 和自己比较也是 false。）

2. **特殊情况！！！：如果两边都是字符串，那么不会变成数字再进行比较，而是比较两个字符串之间的 Unicode 编码。

   所以我们在比较两个字符串的时候，一定要先转型，比如 `parseInt()`

```
// 比较两个字符串时，比较的是字符串的字符编码，所以可能会得到不可预期的结果
console.log("56" > "123");  // true
```

3. 任何值和 NaN 比较结果都是 false

###### `==`和 `===` 的区别

`==` 判断是否等于，其并不严谨，会将不同类型的对象转换成**相同类型** 再进行比较。

1. undefined 衍生自 null ，所以这两个值做` ==` 判断的时候，会返回 true
2. NaN 和任何值都不相等，包括其本身。 所以 

```
console.log(NaN == NaN); //false
```

` ===` 全等符号的强调

全等不会做类型转换。

1. undefined=== null 是false

2. ```javascript
   	console.log("6" === 6);		//false
      	console.log(6 === 6);		//true
   ```

# 05-流程控制语句：选择结构（if和switch）

#### switch

```javascript
	switch(表达式) {
		case 值1：
				语句体1;
			break;

		case 值2：
			语句体2;
			break;

		...
		...

		default：
			语句体 n+1;
			break;
	}
```

省略 break 可能会出现 case 穿透。即执行完一个 case 之后接着执行下一个 case 而不是直接结束。

# 06-流程控制语句：循环结构（for和while）

#### for循环的语法

```
	for(①初始化表达式; ②条件表达式; ④更新表达式){
		③语句...
	}
```

1. 初始化表达式之中可以给多个变量赋值。

那就举个栗子：

```
	for (var i = 1; i <= 10; i++) {

	}
	console.log(i);
```

输出结果：11

但是对于下面的代码，就不行：

```javascript
        for (let i = 1; i <= 10; i++) {

        }
        console.log(i);

```

输出：ReferenceError: i is not defined at operator04.html:42

原因就在于 let 是局部变量，我们之前的文章之中[浅析JavaScript之中的let和const](<https://timzhouyes.github.io/2019/04/15/JavaScript-let-var/>)

有过具体详细的介绍，这里就不多过赘述了。

#### while循环语句和 do...while () 语句

while 语句先做条件判断，再做执行命令。如果值是false 那么直接终止循环。有必要的话也可以使用 break 来终止循环。

而 do...while() 语句是先执行后判断。

所以do...while() 可以保证代码块至少被执行一次，但是 while 不可以。

#### break 和 continue

### break

- break可以用来退出switch语句或**整个**循环语句（循环语句包括for、while。不包括if。if里不能用 break 和 continue，否则会报错）。
- break会立即终止离它**最近**的那个循环语句。
- 可以为循环语句创建一个label，来标识当前的循环（格式：label:循环语句）。使用break语句时，可以在break后跟着一个label，这样break将会结束指定的循环，而不是最近的。

**举例1**：通过 break 终止循环语句

```
    for (var i = 0; i < 5; i++) {
        console.log('i的值:' + i);
        if (i == 2) {
            break;  // 注意，虽然在 if 里 使用了 break，但这里的 break 是服务于外面的 for 循环。
        }
    }
```

打印结果：

```
i的值:0
i的值:1
i的值:2
```

**举例2**：label的使用

```
    outer:
    for (var i = 0; i < 5; i++) {
        console.log("外层循环 i 的值：" + i)
        for (var j = 0; j < 5; j++) {
            break outer; // 直接跳出outer所在的外层循环（这个outer是我自定义的label）
            console.log("内层循环 j 的值:" + j);
        }
    }
```

打印结果：**注意这里面内层循环一次都没有循环完毕就退出了**

```
外层循环 i 的值：0
```

### continue

- continue可以用来跳过**当次**循环。
- 同样，continue默认只会离他**最近**的循环起作用。

# 07-对象简介和对象的基本操作

###### 对象简介

基本数据类型的值是保存在**栈内存** 之中的。值和值之间独立存在，修改一个变量不会影响到另外的变量。（保存的就是值）

对象是保存在**堆内存**之中的，保存的是地址，修改一个有可能会对另外个造成影响（如果两个对象保存的是一个对象引用）

###### 对象的分类

1.内置对象：

- 由ES标准中定义的对象，在任何的ES的实现中都可以使用
- 比如：Math、String、Number、Boolean、Function、Object....

2.宿主对象：

- 由JS的运行环境提供的对象，目前来讲主要指由浏览器提供的对象。
- 比如 BOM DOM。比如`console`、`document`。

3.自定义对象：

- 由开发人员自己创建的对象

###### 创建对象

```
	var obj = new Object();
```

###### 向对象之中添加属性

```
	对象.属性名 = 属性值;
```

例子：

```javascript
    var obj = new Object();

    //向obj中添加一个name属性
    obj.name = "孙悟空";

    //向obj中添加一个gender属性
    obj.gender = "男";

    //向obj中添加一个age属性
    obj.age = 18;

    console.log(JSON.stringify(obj)); // 将 obj 以字符串的形式打印出来
```

注意，将对象打印出来的时候使用 log 经常会出错，所以这个方法很好用：

`    console.log(JSON.stringify(obj)); // 将 obj 以字符串的形式打印出来`

##### 对象使用方法补充：

对象的属性值可以使任何的数据类型，甚至可以是一个函数。在给对象的属性值赋函数的时候，是否加括号区别很大。

```
    var obj = new Object();
    obj.sayName = function () {
        console.log('smyhvae');
    };

    console.log(obj.sayName);  //没加括号，获取的是对象
    console.log('-----------');
    console.log(obj.sayName());  //加了括号，执行函数内容，并执行函数体的内容
```

结果：

```
ƒ () {
	console.log('smyhvae');
        }
 -----------
 smyhvae

```

可以看到，如果不加括号，其输出的是函数体的“原本内容”，但是在加了括号之后，是执行函数。

###### 对象的补充*2

JS 之中的属性值，还可以是一个对象。

举例：

```javascript
    //创建对象 obj1
    var obj1 = new Object();
    obj1.test = undefined;

    //创建对象 obj2
    var obj2 = new Object();
    obj2.name = "smyhvae";

    //将整个 obj2 对象，设置为 obj1 的属性
    obj1.test = obj2;

    console.log(obj1.test.name);
```

打印结果为：smyhvae

#### 获取对象之中的属性值

**方式1**：

语法：

```
	对象.属性名
```

**如果获取对象中没有的属性，不会报错而是返回`undefined`。**

**方式2**：可以使用`[]`这种形式去操作属性

对象的属性名不强制要求遵守标识符的规范，但是我们使用是还是尽量按照标识符的规范去做。

但如果要使用特殊的属性名，就不能采用`.`的方式来操作对象的属性。比如说，`123`这种属性名，如果我们直接写成`obj.123 = 789`，是会报错的。那怎么办呢？办法如下：

语法格式如下：（读取时，也是采用这种方式）

```
对象["属性名"] = 属性值
```

上面这种语法格式，举例如下：

```
 obj["123"] = 789;
```

**重要**：使用`[]`这种形式去操作属性，更加的灵活，因为，我们可以在`[]`中直接传递一个**变量**，这样变量值是多少就会读取那个属性。

#### 删除对象的属性值

语法：

```javascript
	delete obj.name;
```

###### in 运算符

通过该运算符可以检查一个对象之中是否含有指定的属性，有则返回 true， 没有则返回 false

语法：

```
	"属性名" in 对象
```

举例：

```
	//检查obj中是否含有name属性
	console.log("name" in obj);
```

