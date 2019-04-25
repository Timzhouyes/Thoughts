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



Tip：this指代的是当前class之中的变量，所以善用this可以在getter和setter之中省去命名新变量。如下所示：



```java
    public void setName(String name){
        this.name=name;
    }
```



上面的代码就实现了将method之中的name赋值给class之中的`private String name` 的作用。



在根目录下面新建一个model的包，然后在包的下面新建一个实体类User，其信息如下：



```java
public class User {
    private String name;
    private int age;
    private String pass;
    //Getter和Setter可以通过IDE自动生成
```



在项目之中新建一个Web包，且在Web下面新建一个类WebController，类之中创建一个方法返回User，如下：



```java
@RequestMapping(name="/getUser",method= RequestMethod.POST)
    public User getUser(){
        User user=new User();
        user.setName("mint");
        user.setAge(19);
        user.setPass("123456");
        return user;
    }
```





-  `@RestController`的注解相当于将`@ResponseBody`+`@Controller`合在一起使用，使用`@RestController`注解之后，代表整个类都会以JSON方式返回结果。
- `@RequestMapping(name="/getUser",method= RequestMethod.POST)`这句设置了路由“/getUser”之中的返回值，而其中的`method=RequestMethod.POST` 意为只有Post的请求方式是被允许的，如果使用Get的方式，那么会报405不允许访问的错误。错误信息如下：





```html
There was an unexpected error (type=Method Not Allowed, status=405).
Request method 'GET' not supported
org.springframework.web.HttpRequestMethodNotSupportedException: Request method 'GET' not supported
```





- 个人Tip：在这里我试了将上面的语句之中的`name="/getUser"` 改成`name="/getUser123"` ,之后发现在Postman测试时，使用POST方法访问，两个URL都可以获得信息。



#### 测试代码



在test之中新建WebController测试类，对getUser()方法进行测试：



```java
@SpringBootTest
public class WebControllerTest {
    private MockMvc mockMvc;

    @Before
    public void setUp() throws Exception{
        mockMvc= MockMvcBuilders.standaloneSetup(new WebController()).build();
    }

    @Test
    public void getUser() throws Exception{
        String responseString=mockMvc.perform(MockMvcRequestBuilders.post("/getUser")).andReturn().getResponse().getContentAsString();
        System.out.println("result:"+responseString);
    }
}
```



- @Before  之中的作用和之前一样，都是先让程序跑起来，建立测试环境。
- @Test 之中的 `andReturn().getResponse().getContentAsString()` 意为得到相应之后将其转换成String。



#### 返回一个JSON List

下面是将一个由User组成的List返回的方法：



首先，将WebController之中添加getUsers(),代码如下：



```java
    @RequestMapping(value="/getUsers")
    public List<User> getUsers(){
        List<User> users =new ArrayList<User>();
        User user1=new User();
        user1.setName("mint1");
        user1.setAge(19);
        user1.setPass("123456");
        users.add(user1);
        User user2=new User();
        user2.setName("mint3");
        user2.setAge(192);
        user2.setPass("1234516");
        users.add(user2);
        return users;
    }

```



然后添加测试方法：



```java
    @Test
    public void getUsers() throws Exception{
        String responseString=mockMvc.perform(MockMvcRequestBuilders.get("/getUsers")).andReturn().getResponse().getContentAsString();
        System.out.println("result"+responseString);
    }
```



注意，测试方法之中省去了 @Before 的部分。



### 个人提醒

在第二个返回一个List的过程之中，我遇到了一些很有趣的事情。

