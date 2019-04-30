---
layout:     post   				    # 使用的布局（不需要改）
title:      《Spring Boot 42讲》学习笔记（3） 				# 标题 
subtitle:   模板引擎Thymeleaf使用  #副标题
date:       2019-04-29				# 时间
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



# 第2-3课：模板引擎Thymeleaf基础使用

### 1. 什么是模板引擎？

模板引擎，是为了用户界面和业务数据（内容）分离而成的，其可以生成特定格式的文档，用于网站的模板引擎就会生成一个标准的HTML文档。

模板引擎的实现有“置换引擎”（将模板内容之中的特定标记替换，但效率底下），“解释型”模板引擎和“编译型”模板引擎等。

### 2. Thymeleaf介绍与其特点

具体介绍网上很多，个人理解 Thymeleaf 就是一个用来写前端的模板。

Thymeleaf的特点如下：

- Thymeleaf在有网络和无网络的条件下都可以运行，既可以查看页面布局等静态效果，又可以查看带数据的动态页面效果。原理是其支持HTML原型，然后在HTML标签之中加入额外的属性来达到模板+数据的方式。由于浏览器在解释HTML的时候会忽略未定义的标签属性，所以Thymeleaf的模板可以静态的运行，当有数据返回的时候，Thymeleaf标签便可以动态的替换掉静态内容，使页面动态显示。
- 开箱即用：支持标准方言和Spring方言，可以直接套用模板实现 JSTL 和 OGNL 等等效果。
- Thymeleaf提供一个Spring标准方言和SpringMVC完美集成的可选模块，可快速实现表单绑定，属性编辑器，I18N等等。

#### 对比

Thymeleaf相比之下使用了自然的模板技术，意味着Thymeleaf的模板语法并不会破坏文档结构，模板依旧是有效的XML文档。

下面是打印消息的对比：

```
Velocity: <p>$message</p>
FreeMarker: <p>${message}</p>
Thymeleaf: <p th:text="${message}">Hello World!</p>
```

可以看到Thymeleaf的作用于在HTML的标签之内，类似于标签的一个属性来使用，这就是其一个特点。

**注意：由于Thymeleaf使用了XML DOM 解析器，因此不适合处理大规模的XML文件**

> 为什么不适合？
>
>  **DOM（Document Object Model)**
>
> ​      DOM是用与平台和语言无关的方式表示XML文档的官方W3C标准。DOM是以层次结构组织的节点或信息片断的集合。这个层次结构允许开发人员在树中寻找特定信息。分析该结构通常需要加载整个文档和构造层次结构，然后才能做任何工作。由于它是基于信息层次的，因而DOM被认为是基于树或基于对象的。
>
> 【优点】
>       ①允许应用程序对数据和结构做出更改。
>       ②访问是双向的，可以在任何时候在树中上下导航，获取和操作任意部分的数据。
> 【缺点】
>       ①通常需要加载整个XML文档来构造层次结构，消耗资源大。
>
> 因此可以看出大规模的XML文件的预先加载会消耗过量资源，因此不是最适合的。



### 快速上手：Thymeleaf的Hello world

首先是加入Dependency

```
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

然后在applications.properties 之中配置变量

`spring.thymeleaf.cache=false`

含义为关闭Thymeleaf的缓存，不然在开发过程之中修改页面不会立刻生效，而是需要重启。生产配置可以为True。



#### 模板页面编写

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8"></meta>
    <title>Hello</title>
</head>
<body>
<h1 th:text="${message}">Hello World</h1>
</body>
</html>
```

这个页面，如果直接在IDEA之中使用Chrome打开，所显示的只有一行“Hello World”，这也体现了thymeleaf的特性，即可以离线访问已经输入页面，便于前端进行操作。

注意，所有使用Thymeleaf的页面必须在HTML标签声明Thymeleaf

`<html lang="en" xmlns:th="http://www.thymeleaf.org">`

表明页面使用的是 Thymeleaf 语法

#### Controller

```java
@Controller
public class HelloController {
    @RequestMapping("/")
    public String index(ModelMap map)
    {
        map.addAttribute("message", "https://timzhouyes.github.io");
        return "hello";
    }
}
```

- 此处和之前不同，使用的是`@Controller` 而非 `@RESTController` ，区别下面讲。
- 代码的含义是将map传入到页面里面，然后将值替换掉。

#### 一些问题

和教程不同，我一直是将新代码加到上一个代码的项目之中，而非开新的项目。当前的这个教程，即2-3，其`pom.xml` 之中的 Dependencies 有两项要删除，分别为：

```
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jstl</artifactId>
		</dependency>
		<dependency>
			<groupId>org.apache.tomcat.embed</groupId>
			<artifactId>tomcat-embed-jasper</artifactId>
		</dependency>
```



实测这两项加入之后，无论怎样调整，都会显示`Whitelabel Error Page`，个人认为这两项和Thymeleaf不兼容，会拦截所有想要导向`\templates\hello.html` 的请求，并且将其转到请求`\WEB-INF\jsp\hello.jsp`,从而导致 Whitelabel 的错误。顺带说一句，教程2-2之中的解决`Whitelabel Error Page` 的方法在这里不起作用。



#### 对于@Controller 和 @RestController 的解析

下面是对于@Controller和@RestController的区别解析，由于在自己输入代码的过程之中将二者混淆，导致最后结果不太对，这个地方要明确注意一下。

首先是官方文档的解释：@RestController注解相当于@ResponseBody ＋ @Controller合在一起的作用。

但是在使用的过程之中有一些出入：

1. 如果只是使用@RestController注解Controller，那么Controller之中的方法就无法返回对应的 JSP 页面，也就是其配置的InternalResourceViewResolver不起作用，那么返回的就是Return里面的内容。也就是本来应该到`success.jsp`页面的，结果只显示“success”
2. 如果想要返回到指定页面，需要使用@Controller配合视图解析器InternalResourceViewResolver才可以
3. 如果需要返回JSON，XML或者自定义MediaType内容到页面，需要在对应的方法上面加上 @ResponseBody

### 常用语法

##### 赋值，字符串拼接

赋值：

`<p th:text=${username}>Username</p>`

`<span th:text="'Welcome to our application'+${username}+'!'"></span>`

字符串拼接还有另一种简洁的方法：

`<span th:text="|Welcome to our application,${username}!|"></span>`



下面是`\templates\string.html`:

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Example String</title>
</head>
<body>
    <div>
        <h1>text</h1>
        <p th:text="${username}">Username</p>
        <span th:text="'Welcome to our application'+${username}+'!'"></span>
        <br/>
        <span th:text="|Welcome to our application,${username}!|"></span>
    </div>
</body>
</html>
```

后端传值：

```java
@Controller
public class ExampleController {
    @RequestMapping("/string")
    public String string(ModelMap map)
    {
        map.addAttribute("username", "Mint");
        return "string";
    }
}

```



和之前一样，使用ModelMap以KV的形式传到页面。

直接在 IDEA 里面打开的结果显示:

> # text
>
> Username

而在网页之中传递得到的结果是:

> # text
>
> Mint
>
> Welcome to our applicationMint!
>
> Welcome to our application,Mint!



### 条件判断If/Unless

在 Thymeleaf 之中，使用 th:if 和 th:unless 进行判断，在下面例子之中， `<a>` 标签只有在 th:if 成立时才显示， 而只有在 th:unless 不成立时才显示。



if.html 的代码：

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Example If/Unless</title>
</head>
<body>
<div>
    <h1>If/Unless</h1>
    <a th:if="${flag=='yes'}" th:href="@{https://timzhouyes.github.io}">home</a>
    <br/>
    <a th:unless="${flag!='no'}" th:href="@{https://google.com}">unless test</a>
</div>

</body>
</html>
```



后端传值Controller部分：

```java
    @RequestMapping("/if")
    public String ifunless(ModelMap map)
    {
        map.addAttribute("flag","yes");
        return "if";
    }
```

也是使用KV方式传值到页面。

 该代码效果为只显示“home”， 不显示其他。



##### for循环

For 循环一般结合前端的表格使用。

首先在后端定义一个用户列表，注意此处需要首先在model 的 User 里面加入一个 constructor：

```java
    public User(String name, int age, String pass) {
        this.name = name;
        this.age = age;
        this.pass = pass;
    }
```

这样就不必一次次的调用 setName 等等。



再定义一个用户列表：

```java
    private List<User> getUserList(){
        List<User> list=new ArrayList<User>();
        User user1=new User("A1",13,"1234567");
        User user2=new User("A2",14,"4321234");
        User user3=new User("A3",19,"0933211");
        list.add(user1);
        list.add(user2);
        list.add(user3);
        return list;
    }
```



按照键 users ，传递到前端：

```java
    @RequestMapping("/list")
    public String list(ModelMap map)
    {
        map.addAttribute("users",getUserList());
        return "list";
    }
```

页面 `list.html` 进行数据展示：

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Example For test</title>
</head>
<body>
<div>
    <h1>for loop testing</h1>
    <table>
        <tr th:each="user,iterStat:${users}">
            <td th:text="${user.name}">Neo</td>
            <td th:text="${user.age}">19</td>
            <td th:text="${user.pass}">6666666</td>
            <td th:text="${iterStat.index}">index</td>
        </tr>
    </table>
</div>

</body>
</html>
```

其中的`iterStat` 称为状态变量，属性有：

- index: 当前迭代对象的index,从0计算。
- count: 当前迭代对象的index ，从1计算。
- size:被迭代对象的大小。
- current：当前迭代变量，我的变量是`com.neo.hello.model.User@703cd7ff`
- even/odd: 布尔值，当前循环是否是偶数/奇数。
- first: 布尔值，当前循环是否是第一个
- last: 布尔值，当前循环是否是最后一个

### URL

Thymeleaf对于URL的处理是通过语法@{...} 来处理的。如果需要 Thymeleaf 对于URL 进行渲染，务必使用 th:href, th:src 等属性，下面是一个例子：

此处的URL经过传值进入：

##### `url.html`

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Example URL</title>
</head>
<body>
<div>
    <h1>URL</h1>
    <a th:href="@{http://www.ityouknow.com/{type}(type=${type})}">link1</a>
    <br/>
    <a th:href="@{http://www.ityouknow.com/{pageId}/can-use-springcloud.html(pageId=${pageId})}">view</a>
    <br/>
    <div th:style="'background:url(' + @{${img}} + ');'">
        <br/><br/><br/>
    </div>
</div>

</body>
</html>
```



##### 后端程序

```java
    @RequestMapping("/url")
    public String url(ModelMap map)
    {
        map.addAttribute("type","link");
        map.addAttribute("pageId","springcloud/2017/09/11/");
        map.addAttribute("img","http://www.ityouknow.com/assets/images/neo.jpg");
        return "url";
    }
```



### 三目运算

```
<input th:value="${name}"/>
<input th:value="${age gt 30 ? '中年':'年轻'}"/>
```

下面这个语句是年龄>30 显示中年， 年龄 < 30 显示年轻。

- gt: greater than （大于）
- ge: great equal （大于等于）
- eq: equal （等于）
- lt： less than （小于）
- le: less equal (小于等于)
- ne:not equal (不等于)



##### 完整的页面内容 `eq.html`

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>ternary operator</title>
</head>
<body>
<div>
    <h1>EQ</h1>
    <input th:value="${name}"/>
    <br/>
    <input th:value="${age gt 30 ? 'Old':'young'}"/>
    <br/>
    <a th:if="${flag eq 'yes'}" th:href="@{https://google.com}">Google</a>
</div>

</body>
</html>
```

##### 后端程序

```java
    @RequestMapping("/Sanmu")
    public String Sanmu(ModelMap map)
    {
        map.addAttribute("name","neo");
        map.addAttribute("age",90);
        map.addAttribute("flag","yes");
        return "eq";
    }
```

注意此处我的 @RequestMapping 和 最后返回的名字并不相同，前者是浏览器之中的URL， 后者是在`\templates` 之中的` eq.html`

