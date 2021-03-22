# 个人博客搭建


技术组合：

- 后端：Spring Boot，JPA，thymeleaf模板
- 数据库:MySQL
- 前端UI：Semantic UI框架

工具与环境：

- IDEA
- Maven 3
- JDK 8
- Axure RP8  -->页面原型设计工具





# 异常处理

拦截异常：自定义400，500, error异常页面

```yaml
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>404</title>
</head>
<body>
    <h1>404</h1>
</body>
</html>
```

```yaml
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>500</title>
</head>
<body>
<h1>500</h1>

</body>
</html>
```

```yaml
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>错误</title>
</head>
<body>
<h1>错误</h1>

<!--可以将错误信息显示在页面的源代码中-->
<div>
    <div th:utext="'&lt;!--'" th:remove="tag"></div>
    <div th:utext="'Failed Request URL : ' + ${url}" th:remove="tag">
    </div>
    <div th:utext="'Exception message : ' + ${exception.message}"
         th:remove="tag"></div>
    <ul th:remove="tag">
        <li th:each="st : ${exception.stackTrace}" th:remove="tag"><span
                th:utext="${st}" th:remove="tag"></span></li>
    </ul>
    <div th:utext="'--&gt;'" th:remove="tag"></div>
</div>

</body>
</html>
```

```yaml
#自定义首页
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>首页</title>
</head>
<body>
<h1>首页</h1>
</body>
</html>
```

定义controller

```java
//错误页面异常信息显示处理
//定义首页控制器
package com.example.BlogDemo.web;

import com.example.BlogDemo.MyNotFoundException;
import javassist.NotFoundException;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class IndexController {
    @GetMapping("/")
    public String index()  {
//        int i=9/0;
        String blog=null;
        if (blog==null){
            throw new MyNotFoundException("博客不存在");
        }
        return "index";

    }
}
```

```java
//资源找不到异常处理
package com.example.BlogDemo;

import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.ResponseStatus;

@ResponseStatus(HttpStatus.NOT_FOUND)
public class MyNotFoundException extends RuntimeException{
    public MyNotFoundException() {
    }

    public MyNotFoundException(String message) {
        super(message);
    }

    public MyNotFoundException(String message, Throwable cause) {
        super(message, cause);
    }
}

```



```java
//统一处理异常
package com.example.BlogDemo.handler;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.core.annotation.AnnotationUtils;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.ResponseStatus;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
//import java.util.logging.Logger;

@ControllerAdvice
public class ControllerExceptionHandler {
    private final Logger logger= LoggerFactory.getLogger(this.getClass());

    @ExceptionHandler(Exception.class)//拦截异常,并进行处理
    public ModelAndView exceptionHandle(HttpServletRequest request,Exception e) throws Exception {
        logger.error("Request URL:{}, Exception:{]",request.getRequestURL(),e);//控制台输出异常
        if(AnnotationUtils.findAnnotation(e.getClass(), ResponseStatus.class)!=null){
            throw e;
        }
        ModelAndView mv = new ModelAndView();
        mv.addObject("url",request.getRequestURL());
        mv.addObject("exception",e);
        mv.setViewName("error/error");
        return mv;
    }
}
```





# 日志处理



记录日志内容

- 请求url
- 请求ip
- 参数args
- 调用方法classmethod
- 返回内容

```java
package com.example.BlogDemo.aspect;

import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.*;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Component;
import org.springframework.web.context.request.RequestContextHolder;
import org.springframework.web.context.request.ServletRequestAttributes;

import javax.servlet.http.HttpServletRequest;
import javax.xml.transform.Result;
import java.util.Arrays;

@Aspect
@Component
public class LogAspect {

    private final Logger logger = LoggerFactory.getLogger(this.getClass());

    @Pointcut("execution(* com.example.BlogDemo.web.*.*(..))")
//切入点表达式，知道对哪个类里面的哪个方法进行增强，对com.example.BlogDemo.web包中所有类，类中所有方法进行增强
    public void log() {
    }

    @Before("log()")//在log()方法之前进行切面操作，在log()方法-->web中的控制器请求，即只要有请求发送，都会在请求返回之前执行该方法
    public void dobefore(JoinPoint joinPoint) {//在前面之前执行
        ServletRequestAttributes attributes= (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
        HttpServletRequest request= attributes.getRequest();
        String url=request.getRequestURL().toString();//获取url和ip
        String ip =request.getRemoteAddr();
        String classMethod=joinPoint.getSignature().getDeclaringTypeName()+"."+joinPoint.getSignature().getName();
        Object[] args =joinPoint.getArgs();
        RequestLog requestLog=new RequestLog(url,ip,classMethod,args);


        logger.info("debefore......");
        logger.info("Request:{}",requestLog);//记录请求，包括url,ip,类方法,参数

        /*
        *sout:会输出到控制台和日志文件中
        * debefore......
        * Request:RequestLog{url='http://localhost:8080/3/ss', ip='127.0.0.1', ClassMethod='com.example.BlogDemo.web.IndexController.index', args=[3, ss]}
        *  */
    }


    @After("log()")//最终通知
    public void doafter() {
        logger.info("doafter......");
    }

    @AfterReturning(returning = "result", pointcut = "log()")//对请求完之后返回的内容进行记录,
    public void doafterReturn(Object result) {//拦截返回内容
        logger.info("Result:{}", result);
    }

    private class RequestLog{
        private String url;
        private String ip;
        private String ClassMethod;
        private Object[] args;

        public RequestLog(String url, String ip, String classMethod, Object[] args) {
            this.url = url;
            this.ip = ip;
            ClassMethod = classMethod;
            this.args = args;
        }

        @Override
        public String toString() {
            return "RequestLog{" +
                    "url='" + url + '\'' +
                    ", ip='" + ip + '\'' +
                    ", ClassMethod='" + ClassMethod + '\'' +
                    ", args=" + Arrays.toString(args) +
                    '}';
        }
    }
}

```

```
2021-03-05 09:28:18.839  INFO 9716 --- [nio-8080-exec-1] com.example.BlogDemo.aspect.LogAspect    :debefore......
2021-03-05 09:28:18.839  INFO 9716 --- [nio-8080-exec-1] com.example.BlogDemo.aspect.LogAspect    : Request:RequestLog{url='http://localhost:8080/3/ss', ip='127.0.0.1', ClassMethod='com.example.BlogDemo.web.IndexController.index', args=[3, ss]}
```

# 页面处理

1.静态页面导入project

- 将html文件放入templates文件夹中
- 将js，css，图片等放到static文件夹中

2.thymeleaf布局

- 定义fragment
- 使用fragment布局

3.错误页面美化



# 实体设计

实体类：

- 博客类Blog
- 博客分类Type
- 博客标签Tag
- 博客评论Comment
- 用户User

**利用JPA，通过面向对象的方式在数据库中进行数据库表的创建。**

1. 构建实体类
2. 创建实体类之间的关系



新建po文件夹，并进行上诉两步操作

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210305153552.png)

运行项目，会在数据库中自动生成数据表

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210305153737.png)



后台管理功能实现

- dao对数据进行操作

- service实现业务逻辑，在数据库中查找，返回用户
- LoginController,登录控制器。调用service和dao

```java
package com.lrm.web.admin;

import com.lrm.po.User;
import com.lrm.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.servlet.mvc.support.RedirectAttributes;

import javax.servlet.http.HttpSession;

@Controller//控制器
@RequestMapping("/admin")//admin的请求需要在这里进行处理
public class LoginController {
    @Autowired
    private UserService userService;

    @GetMapping()//admin请求会直接跳转到login
    public String loginPage() {
        return "admin/login";//跳转到login页面
    }

    @PostMapping("/login")//
    public String login(@RequestParam String username, @RequestParam String password, HttpSession session, RedirectAttributes attributes) {
        User user = userService.checkUser(username, password);
        if (user != null) {//如果用户正确，将其放入session中
            user.setPassword(null);
            session.setAttribute("user", user);
            return "admin/index";
        } else {//如果用户名和密码不正确
            attributes.addFlashAttribute("message", "用户名和密码错误");
            return "redirect:/admin";//重定向
        }
    }

    @GetMapping("/logout")//用户注销
    public String logout(HttpSession session) {
        session.removeAttribute("user");//将session中的user清空
        return "redirect:/admin";//重定向到admin
    }

}

```

- 登录拦截器

目的：在没有登录的情况下， 不希望其他人能够访问到管理的界面（直接通过地址进行访问，这可能会导致后台的内容被篡改）；所以要加上登录拦截器。

```java
package com.lrm.interceptor;

import org.springframework.web.servlet.handler.HandlerInterceptorAdapter;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Created by limi on 2017/10/15.
 */
public class LoginInterceptor extends HandlerInterceptorAdapter {

    @Override
    public boolean preHandle(HttpServletRequest request,//在请求未到达前进行预处理
                             HttpServletResponse response,
                             Object handler) throws Exception {
        if (request.getSession().getAttribute("user") == null) {//如果未登录
            response.sendRedirect("/admin");//就重定向到登录页面
            return false;
        }
        return true;
    }
}

```

```java
package com.lrm.interceptor;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurerAdapter;

/**
 * Created by limi on 2017/10/15.
 */
@Configuration//配置类
public class WebConfig extends WebMvcConfigurerAdapter {

    @Override//重写控制器过滤设置
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LoginInterceptor())
                .addPathPatterns("/admin/**")
                .excludePathPatterns("/admin")//排除掉一些路径
                .excludePathPatterns("/admin/login");//排除掉一些路径
    }
}

```

# 分类管理


