# SpringMVC


# 什么是SpringMVC

Spring MVC 是 Spring 提供给 Web 应用的框架设计

## MVC

[参考](https://www.jianshu.com/p/91a2d0a1e45a)

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210312153341.png)

首先用户的请求会到达 Servlet，然后根据请求调用相应的 Java Bean，并把所有的显示结果交给 JSP 去完成，这样的模式我们就称为 MVC 模式。

- **M 代表 模型（Model）**
   模型是什么呢？ 模型就是数据，就是 dao,bean
- **V 代表 视图（View）**
   视图是什么呢？ 就是网页, JSP，用来展示模型中的数据;<u>主要负责接受Servlet传递的内容，调用JavaBean，将内容显示给用户</u>
- **C 代表 控制器（controller)**
   控制器是什么？ 控制器的作用就是把不同的数据(Model)，显示在不同的视图(View)上，Servlet 扮演的就是这样的角色。<u>主要负责所有用户的请求参数，判断请求参数是否合法，根据请求的类型调用JavaBean，将最终的处理结果交给显示层显示！</u>



---



![image-20210312153639550](C:\Users\ssl\AppData\Roaming\Typora\typora-user-images\image-20210312153639550.png)

---



![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210312154336.png)

**传统的模型层被拆分为了业务层(Service)和数据访问层（DAO,Data Access Object）。** 在 Service 下可以通过 Spring 的声明式事务操作数据访问层，而在业务层上还允许我们访问 NoSQL ，这样就能够满足异军突起的 NoSQL 的使用了，它可以大大提高互联网系统的性能。

- **特点：**
   结构松散，几乎可以在 Spring MVC 中使用各类视图
   松耦合，各个模块分离
   与 Spring 无缝集成

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210312154725.png)

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
> Controller 接口将处理用户请求，这和 [Java](http://c.biancheng.net/java/) Servlet 扮演的角色是一致的。一旦 Controller 处理完用户请求，将返回 ModelAndView 对象给 DispatcherServlet 前端控制器，ModelAndView 中包含了模型（Model）和视图（View）。
>
> 从宏观角度考虑，DispatcherServlet 是整个 Web 应用的控制器；从微观考虑，Controller 是单个 Http 请求处理过程中的控制器，而 ModelAndView 是 Http 请求过程中返回的模型（Model）和视图（View）。
>
> ViewResolver 接口（视图解析器）在 Web 应用中负责查找 View 对象，从而将相应结果渲染给客户。

## Demo

[链接](https://blog.csdn.net/superxiaolong123/article/details/79881335)



# HttpSession

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



# @PostMapping

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



# redirect

[参考](https://www.cnblogs.com/selene/p/4518246.html)

下图所示的间接转发请求的过程如下：

1. 浏览器向Servlet1发出访问请求；
2. Servlet1调用sendRedirect()方法，将浏览器重定向到Servlet2；
3. 浏览器向servlet2发出请求；
4. 最终由Servlet2做出响应。 



![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210316154329.png)



# 什么是JavaBean

[参考](https://www.zhihu.com/question/19773379)

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210312150236.png)



![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210312150412.png)

# 参考

[链接](http://c.biancheng.net/spring_mvc/)
