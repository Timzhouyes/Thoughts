---
layout:     post   				    # 使用的布局（不需要改）
title:      Java Guide 笔记  		# 标题 
subtitle:           #副标题
date:       2021-12-06		# 时间
author:     Haiming 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - Programming
    - Java
    - Interview
---

# 二 Java 基础+集合+多线程+JVM

## 2.1. Java 基础

### 2.1.1 面向对象和面向过程的区别

面向过程就一定比面向对象性能高？

不对。面向过程本身也要分配内存，计算偏移量等等，java 相对C 性能低一些是为了可以执行，所以是先变成字节码再去由 JVM 解释执行，其产出并不是真正的机械码。然而C 或者 C++其生成的是可以直接执行的机械码，那么少了中间一环，性能相对就要好一些。

一些面向过程的脚本语言，其性能也不一定有 java 好。

### 2.1.3. 关于 JVM JDK 和 JRE 最详细通俗的解答

Java 程序从源码到运行一般有下面三步：

![image-20211207141126891](../img/2021-12-06-javaGuide/image-20211207141126891.png)

按照图中来看，`.java`在 javac 编译之后变成`.class` 文件，然后`.class`文件由 JVM 进行**解释变成机器可执行的二进制机器码后执行**。

这种逐行执行的方法，相对而言效率较低。而且软件也是符合“二八定律”，有少部分的热点代码会被执行多次。

那么是不是可以“空间换时间”呢？后面 HotSpot 之中引入了 JIT 编译器，JIT 属于运行时编译，将热热点部分的代码的字节码对应的机器码保存下来，用来下次使用。

JDK9之中引入了一种新的编译模式，AOT(ahead of time compliation)，其直接将字节码编译成机器码，避免了 JIT 预热等等开销。AOT 的编译质量是肯定比不上 JIT 编译器的。

### 2.1.5. Java 和 C++的区别?

相同点：都是面向对象的语言，那么都支持面向对象的基本特征：继承，封装和多态

不同点：

1. Java 之中不提供指针来对内存进行访问，因此程序内存更安全
2. Java 的类是单继承的，而C++支持多重继承。Java 之中的 interface 支持多重继承

> Java 不支持**类**的多重继承，主要是因为类的多重继承有可能会产生菱形继承的问题：
>
> 有一个root class, class A 和  class B 都继承这个 root class, 并且对同一个方法有不同的实现，现在一个 class C 继承 class A 和 class B，那么对于这个方法应该怎么处理？
>
> 要是不合并，那么会在子类之中包含两份祖父类之中的内容，产生歧义。但是合并的话， 那么如何指定呢？
>
> 但是在接口层面就没这些问题：人家根本就没实现，反正都是在最后这个继承类之中进行实现，那么又担心什么呢？
>
> **参考**
>
> 为什么Java可以多继承interface，而不可以多继承class？ - 徐辰的回答 - 知乎 https://www.zhihu.com/question/20306381/answer/16895493
>
> Java 为什么不支持多继承？ - RednaxelaFX的回答 - 知乎 https://www.zhihu.com/question/24317891/answer/65097560

3. Java 之中有自动内存管理机制，不需要手动的去 malloc 内存
4. 在 C 语言之中，字符串或者字符数组的最后都有一个额外的`\0`表示结束，但是 Java 之中没有结束符这个概念。
   这个是因为 C 是面向过程的语言，那么相对的其对于各种字符串需要用一个标识符来作为标识。而 java 是面向对象的语言，所有的东西都是类，比如 String 或者 List之中都有专门的一个属性来做对应的长度标识：`length` 或者`size`，自然就不需要一个标志位来做这些处理了。

### 2.1.6. 字符型常量和字符串常量的区别?

1. 含义上：char 相当于一个整型值，可以参与相应的表达式运算。但是字符串常量代表的是该字符串在内存之中存放的位置

2. 占内存大小：字符常量只占两个字节，16bits，但是字符串常量占若干个字节。

   > Java 之中，各种**基本类型**所占的存储空间的大小并不随着机器的不同而不同，比如 char 就是两个字节，这种存储空间不跟着机器硬件架构的变化而变化的不变性，就是 Java 程序更具有可移植性的原因之一。

### 2.1.7. 构造器 Constructor 是否可被 override?

override: 重写

Overload: 重载

那么分析可得，constuctor 不可能被 override。 首先 override 需要：

1. 继承相关的类
2. 重写**相同名称的方法**

constructor 之中必须和对应的类名字相同，那么子类一定和父类名字不同，那么子类的 constuctor 就不可能**重写**父类的构造函数。

### 2.1.8. 重载和重写的区别

**重载：**

发生在编译期。发生在一个类之中，方法名相同，参数类型不同，个数不同，顺序不同，方法的**返回值**和**访问修饰符**可以不同。

也就是说，除了名字相同之外，什么都可以不同。

**重写：**

重写发生在运行期，是子类对于父类的**允许访问的方法**的实现过程的重新编写。

1. 对于**基本类型和 void**, 返回值的类型不可以修改。 对于**引用类型**，返回值的类型可以使用其子类型。方法名，参数列表必须相同，抛出的异常范围**小于等于**父类。**访问修饰符范围**大于等于父类。

   > 抛出的异常范围小于父类，才能够保证该异常被父类抓住。访问修饰符的范围大于等于父类，才能让父类调用出的方法被子类替换之后仍然成立。

2. 父类访问修饰符是`private/final/static`，那么子类不能重写该方法。但是被 static 修饰的方法能够被再次声明。

### 2.1.10. String StringBuffer 和 StringBuilder 的区别是什么? String 为什么是不可变的?

主要区别是**可变性**。

**String**，在 java 9 之前，其内部保存字符串的实现是用`final`关键字来修饰保存的。`private final char value[]`。所以 String 对象是不可变的。

在 java 9之后，其内部的实现是用 byte 数组来存储的，`private final byte[] value`。

> https://www.baeldung.com/java-9-compact-string#java
>
> Java 9 has brought the concept of compact *Strings* ba*ck.*
>
> This means that **whenever we create a \*String\* if all the characters of the \*String\* can be represented using a byte — LATIN-1 representation, a byte array will be used** internally, such that one byte is given for one character.
>
> In other cases, if any character requires more than 8-bits to represent it, all the characters are stored using two bytes for each — UTF-16 representation.
>
> So basically, whenever possible, it’ll just use a single byte for each character.
>
> Now, the question is – how will all the *String* operations work? How will it distinguish between the LATIN-1 and UTF-16 representations?
>
> **Well, to tackle this issue, another change is made to the internal implementation of the \*String\*. We have a final field \*coder\*, that preserves this information.**
>
> 也就是说，能用单个字节表示的时候，都用单个字节，latin-1 的方式进行表示。不能的时候，全部的字符都用两个字节来标识，utf-16.
>
>
> https://www.baeldung.com/java-9-compact-string#compressed-1
>
> 相对而言，全英文的句子更多，那么意味着大部分的 String 都可以用 latin-1格式来编码，用 byte 显然会对内存的压力更小，而且的确 String 的值占用大部分的内存池容量，对 String 做一些优化是很值得的。

**StringBuilder**,**StringBuffer**都是继承自`AbstractStringBuilder`,其中也是用字符数组保存字符串`char[] value`，但是没有用 final 修饰，所以这两种对象都是可变的。

但是 StringBuilder 是单线程，而 StringBuffer 是多线程安全的，方式是加上了同步锁。

### 2.1.11. 自动装箱与拆箱

https://www.cnblogs.com/dolphin0520/p/3780005.html

1. 装箱是用`.valueOf()`方法来做的，拆箱是对应的`xxxValue()`,比如 int 就是`intValue()`。

2. 拆装箱和缓存池一般都有关系，比如 Int 在[-128,127]之间的话，新建对象就返回已经存在的缓存池之中的对象的引用。否则创建一个新的对象。

3. > 下面程序的输出结果是什么？
   >
   > ```java
   > public class Main {
   >     public static void main(String[] args) {
   >          
   >         Integer a = 1;
   >         Integer b = 2;
   >         Integer c = 3;
   >         Integer d = 3;
   >         Integer e = 321;
   >         Integer f = 321;
   >         Long g = 3L;
   >         Long h = 2L;
   >          
   >         System.out.println(c==d);
   >         System.out.println(e==f);
   >         System.out.println(c==(a+b));
   >         System.out.println(c.equals(a+b));
   >         System.out.println(g==(a+b));
   >         System.out.println(g.equals(a+b));
   >         System.out.println(g.equals(a+h));
   >     }
   > }
   > ```
   >
   > 　　先别看输出结果，读者自己想一下这段代码的输出结果是什么。这里面需要注意的是：当 "=="运算符的两个操作数**都是 包装器类型的引用**，则是比较指向的是否是同一个对象，而如果其中有一个操作数是**表达式（即包含算术运算）**则比较的是数值（即会触发自动拆箱的过程）。另外，对于包装器类型，**equals方法并不会进行类型转换**。明白了这2点之后，上面的输出结果便一目了然：
   >
   > ```java
   > true
   > false
   > true
   > true
   > true
   > false
   > true
   > ```
   >
   > 　　第一个和第二个输出结果没有什么疑问。第三句由于 a+b包含了算术运算，因此会触发自动拆箱过程（会调用intValue方法），因此它们比较的是数值是否相等。而对于c.equals(a+b)会先触发自动拆箱过程，再触发自动装箱过程，也就是说a+b，会先各自调用intValue方法，得到了加法运算后的数值之后，便调用Integer.valueOf方法，再进行equals比较。同理对于后面的也是这样，不过要注意倒数第二个和最后一个输出的结果（如果数值是int类型的，装箱过程调用的是Integer.valueOf；如果是long类型的，装箱调用的Long.valueOf方法）。

   - 两边均是仅仅包装器类型：比较是否指向同一个对象——直接调用对象的时候，值传递的是其对应的指针。
   - 具有相关的运算：比较的是数值，触发自动拆箱的过程——已经有一边不止一个对象了，那肯定没法直接比地址啊，看值做操作吧
   - 包装器类型，`equals()`并不会做类型转换——类型转换，只是针对于使用基本类型的常量的时候做的转换，都上不同的类了，自然就不做相应的转换了

### 2.1.14. 接口和抽象类的区别是什么?

1. 抽象类之中可以有非抽象的方法， 但是接口之中的方法不可以有实现（在 java8开始接口可以有默认实现，这也导致了菱形继承的问题。而处理方式是在语言层面限定，对于菱形继承，必须显式的使用@ovverride 并且重写方法，否则报错）
2. 一个类可以实现多个接口，但是只能实现一个抽象类

### 2.1.16. 创建一个对象用什么运算符?对象实体与对象引用有何不同?

1. 内存位置不同：对象实例在堆内存中，对象引用在栈内存之中，指向堆内存之中的对象实例
2. 一个引用可以指向0或者1个对象，一个对象实例可以有多个引用指向他

### 2.1.23. == 与 equals(重要)

`==`: 其作用是看两个对象的地址是否相等。结合之前说的，Java 是值传递，对于基本类型比较的就是值，而对于引用类型比较的则是其引用（内存地址）是否为同一个。

`equals()`: 在默认情况，即没有被覆盖的情况下，其等价`==`来比较两个对象。要是类之中覆盖了`equals()`方法，那么就用覆盖之后的逻辑来判断二者是否相等。

### 2.1.24. hashCode 与 equals (重要)

hashCode() 介绍：

hashCode() 的作用是获取散列码，实际上返回的是一个 int 整数。`Object`类之中的 hashCode() 方法是本地方法，其通常将对象的内存地址转换整数之后返回。

**为什么重写 equals 时必须重写 hashCode 方法?**

不然用默认的实现，就可以得出其所有的对象，在不散列碰撞的情况`hashCode()` 都不同。可是此时判断对象是否相等的`equals()`方法已经被重写。在 Hash 相关的集合里面，比如 hashMap,首先判断的都是 `hashCode()`，那`hashCode()`永远都不一样，自然就没有比较`equals()`方法的地方了，就会造成可能两个对象在`equals()`判断是一样的，应该只存在一个但是`hashCode()` 不同所以均存在的情况。

### 2.1.25. 为什么 Java 中只有值传递?

Java 程序设计语言总是采用按值调用。也就是说，**方法得到的是所有参数值的一个拷⻉**，也就 是说，方法不能修改传递给它的任何参数变量的内容。

### 2.1.27. 线程有哪些基本状态?

首先肯定要有出生和死亡：new，terminated。之外要可以运行：runnable。再就是抢不到锁所以被 blocked, 等待其他线程给动作的 waiting 和 超时等待 time_waiting

### 2.1.28. 关于 final 关键字的一些总结

final 可以用在三个地方：变量，方法，类

在 java 之中，可以定义的部分也就是这三个：变量，方法，类。

1. 对于一个 final 形式的变量，如果其是基本类型，那么数值在初始化之后不可以更改。如果是引用类型，那么初始化之后不可以指向其他对象。
2. final 修饰一个类的时候，其不可以被继承。其中所有方法都会隐式的定义成 final
3. final 的方法不可以修改。

### 2.1.29. Java 中的异常处理

#### 2.1.29.1. Java 异常类层次结构图

![image-20211209102324551](../img/2021-12-06-javaGuide/image-20211209102324551.png)

主要分为两大类：Errors 和 Exceptions。Errors 本身一旦发生了，是无法处理的，一般都是直接 kill 掉所属的线程。

Exceptions 里面还可以分为两类：Check Exceptions 和 Uncheck Exceptions。

其中 Check Exceptions 之中包含了除 RuntimeExceptions 之外的所有 Exceptions, 这些 Exceptions 不可以不 check，也就是必须用 catch/ throw 来写对应的逻辑，不然就无法通过编译。

而 Uncheck Exceptions 之中则有所有其他的 Exceptions。这些哪怕没有被 catch/ throw 也可以正常运行。

Finally 之中如果有对应的 return 语句，那么 finally 之中的语句的内容会被执行，并且 finally 之中的返回值会覆盖原始的返回值。

### 2.1.30. Java 序列化中如果有些字段不想进行序列化，怎么办?

用 transient 修饰。但是 transient 只能修饰变量，无法修饰类和方法。

> 在我看来，只有某些变量在序列化和反序列化的时候可以忽略， 方法则没有这个必要。

#### 2.1.32.2. 既然有了字节流,为什么还要有字符流?

问题本质想问: 不管是文件读写还是网络发送接收，信息的最小存储单元都是**字节**，那为什么
I/O 流操作要分为字节流操作和字符流操作呢?

可以只给字节流操作，但：

1. 字节转换成字符的过程还是比较耗时的
2. 如果不知道字节流的编码方式，就很容易出问题。

**为了方便起见**，IO 流提供了一个直接操作字符的接口，方便对字符进行流操作

## 2.2 代理详解

### 2.2.1 静态代理和动态代理的对比

1. 静态代理：
   - 对每个目标类都**实现一个代理类**
   - 静态代理之中如果接口新增方法， 目标对象和代理对象都要进行修改。这是非常麻烦的。
   - 在编译的时候就将接口，实现类，代理类等等变成一个个**真正的 class 文件**
2. 动态代理：
   - 更灵活，不必须实现接口（cglib), 可以直接代理实现类（实现接口的 jdk 代理或者全部的 cglib)。
   - 在**运行时**动态生成类字节码，加载到 JVM 之中

### 2.2.2 JDK 动态代理和 CGLIB 动态代理对比

1. 从范围看，jdk 动态代理只能代理实现了接口的类，或者直接代理接口。但是 cglib 可以代理未实现任何接口的类
2. 从效率看，大部分情况都是 jdk 动态代理更优秀（因为其只是生成对应的方法然后再拦截，肯定比 cglib 直接生成一个代理类的子类效率更高）

## 2.3 IO 模型详解

本部分包含各种 IO 模型和 select, poll, epoll 等等。

IO 主要分为三中：BIO， NIO 和 AIO。

BIO： 同步阻塞 IO 模型，线程发起 IO 之后一直阻塞，直到缓冲区的数据就绪之后才进行下一步操作。每一个请求都得一个**新的线程**来进行处理。

NIO：同步非阻塞 IO。这里面的同步，指的是线程自己来看是否有请求进来，而异步指的是操作系统来通知对应的线程其是否有数据。那么这个**非阻塞**指的是线程发起请求之后，立刻返回，但是要**主动的**定期轮询是否缓冲区数据就绪。
NIO 是 new IO 的意思，就是 NIO+多路复用技术，而这个多路复用部分， 本身就是一个线程轮询去查看**一堆 IO 缓冲区**之中哪些就绪，检查IO 数据是否就绪的任务是交给操作系统级别的 select 或者 epoll 模型。

AIO：异步非阻塞模型，是直接让操作系统来返回其是否就绪。

### 2.3.3 select, poll, epoll 之间区别

参考： https://zhuanlan.zhihu.com/p/272891398

先说前提：select, poll 和 epoll 本质上都是**同步 IO**。因为都需要在读写事件就绪之后**自己负责**读写！异步 IO 的话不需要自己负责读写！

Select: 在轮询的时候需要将所有的 FD 从用户态拷贝到内核态，而且只知道有请求进来但是不知道是哪个具体请求，所以要**轮询**全部的请求，摘出谁有请求进入再一起处理。有最大1024的限制。

Poll：和 select 区别不大，但是基于链表存储，没有**最大连接数限制**

Epoll：是被动接受通知，有 callback 函数，每次醒来的时候只需要去看对应区域的 FD。而且其内存是通过文件映射，内核和用户空间共享一块内存来实现的，这就避免了数据拷来拷去所带来的消耗。

对于 epoll，还有触发模式的不同：水平触发和边缘触发。他们对于socket 内存之中没有读完的数据处理方式不同：

	- 水平触发：只要该 fd 还有数据可读，每次 epoll_wait 都会返回其事件来让用户操作
	- 边缘触发：只提示一次，直到下次有数据流入之前都不会再提示。所以 ET(edge trigger) 之中，读一个 fd 的时候一定要把 buffer 全部读完。

## 三 Java集合

## 3.1 Collection 子接口之 List

### 3.1.1 Arraylist 与 LinkedList 区别?

1. 是否线程安全？均非同步，所以都不是线程安全
2. 底层数据结构：ArrayList之中是`Object[]`，LinkedList 底层是**双向链表**
3. 插入和删除是否受元素位置影响？
   - `ArrayList`：受。当整个 List 的长度为 n，要在指定位置 i 插入和删除元素的时候，时间复杂度为 O(n-i)。因为在将位置 i 处增加一个元素之后，需要将后面 (n-i) 个元素全部都往后挪一位
   - `LinkedList`: 其在寻找插入位置的时候也是会受到影响：假设在位置 i 处增加一个元素，那么需要遍历到位置 i 才能够进行插入或者删除操作。但是在找到这个节点之后，就只需要对其前面后面的指针做修改，不需要将后面的元素再挪位置。
4. 是否支持快速随机访问：`ArrayList`  接受，但是`LinkedList`不接受
5. 内存空间占用：`ArrayList`之中要在 list 的末尾预留一部分空间，`LinkedList` 不用预留空间，但是其中每个节点的空间都要更多，因为要保存前驱和后驱节点

### 3.1.2 ArrayList 扩容机制分析

如果没有传入数组容量，新创建的数组默认是一个空数组，在第一次使用时候才会按照默认容量10来创建。

扩容之中采用的位运算：`        int newCapacity = oldCapacity + (oldCapacity >> 1); `。每次增加之后的容量是愿容量的1.5倍。

增加的逻辑是新建一个容量为 `newCapacity` 的数组，用`Arrays.copyOf()`方式将老数组之中内容拷贝过去且指向内部用来保存数据的`Object[]` elementData。

## 3.2  Collection 子接口之 Set

### 3.2.1 比较 HashSet、LinkedHashSet 和 TreeSet 三者的异同

相同点：三者都线程不安全

不同点：底层实现各不相同：

- `hashSet `的底层实现是`HashMap`。
- `LinkedHashSet` 的底层实现是`HashMap` 和链表
- `TreeSet` 的底层实现是红黑树

底层数据结构的不同，导致三者的应用场景也不同。

- `HashSet`：元素不需要有序，最基本的 Set 实现
- `LinkedHashSet`：元素有序插入和取出，符合 FIFO
- `TreeSet`:红黑树天然排序，需要对元素自定义排序规则场景

## 3.3 Collection 子接口之 Queue

### 3.3.1 Queue 与 Deque 的区别

**不同：**

queue 是单端队列，实现 FIFO。

Deque 是双端队列，队列两端均可以插入或者删除元素。



事实上，`Deque` 还提供有 `push()` 和 `pop()` 等其他方法，可用于模拟栈。

**相同：**

`Deque` 和 queue 在**因为容量问题而导致操作失败后处理方式不同**方面都有两套方法，一套抛出异常，一套返回特殊值。

| `Queue` 接口 | 抛出异常  | 返回特殊值 |
| ------------ | --------- | ---------- |
| 插入队尾     | add(E e)  | offer(E e) |
| 删除队首     | remove()  | poll()     |
| 查询队首元素 | element() | peek()     |

| `Deque` 接口 | 抛出异常      | 返回特殊值      |
| ------------ | ------------- | --------------- |
| 插入队首     | addFirst(E e) | offerFirst(E e) |
| 插入队尾     | addLast(E e)  | offerLast(E e)  |
| 删除队首     | removeFirst() | pollFirst()     |
| 删除队尾     | removeLast()  | pollLast()      |
| 查询队首元素 | getFirst()    | peekFirst()     |
| 查询队尾元素 | getLast()     | peekLast()      |

### 3.3.2 说一说 PriorityQueue

`PriorityQueue` 和`Queue` 的区别是元素的出队顺序和**优先级**是相关的。

1. `PriorityQueue` 利用了二叉堆的数据结构实现，底层是可变长的数组来存储数据
2. 在**插入元素** 和**删除堆顶元素（出队）**的过程之中，时间复杂度是`O(logn)` =》 单个元素快排的时间复杂度， 通过堆元素的上浮和下沉来达到的。
3. `PriorityQueue` 是非线程安全的，且不支持存储`NULL` 和`non-comparable` 的对象 =》如果不能够被比较，就无法得到 priority,也就没有在这个 queue之中存在的意义
4. `PriorityQueue` 默认是小顶堆，但可以通过接收一个`Comparator` 作为构造参数，来自定义元素优先级的先后

## 3.4 Map 接口

### 3.4.1 HashMap 和 HashTable 的区别

1. 线程是否安全：HashMap 是非线程安全的，HashTable 是线程安全的，因为`HashTable` 内部一般都经过`syncronized`修饰

2. 效率：`HashMap` 不考虑线程安全，效率因此比`HashTable` 高一些。HashTable 本身基本被淘汰，需要线程安全的时候可以用 ConcurrentHashMap

3. 对 Null key 和 Null Value 的支持
   **在 HashMap 之中：**

   ```java
   
       /**
        * Computes key.hashCode() and spreads (XORs) higher bits of hash
        * to lower.  Because the table uses power-of-two masking, sets of
        * hashes that vary only in bits above the current mask will
        * always collide. (Among known examples are sets of Float keys
        * holding consecutive whole numbers in small tables.)  So we
        * apply a transform that spreads the impact of higher bits
        * downward. There is a tradeoff between speed, utility, and
        * quality of bit-spreading. Because many common sets of hashes
        * are already reasonably distributed (so don't benefit from
        * spreading), and because we use trees to handle large sets of
        * collisions in bins, we just XOR some shifted bits in the
        * cheapest possible way to reduce systematic lossage, as well as
        * to incorporate impact of the highest bits that would otherwise
        * never be used in index calculations because of table bounds.
        */
       static final int hash(Object key) {
           int h;
           return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
       }
   
   ```

   可以看出来如果 key 是Null，其 hash 值为0. 这也是 HashMap 可以存储 null 键的原因。HashMap 之中的null 值可以有很多

   **在 HashTable 之中**

   不允许有 Null  键和 Null 值。抛 NullPointerException

4. 初始容量大小和每次扩充容量大小的不同
   如果没有指定初始值，那么 Hashtable 的默认大小是11，每次扩充之后容量都变为之前的2n+1. 如果给定了初始值，那么 Hashtable 会用给定的大小。

   HashMap 的初始值是16，每次扩充都是之前的2倍。如果给定了初始值，而 HashMap 会将其扩充到2的幂次方大小。

5. 底层数据结构
   此处主要是 HashMap 的巧妙设计，为了解决哈希冲突，当链表长度大于阈值的时候，将链表转换成红黑树来减少搜索时间。

### 3.4.2 HashMap 和 HashSet 区别

HashSet 就是基于 HashMap 实现的，其是使用了 HashMap 的 key 半边，在 value 半边全部放置的是 new Object()。

|               `HashMap`                |                          `HashSet`                           |
| :------------------------------------: | :----------------------------------------------------------: |
|           实现了 `Map` 接口            |                       实现 `Set` 接口                        |
|               存储键值对               |                          仅存储对象                          |
|     调用 `put()`向 map 中添加元素      |             调用 `add()`方法向 `Set` 中添加元素              |
| `HashMap` 使用键（Key）计算 `hashcode` | `HashSet` 使用成员对象来计算 `hashcode` 值，对于两个对象来说 `hashcode` 可能相同，所以`equals()`方法用来判断对象的相等性 |

### 3.4.3 HashMap 和 TreeMap 区别

TreeMap 和 HashMap 都继承自 AbstractMap。但是需要注意的是，TreeMap 还实现了 Navigable 接口和 SortedMap 接口。

Navigable 接口是对 TreeMap 有了搜索集合之内元素的能力。SortedMap 接口让 TreeMap 有了对集合之内的元素根据键排序的能力。

> 个人认为，TreeMap 之中的天然顺序性是 Navigable 和 SortedMap 接口实现的根源。有序是让搜索性能更高的根本。

### 3.4.4 HashSet 如何检查重复

先看 HashSet 之中的add()方法：

```java
    private transient HashMap<E,Object> map;
    
  // Dummy value to associate with an Object in the backing Map
    private static final Object PRESENT = new Object();

/**
     * Adds the specified element to this set if it is not already present.
     * More formally, adds the specified element <tt>e</tt> to this set if
     * this set contains no element <tt>e2</tt> such that
     * <tt>(e==null&nbsp;?&nbsp;e2==null&nbsp;:&nbsp;e.equals(e2))</tt>.
     * If this set already contains the element, the call leaves the set
     * unchanged and returns <tt>false</tt>.
     *
     * @param e element to be added to this set
     * @return <tt>true</tt> if this set did not already contain the specified
     * element
     */
    public boolean add(E e) {
        return map.put(e, PRESENT)==null;
    }
```

可见是直接调用 HashMap 之中的 put()，将要存储的值放在 HashMap 的 key 之中。

再看 HashMap 之中的 put():

```java

    /**
     * Associates the specified value with the specified key in this map.
     * If the map previously contained a mapping for the key, the old
     * value is replaced.
     *
     * @param key key with which the specified value is to be associated
     * @param value value to be associated with the specified key
     * @return the previous value associated with <tt>key</tt>, or
     *         <tt>null</tt> if there was no mapping for <tt>key</tt>.
     *         (A <tt>null</tt> return can also indicate that the map
     *         previously associated <tt>null</tt> with <tt>key</tt>.)
     */
    public V put(K key, V value) {
        return putVal(hash(key), key, value, false, true);
    }
```

 将之前这个 key 所捆绑的 value 返回，如果是 null，那么说明之前对应的 key 并没有什么对应的 value，也就是这个 key 不存在。

### 3.4.5 HashMap 的底层实现

**JDK 1.8之前**

JDK1.8之前，是数组和链表结合在一起使用。

HashMap 通过key 的hashCode 扰动处理之后，得到相应的 hash 值，然后通过(n-1)&hash 判断当前元素存放的位置。再看当前位置是否存在元素，是的话就将该元素和要存入的元素的 hash 值和 key 值都进行比较，相同则覆盖，不同就用拉链法来解决冲突。

因为 hashCode()方法多半会被覆写，怕一些实现的比较差的 hashCode 实现方法，因此会进行一定的扰动，也就是将其右移之后在进行按位异或。

JDK1.8的hash方法：

```java
    static final int hash(Object key) {
      int h;
      // key.hashCode()：返回散列值也就是hashcode
      // ^ ：按位异或
      // >>>:无符号右移，忽略符号位，空位都以0补齐
      return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
  }

```

而 JDK1.7的 hash 方法扰动了4次，相对性能就会差一些。

**JDK1.8 之后**

相比于之前的版本， JDK1.8 之后在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为 8）（将链表转换成红黑树前会判断，如果当前数组的长度小于 64，那么会选择先进行数组扩容，而不是转换为红黑树）时，将链表转化为红黑树，以减少搜索时间。

### 3.4.6 HashMap 的长度为什么是 2 的幂次方

为了**加快哈希计算**和**减少哈希冲突**

为了找到 key 的位置在 hashMap 的哪个槽中，我们要计算 `hash(KEY) % 数组长度`

但是%的效率太低了，我们可以用&来代替%。为了保证&的计算结果等于%的结果，要将 length-1

也就是` hash(KEY) & (length-1)`

而且为2的幂次方，在扩容的时候永远乘2，也能够保证所有的元素要么在原位，要么在扩容之后的+n 的位置，不会有其他的情况，这样方便扩容。

### 3.4.7 HashMap 多线程操作导致死循环问题

因为 jdk1.7之前是头插，jdk1.8及之后改成尾插，在数组容量不够的情况下，发生 rehash 的过程之中，多线程操作有可能会出现循环链表——两个元素相互指向。

详情请查看：[https://coolshell.cn/articles/9606.html(opens new window)](https://coolshell.cn/articles/9606.html)

### 3.4.8 ConcurrentHashMap 和 Hashtable 的区别

二者虽然都是线程安全，但是实现线程安全的方式并不相同。

1. 底层数据结构

   - JDK1.7 的 ConcurrentHashMap 底层采用 分段的数组+链表 的方式实现。其中分段的数组指的是分成很多个 Segment 的 HashEntry 数组。而 JDK1.8之中采用的数据结构和 HashMap1.8一样，数组+链表+红黑树。
   - HashTable 之中采用的还是和 HashMap在 JDK1.8之前的 HashMap 底层数据结构一样的方式，数组+链表；数组是 HashMap 的主体，链表是使用拉链法来解决哈希冲突的方式。

2. 实现线程安全的方式

   - 在 JDK1.7之中，ConcurrentHashMap 对整个桶数组进行了分割分段(Segment)，且 Segment 之中实现了 ReentrantLock 这个可重入锁。

     ```java
     static class Segment<K,V> extends ReentrantLock implements Serializable {
     }
     ```

     多线程访问容器之中不同数据端的数据，就不会存在锁竞争。而在 JDK1.8之中，其实现是 Node 数组+链表/红黑树。冲突的解决是使用 CAS 和 syncronized 方法。syncronized 只锁定当前链表或者红黑二叉树的首节点，只要 hash 不冲突，就不会并发。链表是正常情况下，使用拉链法解决哈希冲突的办法，而红黑树是当链表的长度过长的时候，自动转换成红黑树以增加查找效率，此时对应的 Node 会变成 TreeNode。

   - HashTable 之中，就是把会冲突的方法加上了 syncronize 锁而已，那么一锁锁的是全局，一定会有问题。

## 3.5 HashMap 底层数据结构分析

### 3.5.1 底层数据结构分析

JDK 1.8之前，HashMap 底层是数组+链表结合在一起使用，也就是 **链表散列**。

JDK 1.8之后，底层是 数组+链表+红黑树，其中红黑树是当其链表长度过长的时候，增加查询效率的。默认的时候，链表长度为8，在大于8的时候看entry 数组的长度是否大于64，如果已经大于，说明内部数据量很大，这个时候就会将对应的转换成红黑树了。

`capacity`的用法：

理论上，用拉链法解决哈希冲突的值是无限的，那么就是说节点数组可以不必再增长。但是如果链表的数量很长，哪怕转换成红黑树，其查询效率也不会很高，这个时候就需要扩容。这个 capacity 就是决定当数组占用率到什么时候就去选择扩容。默认值是0.75.

## 3.6 ConcurrentHashMap源码&底层数据结构分析

比较的时候肯定要比1.7和1.8的源码差别

### 3.6.1 ConcurrentHashMap 1.7

**1. 存储结构**

其中主要是用 Segment 来做，Segment 数组本身不可以扩容，默认是16个，也就是同时可以16并发。而在 Segment 里面，有很多 HashEntry<K,V> 数组，这部分可以扩容。
