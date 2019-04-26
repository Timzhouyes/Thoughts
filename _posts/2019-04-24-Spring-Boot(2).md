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

@RequestMapping 的 name 属性几乎没用，想要限定 route 的路径需要使用 value 属性。 具体可以看我在StackOverflow 上面的提问：[Confusion for @RequestMapping solving one value and a list values](https://stackoverflow.com/questions/55844721/confusion-for-requestmapping-solving-one-value-and-a-list-values)





# 第2-1课：Spring Boot对基础Web开发的支持（下）



#### 数据校验



很多时候在处理应用的业务逻辑时，要考虑数据校验的问题。程序必须保证送进来的数据是正确的。



在Spring MVC之中有两种方式可以验证输入，一种是Spring自带的验证框架，另一种是利用JSR实现。



**JSR**（Java规范请求，*Java Specification Requests*），指定了一整套API，通过标注给对象添加约束。Hibernate Validator 就是 JSR 规范的具体实现。 Spring Boot的参数校验依赖于hibernate validator来进行，使用其校验数据，需要定义一个接收的数据模型，使用注解的形式描述字段校验的规则。



下面以User为对象演示如何使用。



首先在WebController之中添加一个saveUser的方法，参数为User



```java
    @RequestMapping("/saveUser")
    public void saveUser (@Valid User user, BindingResult result) {
        System.out.println("user:" + user);
        if (result.hasErrors()) {
            List<ObjectError> list = result.getAllErrors();
            for (ObjectError error : list) {
                System.out.println(error.getCode() + "-" + error.getDefaultMessage());
            }
        }
    }
```



- `@Valid` ：在 User 前面添加了 @Valid 的 annotation， 代表这个对象经过了参数校验。
- `BindingResult` 参数校验的结果会存在这个对象之中，可以根据属性判断是否校验通过，若没有通过，就将错误信息打印出来。



下面是添加了 annotation 之后的注解， 对不同的属性，添加不同的内容： 



```java
public class User {
    @NotEmpty(message="Name can not be null!")
    private String name;
    @Max(value=200,message = "Age can not larger than 200")
    @Min(value=18,message="You have to be adult!")
    private int age;
    @NotEmpty(message = "Password can not be null")
    @Length(min = 6,message = "Password can not shorter than 6 characters")
    private String pass;
//……
}
```



所有的 message 之中的信息，都是自己定义的错误提示信息



下面是测试方法：



```java
    @Test
    public void saveUsers() throws Exception{
        mockMvc.perform(MockMvcRequestBuilders.post("/saveUser").param("name","").param("age","666").param("pass","abcd"));

    }
```



测试得到的返回值如下，注意此处的 user 的返回值和教程不同。



```
user:com.neo.hello.model.User@619bfe29
Length-Password can not shorter than 6 characters
NotEmpty-Name can not be null!
Max-Age can not larger than 200
```



- 个人测试：如果在测试之中，将 age 的传入参数改为 “asd", 会在 Age 的限定处返回：



`typeMismatch-Failed to convert property value of type 'java.lang.String' to required type 'int' for property 'age'; nested exception is java.lang.NumberFormatException: For input string: "asd"`



### 自定义Filter



Filter。过滤器，可以在前端拦截用户的请求。Web开发者通过Filter技术，可以对Web服务器管理的所有Web资源，例如JSP，Servlet，静态图片文件或者HTML文件进行拦截，从而实现某些特殊的功能：URL级别的权限访问限制，过滤敏感词汇，排除有XSS威胁的字符，记录请求日志等等。



Spring Boot有内置的Filter,也支持我们通过自己的需求来自定义Filter。



自定义Filter有两种方式，一种是使用@WebFilter，第二种是使用FilterRegistrtionBean. 教程之中不推荐第一种，因为其实践之后发现 @WebFilter 自定义的优先级顺序不能生效， 因此推荐第二个方案。 



使用FilterRegistrationBean自定义过滤器的方案如下:

- 实现Filter接口，实现其中的doFilter()方法
- 添加@Configuration注解，将自定义Filter加入过滤链。



此处需要加以注意，我在Filter之中加入了`java.util.logging.Filter;`这个引用，可能是由于IDEA自动加入的，但是这个引用导致了我在程序之中的`@Override` 全部提示错误。查阅资料之后发现，这个引用的作用为将Log之中的内容做过滤筛选，这自然不是我们想要的。使用IDEA的时候要注意这一点。下面这段代码，我把引用部分加上了，除了这三个之外的自动补全要小心。



```java
import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import java.io.IOException;

public class MyFilter implements Filter {

    @Override
    public void init(FilterConfig arg0) throws ServletException {
        // TODO Auto-generated method stub
    }

    @Override
    public void doFilter(ServletRequest srequest, ServletResponse sresponse, FilterChain filterChain) throws IOException, ServletException
    {
        HttpServletRequest request=(HttpServletRequest) srequest;
        System.out.println(("this is my Filter, url:"+request.getRequestURI()));
        filterChain.doFilter(srequest, sresponse);
    }
    
    @Override
    public void destroy(){
        //// TODO: 4/26/2019
    }
}
```



将自定义的Filter加入过滤链：



```java
@Configuration
public class WebConfiguration {
    @Bean
    public FilterRegistrationBean testFliterRegisteration(){

        FilterRegistrationBean registration=new FilterRegistrationBean();
        registration.setFilter(new MyFilter());
        registration.addUrlPatterns("/*");
        registration.setName("MyFilter");
        registration.setOrder(6);
        return registration;
    }

    @Bean
    public FilterRegistrationBean test2FilterRegisteration(){
        FilterRegistrationBean registration = new FilterRegistrationBean();
        registration.setFilter(new MyFilter2());
        registration.addUrlPatterns("/*");
        registration.setName("MyFilter2");
        registration.setOrder(1);
        return registration;
    }
}
```





注意下面的代码是加入了两个Filter，其中Filter2 的构造和Filter一样，只是将名字改换一下而已。其中的`registration.setOrder()` 命令是设置Filter的过滤顺序，值越小说明越先过滤。



下面是输出的结果：



```
this is my Filter222222, url:/getUsers
this is my Filter, url:/getUsers
this is my Filter222222, url:/favicon.ico
this is my Filter, url:/favicon.ico
```



可见在console之中输出的结果，Filter2在Filter之前。说明我们顺序的设置是起作用的。



### 配置文件

配置文件，在我看来就是整个文件的全局变量，是一开始就被初始化写到整个程序里面的内容。下面是如何使用`resources/application.properties` 之中的变量来传值的步骤：



注意，等号右边哪怕是String，也不要加双引号。不然会报错。



```
neo.hello.title=Zhou Haiming
neo.hello.description=Do is better than say

```



在test 下面加入一个PropertiesTest的class：



```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class PropertiesTest {
    @Value("${neo.hello.title}")
    private String title;

    @Test
    public void testSingle(){
        Assert.assertEquals(title, "Zhou Haiming");
//        System.out.println("title "+ title);
    }
}

```



可见在这个Test之中可以将`application.properties` 之中的值放到title里面，下面直接进行验证。



