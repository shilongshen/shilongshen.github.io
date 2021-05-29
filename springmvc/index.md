# SpringMVC

# MVC

[参考](https://www.jianshu.com/p/91a2d0a1e45a)

<img src="https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210312153341.png" style="zoom:67%;" />

首先用户的请求会到达 Servlet，然后根据请求调用相应的 Java Bean，并把所有的显示结果交给 JSP 去完成，这样的模式我们就称为 MVC 模式。

- **M 代表 模型（Model）**
   模型负责封装数据在视图层显示
- **V 代表 视图（View）**
   视图是什么呢？ 就是网页, JSP，用来展示模型中的数据;<u>主要负责接受Servlet传递的内容，调用JavaBean，将内容显示给用户</u>
- **C 代表 控制器（controller)**
   控制器是什么？ 控制器的作用就是把不同的数据(Model)，显示在不同的视图(View)上，Servlet 扮演的就是这样的角色。<u>主要负责所有用户的请求参数，判断请求参数是否合法，根据请求的类型调用JavaBean，将最终的处理结果交给显示层显示！</u>
- 目的：将业务逻辑从页面中分离出来，进行解耦

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210418193405.png)



# SpringMVC基本概念

Spring MVC 是 Spring 提供给 Web 应用的框架设计

<img src="https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210312154336.png" style="zoom:67%;" />

<img src="https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210418194307.png" style="zoom:67%;" />

**传统的模型层被拆分为了业务层(Service)和数据访问层（DAO,Data Access Object）。** 在 Service 下可以通过 Spring 的声明式事务操作数据访问层，而在业务层上还允许我们访问 NoSQL ，这样就能够满足异军突起的 NoSQL 的使用了，它可以大大提高互联网系统的性能。

- **特点：**
   结构松散，几乎可以在 Spring MVC 中使用各类视图
   松耦合，各个模块分离
   与 Spring 无缝集成

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210418194758.png)

<img src="https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210312154725.png" style="zoom:67%;" />

> 从图 1 可总结出 Spring MVC 的工作流程如下：
>
> 1. 客户端请求提交到 DispatcherServlet。
> 2. 由 DispatcherServlet 控制器寻找一个或多个 HandlerMapping，找到处理请求的 Controller。
> 3. DispatcherServlet 将请求提交到 Controller。
> 4. Controller 调用业务逻辑处理后返回 ModelAndView。
> 5. DispatcherServlet 寻找一个或多个 ViewResolver 视图解析器，找到 ModelAndView 指定的视图。
> 6. 视图负责将结果显示到客户端。
>
> ## Spring MVC接口
>
> 在图 1 中包含 4 个 Spring MVC 接口，即 DispatcherServlet、HandlerMapping、Controller 和 ViewResolver。
>
> Spring MVC 所有的请求都经过 DispatcherServlet 来统一分发（通过DispatcherServlet拦截请求(拦截器，web.xml中配置)，再请求分发到对应的Controller），在 DispatcherServlet 将请求分发给 Controller 之前需要借助 Spring MVC 提供的 HandlerMapping 定位到具体的 Controller。
>
> <u>HandlerMapping 接口通过解析xml文件负责完成客户请求到 Controller 映射</u>。
>
> Controller 接口将处理用户请求，这和 [Java](http://c.biancheng.net/java/) Servlet 扮演的角色是一致的。一旦 Controller 处理完用户请求，将返回 ModelAndView 对象给 DispatcherServlet 前端控制器，ModelAndView （Model指封装数据的模型）和视图（View指视图的名字，会在解析时找到相应的页面，并将Model中封装的数据传入页面中，然后进行解析）。
>
> 从宏观角度考虑，**DispatcherServlet 是整个 Web 应用的控制器；从微观考虑，Controller 是单个 Http 请求处理过程中的控制器**，而 ModelAndView 是 Http 请求过程中返回的模型（Model）和视图（View）。
>
> ViewResolver 接口（视图解析器）在 Web 应用中负责查找 View 对象，从而将相应结果渲染给客户。

## Demo

[链接](https://blog.csdn.net/superxiaolong123/article/details/79881335)

[链接](https://www.jianshu.com/p/2101d176555b)

<img src="https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210430173839.png" style="zoom:80%;" />

- web.xml：web的配置文件
- ApplicationContext.xml 是spring 全局配置文件，用来控制spring 特性的   -->spring的配置文件
- dispatcher-servlet.xml 是spring mvc里面的，控制器、拦截uri转发view   -->springMVC的配置文件，前端控制器

## ssm整合

### web.xml

```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>Archetype Created Web Application</display-name>

    <!--  spring配置文件-->
    <!--从类路径下加载spring的配置文件,classpath关键字特指类路径下加载 begin-->
  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>/WEB-INF/applicationContext.xml</param-value>
  </context-param>

     <!--负责启动spring容器的监听器 begin-->
  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>

    <!--  springMVC的配置文件dispatcher servlet-->
    <!--声明springMVC 的 DispatcherServlet begin-->
  <servlet>
    <servlet-name>dispatcher</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <load-on-startup>1</load-on-startup>
  </servlet>

     <!--拦截器配置 begin-->
  <servlet-mapping>
    <servlet-name>dispatcher</servlet-name>
    <url-pattern>*.form</url-pattern>
  </servlet-mapping>

</web-app>

```

### ApplicationContext.xml 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:mybatis-spring="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <!-- spring希望管理所有业务逻辑组件.数据源、事务控制、aop 等  -->

    <!--
    开启组件扫描
    不扫描Controller，其他都扫描
    -->
    <context:component-scan base-package="com.demo">
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>


    <!--    引入数据库的配置文件-->
    <context:property-placeholder location="classpath:config.properties"/>
    <!--   引入数据源c3p0，从配置文件中读取数据库4个基本信息 -->
    <bean id="pooledDataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="jdbcUrl" value="${url}"/>
        <property name="driverClass" value="${driver}"/>
        <property name="user" value="${username}"/>
        <property name="password" value="${password}"/>
    </bean>

    <!--
        spring事务管理器
            需要控制数据库连接池中所有的连接，包括连接的开启、提交、关闭
    -->
    <bean id="dataSourceTransactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="pooledDataSource"/>
    </bean>

    <!--    开启基于注解的事务-->
    <tx:annotation-driven transaction-manager="dataSourceTransactionManager"/>


    <!--    mybatis的配置-->
    <!--
    会创建出sqlSessionFactory对象
    这就相当于spring已启动就代替我们去创建sqlSessionFactory对象，
    通过sqlSessionFactory来获取sqlSession

    之前创建sqlSessionFactory需要写一长串的代码，但是现在不需要了
    String resource = "MyBatis.xml";
    InputStream inputStream = Resources.getResourceAsStream(resource);
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
     -->
    <bean id="sessionFactoryBean" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!--        指定数据源，-->
        <property name="dataSource" ref="pooledDataSource"/>
        <!--        指定mybatis全局配置文件，配合使用-->
        <property name="configLocation" value="classpath:mybatis.xml"/>
        <!--       指定mapper.xml文件的位置 -->
        <property name="mapperLocations" value="classpath:mybatis.mapper"/>
    </bean>


    <!--
    扫描所有mapper接口的实现，让这些mapper能够自动注入
    base-package="com.demo.dao"：指定mapper接口的路径
    -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.demo.dao"/>
    </bean>

    <!--
    整合mybatis
        目的：
            1.spring管理所有组件，包括mybatis中mapper的实现类；
                这样业务逻辑层（service）要调用Dao层的时候，只需要使用依赖注入（@Autowired）自动注入mapper

            2.spring用来管理事务
    -->


</beans>
```

### dispatcher-servlet.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--
    springMVC只是控制网站跳转逻辑
    -->

    <!--
    开启组件扫描功能
    只在com.demo路径下扫描 
    只扫描@Controller注解的类只扫描控制器
    禁用默认的过滤行为
-->
    <context:component-scan base-package="com.demo" use-default-filters="false">
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>

   <!--    视图解析器-->
    <!--
		1.使用@RequestMapping	注解来映射请求的URL
		2.返回值会通过视图解析器解析为实际的物理视图，对于InternalResourceViewResolver视图解析器会做如下解			析:通过prefix+return value+suffix  得到实际的物理视图
	-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
<!--        在/WEB-INF/pages目录下寻找以return value.jsp为结尾的文件-->
<!--        prefix+return value+suffix-->
        <property name="prefix" value="/WEB-INF/pages"/>
        <property name="suffix" value=".jsp"/>
    </bean>


    <!--    处理动态资源-->
    <mvc:annotation-driven/>
    <!--    处理静态资源-->
    <mvc:default-servlet-handler/>

</beans>
```



## helloworld实例

- 在配置文件中配置dispatcher-servlet
- 加入springMVC的配置文件dispatcher-servlet.xml
- 编写处理请求的处理器，并表示为处理器
- 编写视图

# URL地址映射配置

## @RequestMapping

通过注解@RequestMapping将请求地址与方法进行绑定，可以在类级别和方法级别声明。类级别的注解负责将一个特定的请求路径映射道一个控制器上，将URL和类绑定；通过方法级别的注解可以细化映射，能够将一个特定的请求映射到某个具体的方法上，将url和方法进行绑定。

### 映射单个URL

@RequsetMapping或@RequsetMapping（value=“ ”）

```java
@Controller
public class MainController {

    /**
    声明在方法上面，映射单个URL
    访问地址：http://ip:port/hello
    
    路径开头是否加斜杠/都可以
    */
    @RequestMapping("/hello")
    public ModelAndView test(){
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("hello");
        modelAndView.setViewName("hello");
        return modelAndView;
    }
}
```



### 映射多个URL

@RequsetMapping({“”  ,  “”})

```java
@Controller
public class MainController {

    @RequestMapping({"/hello","/world"})
    public ModelAndView test02(){
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("hello");
        modelAndView.setViewName("hello");
        return modelAndView;
    }
}
```

### 映射URL在控制器上

用于类上，表示类中所有响应请求方法都是以该地址作为父路径。

```java
/*

*/

@Controller
@RequestMapping("/beijng")
public class Controller1 {
    
    //test1处理的url为http://ip:port/beijng/hello
    @RequestMapping("/hello")
    public ModelAndView test1(){
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("hello");
        modelAndView.setViewName("hello");
        return modelAndView;
    }
}
```



### 设置URL映射的请求方式

默认没有设置请求方式，在HTTP请求中最常用的方式时GET和POST，还有一些其他的方法，例如：DELETE,PUT,HEAD。

可以通过method属性设置支持的请求方式

```java
@Controller
@RequestMapping(value = "/hello",method = RequestMethod.GET)//设置请求方式为GET-->获取资源
public class control2 {
    public ModelAndView test1(){
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("hello");
        modelAndView.setViewName("hello");
        return modelAndView;
    }
}
```

### 通过参数名称映射URL

```java

/*
//test1处理的url为http://ip:port/url?hello
*/
@Controller
@RequestMapping(params = "/hello")
public class control2 {
    public ModelAndView test1(){
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("hello");
        modelAndView.setViewName("hello");
        return modelAndView;
    }
}
```

## @GetMapping

> ###### @RequestMapping和@GetMapping区别
>
> - @RequestMapping可以指定GET、POST请求方式
> - @GetMapping等价于@RequestMapping的GET请求方式



# 参数绑定

参数绑定，简单来说就是客户端发送请求，而请求中包含一些数据，那么这些数据怎么到达 Controller 

　在 SpringMVC 中，提交请求的数据是通过**方法形参**来接收的。从客户端请求的 key/value 数据，经过参数绑定，将 key/value 数据绑定到 Controller 的形参上，然后在 Controller 就可以直接使用该形参。

SpringMVC 有支持的默认参数类型，我们直接在形参上给出这些默认类型的声明，就能直接使用了。如下：

　　①、HttpServletRequest 对象

　　②、HttpServletResponse 对象

　　③、HttpSession 对象

　　④、Model/ModelMap 对象　

## 基本数据类型

- 参数如果未传递会报500异常   -->**注意基本数据类型不能为null**
- 参数可以通过地址栏的URL直接传入`http://localhost:8080/hello?age=20&money=200`
- 参数名和形参名对应

```java
@Controller
public class HelloController {
    @RequestMapping("/hello")
    public void test01(int age,double money){
        System.out.println("age="+age+" money="+money);
    }
}
```

- 可以设置参数的默认值，当参数传递的时候，参数对应的值为默认值`@RequestParam(defaultValue = "10")`

```java
@Controller
public class HelloController {
    @RequestMapping("/hello")
    public void test01(@RequestParam(defaultValue = "10") int age, @RequestParam(defaultValue = "200") double money){
        System.out.println("age="+age+" money="+money);
    }
}
```

- 通过`@RequestParam`设置**参数别名**,请求参数地址需要与`name`属性值一致，`(@RequestParam(defaultValue = "10",name = "userage")`

```java
@Controller
public class HelloController {
    @RequestMapping("/hello")
    public void test01(@RequestParam(defaultValue = "10",name = "userage") int age, @RequestParam(defaultValue = "200") double money){
        System.out.println("age="+age+" money="+money);
    }
}
```

## 包装类型

- 包装数据类型可以为null，即当参数未传递时不会报错    -->推荐使用

```java
@Controller
public class HelloController {
    @RequestMapping("/hello")
    public void test01(@RequestParam(defaultValue = "10",name = "userage") Integer age, @RequestParam(defaultValue = "200") Double money){
        System.out.println("age="+age+" money="+money);
    }
}
```

## 数组类型

- 传参形式：参数名=值&参数名=值&参数名=值...
- 参数名需要和形参名保持一致`http://localhost:8888/test02?ids=100&ids=200`

```java
 @RequestMapping("/test02")
    public String test02(String[] ids){
        for (String id:ids){
            System.out.println(id);
        }
        return "hello wodld";
    }
```

## 字符串类型

- 可以接收null。`http://localhost:8888/test03?id=%22beijignnihao%22`

```java
@RequestMapping("/test03")
    public  String test03(String id){
        return "hello world";
    }
```



## JavaBean类型

- 参数名和JavaBean的属性字段名一致

创建一个类

```java
package com.demo.po;

public class User {
    private Integer id;
    private String name;
    private String pwd;

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", pwd='" + pwd + '\'' +
                '}';
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPwd() {
        return pwd;
    }

    public void setPwd(String pwd) {
        this.pwd = pwd;
    }
}

```

控制器,`http://localhost:8888/test04?id=100&name=%22beijign%22&pwd=%22shanghai%22`

输出`User{id=100, name='"beijign"', pwd='"shanghai"'}`

```java

@RequestMapping("/test03")
    public  String test03(String id){
        return "hello world";
    }
```

## List类型

list需要在JavaBean中使用

```java
package com.demo.po;

import java.util.List;

public class User {
    private Integer id;
    private String name;
    private String pwd;

    private List<String> strList;

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", pwd='" + pwd + '\'' +
                ", strList=" + strList +
                '}';
    }

    public List<String> getStrList() {
        return strList;
    }

    public void setStrList(List<String> strList) {
        this.strList = strList;
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPwd() {
        return pwd;
    }

    public void setPwd(String pwd) {
        this.pwd = pwd;
    }
}

```

控制器

```java
@RequestMapping("/test05")
    public  void test05(User user){
        System.out.println(user);
    }
```

```java

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<!--准备一个表单，在一个页面中填写相应的数据，通过提交表单将参数进行提交到test05中，并进行打印-->
    <form action="test05" method="post">
        姓名：<input name="name" type="text"> <br>
        密码：<input name="pwd" type="password"> <br>
        List: <input name="strList[0]" type="text"> &nbsp;
        <input name="strList[1]" type="text">
        <button>提交</button>
    </form>
</body>
</html>
```



## Map类型

Map最为灵活，它也需要绑定在对象上，二不能直接写在Controller方法的参数中

```java
private Set<Integer> set=new HashSet<>();
private Map<Integer,Integer> map=new HashMap<>();
...
```

````java
@RequestMapping("/test06")
    public String test06(User user){
        return "hello world";
    }
````



```html
<form action="test05" method="post">
        姓名：<input name="name" type="text"> <br>
        密码：<input name="pwd" type="password"> <br>
        List: <input name="strList[0]" type="text"> &nbsp;
        <input name="strList[1]" type="text">
        Map:<input type="text" name="map['sh']"> &nbsp;
        <input type="text" name="map['bj']"> &nbsp;
        <input type="text" name="map['sz']">
        <button>提交</button>
    </form>
```

# 请求重发和重定向

SpringMVC默认采用服务器内部转发的形式展示页面信息。同样也支持重定向页面。

## 重定向

重定向是发一个302状态码（临时重定向）给浏览器，浏览器自己取请求跳转的页面。<u>地址栏会发生变化</u>

**请求转发以`forward`开头**

**重定向以`redirect`**开头，可以重定向到视图或者是方法

```java
	/**     
	* 重定向到视图方法1
    */
    @RequestMapping("/test07")
    public ModelAndView test07() {
        ModelAndView modelAndView = new ModelAndView();
//        设置重定向的地址
        modelAndView.setViewName("redirect:view.html");
        return modelAndView;
    }

    /**
     * 重定向到视图方法2
     * 返回视图名称（字符串）  -->常用
     */
    @RequestMapping("/test077")
    public String test077() {

        return "redirect:view.html";
    }

    /**
     * 重定向到方法
     */
    @RequestMapping("/test08")
    public ModelAndView test08() {
        ModelAndView modelAndView = new ModelAndView();
//        设置重定向的方法
        modelAndView.setViewName("redirect:hello");
        return modelAndView;
    }
```

重定向中传递参数

```java
/**
     * 重定向到视图
     * 返回视图名称（字符串）  -->常用
     * 并且进行传递参数
     */
    @RequestMapping("/test0777")
    public String test0777() {

        return "redirect:view.html?name=zhangsan&pwd=123";
    }
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h2>请求转发和重定向</h2>
<!--接收参数：${param.参数名}-->
 ${param.name}
${param.pwd}
</body>
</html>
```

重定向中传递中文参数（直接传可能会出现乱码），所以需要使用`RedirectAttributes`

```java
@RequestMapping("/test0778")
    public String test0778(RedirectAttributes redirectAttributes) {
//        设置参数
        redirectAttributes.addAttribute("name", "张三");
        redirectAttributes.addAttribute("pwd", "123");


        return "redirect:view.html";
    }
```

## 请求转发

请求转发，直接调用跳转的页面，让他返回。对于浏览器来说，它无法感觉服务器有么有`forward`。**地址栏不发生改变**。可以获取请求域中的数据。

请求转发以`forward`开头

可以请求转发到视图或方法



### 设置Request请求域

1. ModelAndView对象
    addObject("变量名","值")

2. HttpServletRequest
   setAttribute("变量名","值")

3. Model

   addAttribute("变量名","值")

4. ModelMap

   addAttribute("变量名","值")

5. Map

   put("变量名","值")

 ```java
package com.demo.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import java.util.Map;


@Controller
public class IndexController {
    /**
     * 默认请求转发访问
     * 视图存放在springmvc配置文件中设置的目录（视图解析器设置的前缀）
     */
    @RequestMapping("/view01")
    public String view01(HttpServletRequest request) {
//        设置默认请求域
        request.setAttribute("msg", "hello");
        return "hello";
    }

    /**
     * 设置请求域
     *  1.ModelAndView对象
     *      addObject("变量名","值")
     *
     *  2.HttpServletRequest
     *      setAttribute("变量名","值")
     * */
    @RequestMapping("/view02")
    public ModelAndView view02(){
        ModelAndView modelAndView=new ModelAndView();
        modelAndView.addObject("msg","beijing");
        modelAndView.setViewName("model");
        return modelAndView;
    }

    @RequestMapping("/view03")
    public String view03(Model model){
        model.addAttribute("msg","shenzhen");
        return "model";

    }

    @RequestMapping("/view04")
    public String view04(ModelMap modelMap){
        modelMap.addAttribute("msg","shenzhen");
        return "model";

    }

    
    @RequestMapping("/view05")
    public String view05(Map map){
        map.put("msg","shenzhen");
        return "model";

    }


}

 ```

### 设置Request请求域

1. HttpSession

   setAttribute("变量名","值")

```java
@RequestMapping("/view06")
    public  String view06(HttpSession session){
        session.setAttribute("msg","hsanghai");
        return "model";
    }
```

# JSON数据开发

## 基本概念

JSON在企业开发中已经作为通用的接口参数类型，在页面（客户端）解析很方便。SpringMVC对于json提供了良好的支持，这里需要修改相关的配置，添加json数据支持功能

### @ResponseBody

- 将返回的对象转换为指定格式,

该注解将用于将Controller的方法返回的对象，通过适当的`HttpMessageConverter`转换为指定格式后，写入到HTTP响应报文中(`Response`对象)的`body`数据区。

返回的数据不是`html`标签页面，而是其他某种格式的数据时(如jsp,xml等)使用（通常用于`ajax`请求）

### @RequestBody

- 用于将请求的参数转换为特定的格式

该注解用于读取`Request`请求的`body`部分数据，使用系统默认配置的`HttpMessageConverter`进行解析，然后把相应的数据绑定到要返回的对象上，再把`HttpMessageConverter`返回的对象数据绑定到`controller`中方法的参数上。

## 使用配置

......













# 常见参数



## @Control  以及 @RestController

> @Control +@@ResponseBody=@RestController

- 单独使用@Control 不加@@ResponseBody的话返回一个视图。这种情况属于传统的SpringMVC应用，对应于前后端不分离的情况
- @RestController只返回对象，对象数据直接以JSON或XML形式写入HTTP响应中。这是最常接触的情况，即前后端分离

[参考](https://blog.csdn.net/yamadeee/article/details/80313068)







## HttpSession

[参考](https://www.cnblogs.com/myseries/p/11588267.html)

```
HttpSession 服务端的技术
服务器会为每一个用户 创建一个独立的HttpSession

HttpSession原理
当用户第一次访问Servlet时,服务器端会给用户创建一个独立的Session
并且生成一个SessionID,这个SessionID在响应浏览器的时候会被装进cookie中,从而被保存到浏览器中
当用户再一次访问Servlet时,请求中会携带着cookie中的SessionID去访问
服务器会根据这个SessionID去查看是否有对应的Session对象
有就拿出来使用;没有就创建一个Session(相当于用户第一次访问)

域的范围:
    Context域 > Session域 > Request域
    Session域 只要会话不结束就会存在 但是Session有默认的存活时间(30分钟)
```

[参考](https://blog.csdn.net/qq_39949109/article/details/82454060)



## @PostMapping

[参考](https://www.cnblogs.com/niexinlei/p/9704075.html)

```
@getMapping与@postMapping
首先要了解一下@RequestMapping注解。

　　@RequestMapping用于映射url到控制器类的一个特定处理程序方法。可用于方法或者类上面。也就是可以通过url找到对应的方法。

　　@RequestMapping有8个属性。

value：指定请求的实际地址。

method：指定请求的method类型（GET,POST,PUT,DELETE）等。

consumes：指定处理请求的提交内容类型（Context-Type）。

produces：指定返回的内容类型，还可以设置返回值的字符编码。

params：指定request中必须包含某些参数值，才让该方法处理。

headers：指定request中必须包含某些指定的header值，才让该方法处理请求。

 

@getMapping与@postMapping是组合注解。

@getMapping = @requestMapping(method = RequestMethod.GET)。

@postMapping = @requestMapping(method = RequestMethod.POST)。
```



## redirect

[参考](https://www.cnblogs.com/selene/p/4518246.html)

下图所示的间接转发请求的过程如下：

1. 浏览器向Servlet1发出访问请求；
2. Servlet1调用sendRedirect()方法，将浏览器重定向到Servlet2；
3. 浏览器向servlet2发出请求；
4. 最终由Servlet2做出响应。 



![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210316154329.png)



## 什么是JavaBean

[参考](https://www.zhihu.com/question/19773379)

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210312150236.png)



![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210312150412.png)



---





# 参考

[链接](http://c.biancheng.net/spring_mvc/)

JavaGuide面试突击版
