# SpringBoot


# 1、Spring Boot简介

什么是Spring Boot ：

- Spring Boot用于简化Spring应用开发的一个框架；
- 整个Spring技术栈的一个大整合；
- J2EE开发的一站式解决方案

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210214221909.png)



优点：

- 快速创建独立运行的Spring项目以及与主流框架集成
- 使用嵌入式的Servlet容器，应用无需打包WAR包
- starters自动依赖与版本控制
- 大量的自动配置，简化开发，也可以修改默认值
- 无需配置XML，无代码生成，开箱即用
- 准生产环境的运行时应用监控
- 与云计算的天然集成

# 2、微服务

微服务是一种架构风格

一个应用应该是一组小型服务；可以通过HTTP的方式进行沟通；

每一个功能元素最终都是一个可独立替换和独立升级的软件单元；

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210214223817.png)



![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210226144547.png)



# 基本介绍

## HelloWorld

1. 创建maven工程

2. 引入依赖

   ```xml
   <parent>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-parent</artifactId>
           <version>2.4.3</version>
   </parent>
   
   <dependencies>
       <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
   </dependencies>
   ```

3. 编写主程序

   ```java
   package com.demo;
   
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   
   /*
   *主程序类
   * @SpringBootApplication：这是一个SpringBoot应用
   * */
   @SpringBootApplication
   public class MainApplication {
       public static void main(String[] args) {
           SpringApplication.run(MainApplication.class,args);
       }
   }
   
   ```

4. 编写业务类

   ```java
   package com.demo.controller;
   
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   
   
   @RestController
   public class HelloController {
   
       @RequestMapping("/hello")
       public String handle(){
           return "Hello,world!";
       }
   }
   
   ```

5. 测试

   直接运行主程序

6. 简化配置

   在resources中创建application.properties，在application.properties中编写配置即可

7. 简化部署

   在pom.xml文件中添加

   ```xml
   
       <build>
           <plugins>
               <plugin>
                   <groupId>org.springframework.boot</groupId>
                   <artifactId>spring-boot-maven-plugin</artifactId>
               </plugin>
           </plugins>
       </build>
   
   ```

   将项目打成jar包，直接在目标服务器执行即可。



## SpringBoot特点

### 依赖管理

- 父项目做依赖管理

```xml
依赖管理    
<parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.4.RELEASE</version>
</parent>

他的父项目
 <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.3.4.RELEASE</version>
  </parent>

几乎声明了所有开发中常用的依赖的版本号,自动版本仲裁机制

```

- 开发导入starter场景启动器

```xml
1、见到很多 spring-boot-starter-* ： *就某种场景
2、只要引入starter，这个场景的所有常规需要的依赖我们都自动引入
3、SpringBoot所有支持的场景
https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-starter
4、见到的  *-spring-boot-starter： 第三方为我们提供的简化开发的场景启动器。
5、所有场景启动器最底层的依赖
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter</artifactId>
  <version>2.3.4.RELEASE</version>
  <scope>compile</scope>
</dependency>
```

- 无需关注版本号，自动版本仲裁

```
1、引入依赖默认都可以不写版本
2、引入非版本仲裁的jar，要写版本号。
```

- 可以修改默认版本号

```xml
1、查看spring-boot-dependencies里面规定当前依赖的版本 用的 key。
2、在当前项目里面重写配置,在pom.xml中引入修改
    <properties>
        <mysql.version>5.1.43</mysql.version>
    </properties>
```

### 自动配置

- 自动配好Tomcat

  - 引入Tomcat依赖。
  - 配置Tomcat

```xml
	<dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-tomcat</artifactId>
      <version>2.3.4.RELEASE</version>
      <scope>compile</scope>
    </dependency>
```

- 自动配好SpringMVC

  - 引入SpringMVC全套组件
  - 自动配好SpringMVC常用组件（功能）

- 自动配好Web常见功能，如：字符编码问题

  - SpringBoot帮我们配置好了所有web开发的常见场景

- 默认的包结构

  - <u>主程序所在包及其下面的所有子包里面的组件都会被默认扫描进来</u>
  - 无需以前的包扫描配置
  - 想要改变扫描路径，@SpringBootApplication(scanBasePackages=**"com.atguigu"**)

 - 或者@ComponentScan 指定扫描路径

```xml
@SpringBootApplication
等同于
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan("com.atguigu.boot")
```

- 各种配置拥有默认值

  - 默认配置最终都是映射到某个类上，如：MultipartProperties
  - 配置文件的值最终会绑定某个类上，这个类会在容器中创建对象

- 按需加载所有自动配置项

  - 非常多的starter
  - 引入了哪些场景这个场景的自动配置才会开启
  - SpringBoot所有的自动配置功能都在 spring-boot-autoconfigure 包里面

......

## 容器功能

### 组件添加

创建两个类

```java
package com.demo;

public class User {
    private String name;
    private int age;

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void setAge(int age) {
        this.age = age;
    }

//    public String getPet() {
//        return new User();
//    }
}

```

```java
package com.demo;

public class Pet {
    private String name;

    @Override
    public String toString() {
        return "Pet{" +
                "name='" + name + '\'' +
                '}';
    }

    public Pet(){

    }

    public Pet(String name) {
        this.name = name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

```

用Spring创建容器的一种基本方式是使用xml文件进行配置，例如

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <!--进行对象创建和属性注入-->
    <bean id="user" class="com.demo.User">
<!--        <property name="name" value="zhangsan"></property>-->
<!--        <property name="age" value="10"></property>-->
        <constructor-arg name="name" value="zhangsan"></constructor-arg>
        <constructor-arg name="age" value="10"></constructor-arg>
    </bean>

    <bean id="cat" class="com.demo.Pet">
        <property name="name" value="dog"></property>
    </bean>
</beans>
```

使用SpringBoot后可以不用编写xml配置文件，而是使用更加简单的方式进行容器的创建。如下：

创建一个配置文件类

#### @Configuration

使用@Configuration告诉SpringBoot这是一个配置类。这里需要注意@Configuration的选项`proxyBeanMethods`

#### @Bean

使用@Bean标注在方法上，为容器添加组件，包括了创建对象和属性注入。`方法名作为容器中的对象名`

```java
package com.demo.config;

import com.demo.Pet;
import com.demo.User;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.jws.soap.SOAPBinding;


/*
* 1. 配置类中使用@Bean标注在方法上给容器注册组件，默认是单实例
* 2. 配置类本身也是组件
* 3.proxyBeanMethods: 代理bean的方法
*   Full(proxyBeanMethods = true)、【保证每个@Bean方法被调用多少次返回的组件都是单实例的】
*   Lite(proxyBeanMethods = false)【每个@Bean方法被调用多少次返回的组件都是新创建的】
*   组件依赖必须使用Full模式默认。其他默认是否Lite模式
* */
@Configuration(proxyBeanMethods = true)  //告诉SpringBoot这是一个配置类  ==配置文件
public class MyConfig {

    /*
    * Full: 外部无论对配置类中的这个组件注册方法调用多少次获取的都是之前注册容器中的单实例对象
    * */
    @Bean //给容器中添加组件。以方法名作为组件id,返回类型为组件类型。返回的值就是容器中的实例
    public User user01(){
        return new User("zhangsan",18);
    }

    public Pet tomcat(){
        return new Pet("cat");
    }
}

```

#### @Import

给容器中导入组件。

```java
 * 4、@Import({User.class, DBHelper.class})
 *      给容器中自动创建出这两个类型的组件、默认组件的名字就是全类名
 *
 *
 *
 */

@Import({User.class, DBHelper.class})
@Configuration(proxyBeanMethods = false) //告诉SpringBoot这是一个配置类 == 配置文件
public class MyConfig {
}
```

#### @Conditional

条件装配，满足Conditional指定条件，则进行组件注入

#### @ImportResource

原生配置文件引入,即将xml配置文件进行解析导入

```xml
======================beans.xml=========================
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <bean id="haha" class="com.atguigu.boot.bean.User">
        <property name="name" value="zhangsan"></property>
        <property name="age" value="18"></property>
    </bean>

    <bean id="hehe" class="com.atguigu.boot.bean.Pet">
        <property name="name" value="tomcat"></property>
    </bean>
</beans>
```

```java
@ImportResource("classpath:beans.xml")
public class MyConfig {}

======================测试=================
        boolean haha = run.containsBean("haha");
        boolean hehe = run.containsBean("hehe");
        System.out.println("haha："+haha);//true
        System.out.println("hehe："+hehe);//true
```



### 配置绑定

如何使用Java读取到properties文件中的内容，并且把它封装到JavaBean中，以供随时使用；

一般的使用方式

```java
public class getProperties {
     public static void main(String[] args) throws FileNotFoundException, IOException {
         Properties pps = new Properties();
         pps.load(new FileInputStream("a.properties"));
         Enumeration enum1 = pps.propertyNames();//得到配置文件的名字
         while(enum1.hasMoreElements()) {
             String strKey = (String) enum1.nextElement();
             String strValue = pps.getProperty(strKey);
             System.out.println(strKey + "=" + strValue);
             //封装到JavaBean。
         }
     }
 }
```

#### @Component + @ConfigurationProperties

```xml
# application.properties文件中的内容

mycar.brand=BYD
mycar.price=10000

```



```java
package com.demo;


import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

/*
* 只有在容器中的组件，才会拥有SpringBoot提供的强大功能
* */
@Component//使得Car成为容器组件，
@ConfigurationProperties(prefix = "mycar")//mycar相当于Car的一个实例，其属性值brand和price写在配置文件中。将application.properties中的mycar的属性进行绑定

public class Car {
    private String brand;
    private Integer price;

    @Override
    public String toString() {
        return "Car{" +
                "brand='" + brand + '\'' +
                ", price=" + price +
                '}';
    }

    public String getBrand() {
        return brand;
    }

    public Integer getPrice() {
        return price;
    }

    public void setBrand(String brand) {
        this.brand = brand;
    }

    public void setPrice(Integer price) {
        this.price = price;
    }
}

```

#### @EnableConfigurationProperties

```java
# @Component//使得Car成为容器组件，
@ConfigurationProperties(prefix = "mycar")//mycar相当于Car的一个实例，其属性值brand和price写在配置文件中
public class Car {
    ...
}
```

```java
#可以不使用@Component

@EnableConfigurationProperties(Car.class)
//1、开启Car配置绑定功能
//2、把这个Car这个组件自动注册到容器中
public class MyConfig {
    ...
}
```

小结：配置绑定的两种方式

1. 在Car类前面使用

   ```java
   @Component //使得Car成为容器组件，
   @ConfigurationProperties(prefix = "mycar")
   ```

2. 在Car类前使用

```java
@ConfigurationProperties(prefix = "mycar")
```

在MyConfig前面使用

```java
@EnableConfigurationProperties(Car.class)
```



## 自动配置原理

### 引导加载自动配置类

```java
@SpringBootApplication：表示这是一个SpringBoot应用
```

由以下三部分注解构成

```java
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
```

#### @SpringBootConfiguration

表示当前是一个配置类

#### @ComponentScan

指定扫描哪些

#### @EnableAutoConfiguration

@EnableAutoConfiguration由以下两个注解构成：

```java
@AutoConfigurationPackage
@Import(AutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration {}
```

##### @AutoConfigurationPackage

自动配置包，指定了默认的包规则

```java
@Import(AutoConfigurationPackages.Registrar.class)  //给容器中导入一个组件
public @interface AutoConfigurationPackage {}

//利用Registrar给容器中导入一系列组件
//将指定的一个包下的所有组件导入进来？MainApplication 所在包下。
```

##### @Import(AutoConfigurationImportSelector.class)

小结：通过@Import(AutoConfigurationImportSelector.class)默认加载127个自动配置类（将其放入容器中），但是会按照条件 装配规则进行按需装配

```java
1、利用getAutoConfigurationEntry(annotationMetadata);给容器中批量导入一些组件
2、调用List<String> configurations = getCandidateConfigurations(annotationMetadata, attributes)获取到所有需要导入到容器中的配置类
3、利用工厂加载 Map<String, List<String>> loadSpringFactories(@Nullable ClassLoader classLoader)；得到所有的组件
4、从META-INF/spring.factories位置来加载一个文件。
	默认扫描我们当前系统里面所有META-INF/spring.factories位置的文件
    spring-boot-autoconfigure-2.3.4.RELEASE.jar包里面也有META-INF/spring.factories
    
```

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210227083746.png)

```xml
文件里面写死了spring-boot一启动就要给容器中加载的所有配置类
```

### 按需开启自动配置类

```
虽然我们127个场景的所有自动配置启动的时候默认全部加载。xxxxAutoConfiguration
按照条件装配规则（@Conditional），最终会按需配置。
```



### 修改默认配置

SpringBoot默认会在底层配好所有的组件。但是如果用户自己配置了以用户的优先

总结：

- SpringBoot先<u>加载</u>所有的自动配置类  xxxxxAutoConfiguration  
- 每个自动配置类**按照条件**进行<u>生效</u>，默认都会绑定配置文件指定的值。xxxxProperties里面拿。xxxProperties和配置文件进行了绑定
- **生效**的配置类就会给容器中<u>装配</u>很多组件
- 只要容器中有这些组件，相当于这些功能就有了
- 定制化配置

- - 用户直接自己@Bean替换底层的组件
  - 用户去看这个组件是获取的配置文件什么值就去修改。

**xxxxxAutoConfiguration ---> 组件  --->** **xxxxProperties里面拿值  ----> application.properties**

> 相当于使用@Configuration创建配置类  -->按条件生效--> 然后再配置类中进行容器组件的装配，包括对象创建和属性注入(在application.properties进行配置绑定，类似于使用@ConfigurationProperties或@EnableConfigurationProperties。
>
> 定制化配置就就相当于在配置类中用@Bean进行组件的更改。



### 最佳实践

- 引入父项目

```xml
<parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.4.RELEASE</version>
    </parent>
```



- 引入场景依赖

- https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-starter

- 例如开发web引用，需要引入

- ```xml
  <dependencies>
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-web</artifactId>
          <version>2.3.4.RELEASE</version>
          <scope>compile</scope>
      </dependency>
  </dependencies>
  ```

- 

- 查看自动配置了哪些（选做）

- - 自己分析，引入场景对应的自动配置一般都生效了
  - 配置文件中debug=true开启自动配置报告。Negative（不生效）\Positive（生效）

- 是否需要修改

- - 参照文档修改配置项

- - - https://docs.spring.io/spring-boot/docs/current/reference/html/appendix-application-properties.html#common-application-properties
    - 自己分析。xxxxProperties绑定了配置文件的哪些。

- - 自定义加入或者替换组件

- - - @Bean、@Component。。。

- - 自定义器  **XXXXXCustomizer**；
  - ......



## 开发小技巧

### Lombok

在创建类的时候可以不用写set,get以及构造方法，Lombok会在程序编译的时候自动创建。

```xml
<dependency>
   <groupId>org.projectlombok</groupId>
   <artifactId>lombok</artifactId>
</dependency>

idea中搜索安装lombok插件
```

```java

===============================简化JavaBean开发===================================
@NoArgsConstructor//无参构造
@AllArgsConstructor//有参构造
@Data//get,set方法
@ToString//toString方法
@EqualsAndHashCode
public class User {

    private String name;
    private Integer age;

    private Pet pet;

    public User(String name,Integer age){
        this.name = name;
        this.age = age;
    }


}



================================简化日志开发===================================
@Slf4j//日志类
@RestController
public class HelloController {
    @RequestMapping("/hello")
    public String handle01(@RequestParam("name") String name){
        
        log.info("请求进来了....");
        
        return "Hello, Spring Boot 2!"+"你好："+name;
    }
}
```



### dev-tools

自动重启

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <optional>true</optional>
        </dependency>
```

项目或者页面修改以后：Ctrl+F9；

不用在每次修改以后重新启动，只要使用ctrl+F9进行重新编译一遍就行

### Spring Initailizr（项目初始化向导）

# 核心介绍

## 配置文件

### 文件类型

#### properties

同以前的properties用法，（使用配置绑定，例如：@Component + @ConfigurationProperties）

#### yaml

`只是和properties文件在编写语法上有差别`

YAML 是 "YAML Ain't Markup Language"（YAML 不是一种标记语言）的递归缩写。在开发的这种语言时，YAML 的意思其实是："Yet Another Markup Language"（仍是一种标记语言）。 

非常适合用来做以数据为中心的配置文件

##### 基本语法

- key: value；kv之间有空格
- 大小写敏感
- 使用缩进表示层级关系
- 缩进不允许使用tab，只允许空格
- 缩进的空格数不重要，只要相同层级的元素左对齐即可
- '#'表示注释
- 字符串无需加引号，如果要加，''与""表示字符串内容 会被 转义/不转义

##### 数据类型

- 字面量：单个的、不可再分的值。date、boolean、string、number、null

```
k: v
```

- 对象：键值对的集合。map、hash、set、object

```
行内写法：  k: {k1:v1,k2:v2,k3:v3}
#或
k: 
  k1: v1
  k2: v2
  k3: v3
```

- 数组：一组按次序排列的值。array、list、queue

```
行内写法：  k: [v1,v2,v3]
#或者
k:
 - v1
 - v2
 - v3
```

##### 示例

创建两个类

```java
@Data
public class Person {
	
	private String userName;
	private Boolean boss;
	private Date birth;
	private Integer age;
	private Pet pet;
	private String[] interests;
	private List<String> animal;
	private Map<String, Object> score;
	private Set<Double> salarys;
	private Map<String, List<Pet>> allPets;
}


@Data
public class Pet {
	private String name;
	private Double weight;
}
```



```yaml
# yaml表示以上对象
person:
  userName: zhangsan
  boss: false
  birth: 2019/12/12 20:12:33
  age: 18
  pet: 
    name: tomcat
    weight: 23.4
  interests: [篮球,游泳]
  animal: 
    - jerry
    - mario
  score:
    english: 
      first: 30
      second: 40
      third: 50
    math: [131,140,148]
    chinese: {first: 128,second: 136}
  salarys: [3999,4999.98,5999.99]
  allPets:
    sick:
      - {name: tom}
      - {name: jerry,weight: 47}
    health: [{name: mario,weight: 47}]
```

### 配置提示

自定义的类和配置文件绑定一般没有提示。

可以在pom.xml中加入以下依赖

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>


 <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <excludes>
                        <exclude>
                            <groupId>org.springframework.boot</groupId>
                            <artifactId>spring-boot-configuration-processor</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
    </build>
```

## web开发

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210227111257.png)

### 简单功能分析

#### 静态资源访问

[参考](https://blog.csdn.net/bird_tp/article/details/106098481)

**默认静态资源文件夹路径**放在resources下的/static 或 /public 或 /resources 或 `/META-INF/resources`

访问 ： 当前项目根路径/ + 静态资源名 `http://localhost:8080/20026.jpg`

原理： 静态映射/**。

**请求进来，先去找Controller看能不能处理。不能处理的所有请求又都交给静态资源处理器。静态资源也找不到则响应404页面**

可以进行以下两点修改：

```yaml
#1.改变访问静态资源的访问前缀
#此时访问静态资源需要使用 http://localhost:8080/res/20026.jpg  ->加一个前缀
spring:
  mvc:
    static-path-pattern: /res/**
    
    
#2.改变静态资源文件夹的路径
  resources:
    static-locations: [classpath:/haha/]
```

#### 欢迎页支持

在静态资源文件路径中放置index.html可以进行欢迎页设置

此时访问http://localhost:8080/可以显示在index.html中内容。

例如在static文件夹中创建index.html:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>北京欢迎您!</h1>
</body>
</html>
```

此时访问http://localhost:8080/ ，显示：

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210228220454.png)

注意以下两点：

- 可以配置静态资源路径
- 但是不可以配置静态资源的访问前缀。否则导致 index.html不能被默认访问

```yaml
spring:
#  mvc:
#    static-path-pattern: /res/**   这个会导致welcome page功能失效

  resources:
    static-locations: [classpath:/haha/]
```

controller能处理/index

#### 自定义favicon

即设置网站的小图标

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210228221113.png)

favicon.ico 放在静态资源目录下即可。

```yaml
spring:
#  mvc:
#    static-path-pattern: /res/**   这个会导致 Favicon 功能失效
```

#### 静态资源配置原理

待学习...p26-p29

### 请求参数处理

### 数据相应与内容协商

### 视图解析与模板引擎

#### 视图解析原理

#### 模板引擎-Thymeleaf

什么是模板引擎：[参考](https://www.cnblogs.com/dojo-lzz/p/5518474.html)

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210228224739.png)



##### Thymeleaf基本语法

###### 1 、表达式

| 表达式名字 | 语法   | 用途                               |
| ---------- | ------ | ---------------------------------- |
| 变量取值   | ${...} | 获取请求域、session域、对象等值    |
| 选择变量   | *{...} | 获取上下文对象值                   |
| 消息       | #{...} | 获取国际化等值                     |
| 链接       | @{...} | 生成链接                           |
| 片段表达式 | ~{...} | jsp:include 作用，引入公共页面片段 |

###### 2、字面量

文本值: **'one text'** **,** **'Another one!'** **,…**数字: **0** **,** **34** **,** **3.0** **,** **12.3** **,…**布尔值: **true** **,** **false**

空值: **null**

变量： one，two，.... 变量不能有空格

###### 3、文本操作

字符串拼接: **+**

变量替换: **|The name is ${name}|** 

###### 4、数学运算

运算符: + , - , * , / , %

###### 5、布尔运算

运算符:  **and** **,** **or**

一元运算: **!** **,** **not** 

###### 6、比较运算

比较: **>** **,** **<** **,** **>=** **,** **<=** **(** **gt** **,** **lt** **,** **ge** **,** **le** **)**等式: **==** **,** **!=** **(** **eq** **,** **ne** **)** 

###### 7、条件运算

If-then: **(if) ? (then)**

If-then-else: **(if) ? (then) : (else)**

Default: (value) **?: (defaultvalue)** 

###### 8、特殊操作

无操作： _





#### Thymeleaf的使用

##### 引入starter

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
```

##### 自动配置好了thymeleaf

```java
#以下是自动配置的关键注解
@Configuration(proxyBeanMethods = false)
@EnableConfigurationProperties(ThymeleafProperties.class)
@ConditionalOnClass({ TemplateMode.class, SpringTemplateEngine.class })
@AutoConfigureAfter({ WebMvcAutoConfiguration.class, WebFluxAutoConfiguration.class })
public class ThymeleafAutoConfiguration { }
```

自动配好的策略

- 1、所有thymeleaf的配置值都在 ThymeleafProperties
- 2、配置好了 **SpringTemplateEngine**    -->模板引擎
- **3、配好了** **ThymeleafViewResolver**  -->视图解析器
- 4、我们只需要直接开发页面

开发页面需要遵守的一些规则：

- 页面需要放在templates文件夹中
- 页面文件的后缀是.html

```java
	public static final String DEFAULT_PREFIX = "classpath:/templates/";

	public static final String DEFAULT_SUFFIX = ".html";  //xxx.html
```

##### 页面开发

创建ViewTestController，在ViewTestController中编写

```java
package com.example.demo.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class ViewTestController {
    @GetMapping("/beijing")
    public String beijing(Model model){

//        放在model中的数据会自动放在请求域中，也就是相当于调用了request.setAttribute("a",aa)
        model.addAttribute("msg","beijing,nihao");//将msg设置为"beijing,nihao"。
        model.addAttribute("link","https://www.baidu.com/");//将link设置为"https://www.baidu.com/"
        return "success";可以将success.html中的文本或属性进行替换
    }
}
```

创建success.html，并进行编写：



```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">  <!--引入thymeleaf命名空间-->
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1 th:text="${msg}">哈哈</h1>  <!--使用th:text="${msg}"进行文本的替换，即将"哈哈"替换为msg,其中msg是在ViewTestController中定义的-->
<h2>
    <!-- 使用th:href="@{}"进行属性值替换，即将www.atguigu.com替换为link中的链接-->
    <a href="www.atguigu.com" th:href="${link}">去百度</a> <br/> 
    <a href="www.atguigu.com" th:href="@{link}">去百度2</a>
</h2>
</body>
</html>
```

```
th:href="${}"和th:href="@{}"的不同之处在于
1. th:href="${}"相当于真正的取出了link值
2. th:href="@{}"


```

`再次注意thymeleaf做的实际上就是将模板文件中的字符利用数据进行替换，在本例中模板文件就是success.html，数据就来自ViewTestController。`

#### 构建后台管理系统

##### 项目创建

选择：thymeleaf、web-starter、devtools、lombok

##### 静态资源处理

自动配置好，我们只需要把所有静态资源放到 static 文件夹下，包括

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210301092519.png)



##### **处理登录页面**

将login.html放在templatesw文件夹中，（所有模板文件都放在templates），将index.html重命名为main.html放在templatesw文件夹中

需求：

```
1. 当给页面发送"/", "/login"时，会返回login.html中的内容,会跳转到登录页面
2. 在login.html登录成功，重定向到main.html ,重定向防止表单重复提交，如果不使用重定向，每一次刷新主页面都会重新回到登录（login）页面
3. 处理登录请求,发送账户名和密码就会来到主页，分为两种情况
	3.1 输入的账号密码不能为空
	3.2 当session中没有存储账号密码密码时，不能够直接访问主页。-->每次登录成功会将账号密码存储在session中。
4. 登录成功后会主页显示账号名
```

创建IndexConteoller

```java
package com.example.demo.Controller;

import com.example.demo.bean.User;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.util.StringUtils;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;

import javax.servlet.http.HttpSession;

//用于负责页面跳转
@Controller
public class IndexController {
//去登录页
    @GetMapping(value = {"/", "/login"}) //当给页面发送"/", "/login"时，会返回login.html中的内容
    public String loginpage() {
        return "login";
    }

//    处理登录请求,发送账户名和密码就会来到主页
    @PostMapping("/login")
    public String main(User user, HttpSession session, Model model){
        if (StringUtils.hasLength(user.getUserName()) && StringUtils.hasLength(user.getPassWord())){
//            把登录成功的用户保存起来
            session.setAttribute("loginUser",user);
            // 登录成功，重定向到main.html ,重定向防止表单重复提交，如果不使用重定向，每一次刷新主页面都会重新回到登录（login）页面
            return "redirect:/main.html";
//            return "main";
        }else {
            model.addAttribute("msg","账号密码不能为空");//设置提示信息
//            否则回到登录页
            return "login";
        }

    }

    /*
    * 去main页面
    * */
    @GetMapping("/main.html")
    public String mainPage(HttpSession session,Model model){
//        判断是否登录，使用拦截器，过滤器
        Object loginUser=session.getAttribute("loginUser");
        if (loginUser!=null){
            return "main";
        }else {
            model.addAttribute("msg","当前未登录，请重新登陆");//设置提示信息
//            否则回到登录页
            return "login";
        }
    }
}
```

修改login.html

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210301111120.png)

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210301110037.png)

修改main.html

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210301110203.png)





##### table页面

即

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210301111600.png)

引入几个与table相关的html文件以及创建TableController，如下：

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210301111758.png)



```java
package com.example.demo.Controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class TableController {

    @GetMapping("/basic_table.html")
    public String basic_table(){
        return "table/basic_table";
    }

    @GetMapping("/dynamic_table.html")
    public String dynamic_table(){
        return "table/dynamic_table";
    }

    @GetMapping("/responsive_table.html")
    public String responsive_table(){
        return "table/responsive_table";
    }

    @GetMapping("/editable_table.html")
    public String editable_table(){
        return "table/editable_table";
    }


}

```

##### 抽取公共信息

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210301112646.png)

左侧栏和上侧栏的信息是公共的，即如上图红框部分。而中间的部分是可以变动的，如绿框部分。

抽取模板：

[链接](https://www.thymeleaf.org/doc/tutorials/2.1/usingthymeleaf.html#template-layout)

给公共的部分进行声明

```
方法1：th:fragment
方法2：id   -->用id声明的引用时要加#号

```

引用声明的部分

```
th:insert
th:include
th:replace 
```

创建common.html，用于存放左侧和上侧栏的代码

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">


<head th:fragment="commonheader"> <!--使用th:fragment="commonheader"声明这是一个公共头-->
<!--    css,js-->
    <!--common-->

    <!--将所有公共资源的超链接地址使用th:... "@{/}",  /表示当前路径，这样会动态给我们加上项目名，这样在项目部署的时候如果想要修改项目名就不需要需改源代码了-->
    <link href="css/style.css" th:href="@{/css/style.css}" rel="stylesheet">
    <link href="css/style-responsive.css" th:href="@{/css/style-responsive.css}" rel="stylesheet">

    <!-- HTML5 shim and Respond.js IE8 support of HTML5 elements and media queries -->
    <!--[if lt IE 9]>
    <script src="js/html5shiv.js" th:src="@{/js/html5shiv.js}"></script>
    <script src="js/respond.min.js" th:src="@{/js/respond.min.js}"></script>
    <![endif]-->
</head>


<body>
<!--公共部分：-->

<!--左侧菜单    -->
<!-- left side start-->
<div id="leftmenu" class="left-side sticky-left-side">

    <!--logo and iconic logo start-->
    <div class="logo">
        <a href="main.html"><img src="images/logo.png" alt=""></a>
    </div>

    <div class="logo-icon text-center">
        <a href="main.html"><img src="images/logo_icon.png" alt=""></a>
    </div>
    <!--logo and iconic logo end-->

    <div class="left-side-inner">

        <!-- visible to small devices only -->
        <div class="visible-xs hidden-sm hidden-md hidden-lg">
            <div class="media logged-user">
                <img alt="" src="images/photos/user-avatar.png" class="media-object">
                <div class="media-body">
                    <h4><a href="#">John Doe</a></h4>
                    <span>"Hello There..."</span>
                </div>
            </div>

            <h5 class="left-nav-title">Account Information</h5>
            <ul class="nav nav-pills nav-stacked custom-nav">
                <li><a href="#"><i class="fa fa-user"></i> <span>Profile</span></a></li>
                <li><a href="#"><i class="fa fa-cog"></i> <span>Settings</span></a></li>
                <li><a href="#"><i class="fa fa-sign-out"></i> <span>Sign Out</span></a></li>
            </ul>
        </div>

        <!--sidebar nav start-->
        <ul class="nav nav-pills nav-stacked custom-nav">
            <li class="menu-list nav-active"><a href="main.html"><i class="fa fa-home"></i> <span>Dashboard</span></a>
                <ul class="sub-menu-list">
                    <li><a href="index_alt.html"> Dashboard 1</a></li>
                    <li class="active"><a href="main.html"> Dashboard 2</a></li>
                </ul>
            </li>

            <li class="menu-list"><a href=""><i class="fa fa-laptop"></i> <span>Layouts</span></a>
                <ul class="sub-menu-list">
                    <li><a href="blank_page.html"> Blank Page</a></li>
                    <li><a href="boxed_view.html"> Boxed Page</a></li>
                    <li><a href="leftmenu_collapsed_view.html"> Sidebar Collapsed</a></li>
                    <li><a href="horizontal_menu.html"> Horizontal Menu</a></li>

                </ul>
            </li>
            <li class="menu-list"><a href=""><i class="fa fa-book"></i> <span>UI Elements</span></a>
                <ul class="sub-menu-list">
                    <li><a href="general.html"> General</a></li>
                    <li><a href="buttons.html"> Buttons</a></li>
                    <li><a href="tabs-accordions.html"> Tabs & Accordions</a></li>
                    <li><a href="typography.html"> Typography</a></li>
                    <li><a href="slider.html"> Slider</a></li>
                    <li><a href="panels.html"> Panels</a></li>
                    <li><a href="widgets.html"> Widgets</a></li>
                </ul>
            </li>
            <li class="menu-list"><a href=""><i class="fa fa-cogs"></i> <span>Components</span></a>
                <ul class="sub-menu-list">
                    <li><a href="grids.html"> Grids</a></li>
                    <li><a href="gallery.html"> Media Gallery</a></li>
                    <li><a href="calendar.html"> Calendar</a></li>
                    <li><a href="tree_view.html"> Tree View</a></li>
                    <li><a href="nestable.html"> Nestable</a></li>

                </ul>
            </li>

            <li><a href="fontawesome.html"><i class="fa fa-bullhorn"></i> <span>Fontawesome</span></a></li>

            <li class="menu-list"><a href=""><i class="fa fa-envelope"></i> <span>Mail</span></a>
                <ul class="sub-menu-list">
                    <li><a href="mail.html"> Inbox</a></li>
                    <li><a href="mail_compose.html"> Compose Mail</a></li>
                    <li><a href="mail_view.html"> View Mail</a></li>
                </ul>
            </li>

            <li class="menu-list"><a href=""><i class="fa fa-tasks"></i> <span>Forms</span></a>
                <ul class="sub-menu-list">
                    <li><a href="form_layouts.html"> Form Layouts</a></li>
                    <li><a href="form_advanced_components.html"> Advanced Components</a></li>
                    <li><a href="form_wizard.html"> Form Wizards</a></li>
                    <li><a href="form_validation.html"> Form Validation</a></li>
                    <li><a href="editors.html"> Editors</a></li>
                    <li><a href="inline_editors.html"> Inline Editors</a></li>
                    <li><a href="pickers.html"> Pickers</a></li>
                    <li><a href="dropzone.html"> Dropzone</a></li>
                </ul>
            </li>
            <li class="menu-list"><a href=""><i class="fa fa-bar-chart-o"></i> <span>Charts</span></a>
                <ul class="sub-menu-list">
                    <li><a href="flot_chart.html"> Flot Charts</a></li>
                    <li><a href="morris.html"> Morris Charts</a></li>
                    <li><a href="chartjs.html"> Chartjs</a></li>
                    <li><a href="c3chart.html"> C3 Charts</a></li>
                </ul>
            </li>
            <li class="menu-list"><a href="#"><i class="fa fa-th-list"></i> <span>Data Tables</span></a>
                <ul class="sub-menu-list">
                    <li><a href="basic_table.html"> Basic Table</a></li>
                    <li><a href="dynamic_table.html"> Advanced Table</a></li>
                    <li><a href="responsive_table.html"> Responsive Table</a></li>
                    <li><a href="editable_table.html"> Edit Table</a></li>
                </ul>
            </li>

            <li class="menu-list"><a href="#"><i class="fa fa-map-marker"></i> <span>Maps</span></a>
                <ul class="sub-menu-list">
                    <li><a href="google_map.html"> Google Map</a></li>
                    <li><a href="vector_map.html"> Vector Map</a></li>
                </ul>
            </li>
            <li class="menu-list"><a href=""><i class="fa fa-file-text"></i> <span>Extra Pages</span></a>
                <ul class="sub-menu-list">
                    <li><a href="profile.html"> Profile</a></li>
                    <li><a href="invoice.html"> Invoice</a></li>
                    <li><a href="pricing_table.html"> Pricing Table</a></li>
                    <li><a href="timeline.html"> Timeline</a></li>
                    <li><a href="blog_list.html"> Blog List</a></li>
                    <li><a href="blog_details.html"> Blog Details</a></li>
                    <li><a href="directory.html"> Directory </a></li>
                    <li><a href="chat.html"> Chat </a></li>
                    <li><a href="404.html"> 404 Error</a></li>
                    <li><a href="500.html"> 500 Error</a></li>
                    <li><a href="registration.html"> Registration Page</a></li>
                    <li><a href="lock_screen.html"> Lockscreen </a></li>
                </ul>
            </li>
            <li><a href="login.html"><i class="fa fa-sign-in"></i> <span>Login Page</span></a></li>

        </ul>
        <!--sidebar nav end-->

    </div>
</div>
<!-- left side end-->

<!--主菜单的头部-->
<!-- header section start-->
<!-- header section start-->
<div th:fragment="headermenu" class="header-section">

    <!--toggle button start-->
    <a class="toggle-btn"><i class="fa fa-bars"></i></a>
    <!--toggle button end-->

    <!--search start-->
    <form class="searchform" action="http://view.jqueryfuns.com/2014/4/10/7_df25ceea231ba5f44f0fc060c943cdae/index.html" method="post">
        <input type="text" class="form-control" name="keyword" placeholder="Search here..." />
    </form>
    <!--search end-->

    <!--notification menu start -->
    <div class="menu-right">
        <ul class="notification-menu">
            <li>
                <a href="#" class="btn btn-default dropdown-toggle info-number" data-toggle="dropdown">
                    <i class="fa fa-tasks"></i>
                    <span class="badge">8</span>
                </a>
                <div class="dropdown-menu dropdown-menu-head pull-right">
                    <h5 class="title">You have 8 pending task</h5>
                    <ul class="dropdown-list user-list">
                        <li class="new">
                            <a href="#">
                                <div class="task-info">
                                    <div>Database update</div>
                                </div>
                                <div class="progress progress-striped">
                                    <div style="width: 40%" aria-valuemax="100" aria-valuemin="0" aria-valuenow="40" role="progressbar" class="progress-bar progress-bar-warning">
                                        <span class="">40%</span>
                                    </div>
                                </div>
                            </a>
                        </li>
                        <li class="new">
                            <a href="#">
                                <div class="task-info">
                                    <div>Dashboard done</div>
                                </div>
                                <div class="progress progress-striped">
                                    <div style="width: 90%" aria-valuemax="100" aria-valuemin="0" aria-valuenow="90" role="progressbar" class="progress-bar progress-bar-success">
                                        <span class="">90%</span>
                                    </div>
                                </div>
                            </a>
                        </li>
                        <li>
                            <a href="#">
                                <div class="task-info">
                                    <div>Web Development</div>
                                </div>
                                <div class="progress progress-striped">
                                    <div style="width: 66%" aria-valuemax="100" aria-valuemin="0" aria-valuenow="66" role="progressbar" class="progress-bar progress-bar-info">
                                        <span class="">66% </span>
                                    </div>
                                </div>
                            </a>
                        </li>
                        <li>
                            <a href="#">
                                <div class="task-info">
                                    <div>Mobile App</div>
                                </div>
                                <div class="progress progress-striped">
                                    <div style="width: 33%" aria-valuemax="100" aria-valuemin="0" aria-valuenow="33" role="progressbar" class="progress-bar progress-bar-danger">
                                        <span class="">33% </span>
                                    </div>
                                </div>
                            </a>
                        </li>
                        <li>
                            <a href="#">
                                <div class="task-info">
                                    <div>Issues fixed</div>
                                </div>
                                <div class="progress progress-striped">
                                    <div style="width: 80%" aria-valuemax="100" aria-valuemin="0" aria-valuenow="80" role="progressbar" class="progress-bar">
                                        <span class="">80% </span>
                                    </div>
                                </div>
                            </a>
                        </li>
                        <li class="new"><a href="">See All Pending Task</a></li>
                    </ul>
                </div>
            </li>
            <li>
                <a href="#" class="btn btn-default dropdown-toggle info-number" data-toggle="dropdown">
                    <i class="fa fa-envelope-o"></i>
                    <span class="badge">5</span>
                </a>
                <div class="dropdown-menu dropdown-menu-head pull-right">
                    <h5 class="title">You have 5 Mails </h5>
                    <ul class="dropdown-list normal-list">
                        <li class="new">
                            <a href="">
                                <span class="thumb"><img src="images/photos/user1.png" alt="" /></span>
                                <span class="desc">
                                          <span class="name">John Doe <span class="badge badge-success">new</span></span>
                                          <span class="msg">Lorem ipsum dolor sit amet...</span>
                                        </span>
                            </a>
                        </li>
                        <li>
                            <a href="">
                                <span class="thumb"><img src="images/photos/user2.png" alt="" /></span>
                                <span class="desc">
                                          <span class="name">Jonathan Smith</span>
                                          <span class="msg">Lorem ipsum dolor sit amet...</span>
                                        </span>
                            </a>
                        </li>
                        <li>
                            <a href="">
                                <span class="thumb"><img src="images/photos/user3.png" alt="" /></span>
                                <span class="desc">
                                          <span class="name">Jane Doe</span>
                                          <span class="msg">Lorem ipsum dolor sit amet...</span>
                                        </span>
                            </a>
                        </li>
                        <li>
                            <a href="">
                                <span class="thumb"><img src="images/photos/user4.png" alt="" /></span>
                                <span class="desc">
                                          <span class="name">Mark Henry</span>
                                          <span class="msg">Lorem ipsum dolor sit amet...</span>
                                        </span>
                            </a>
                        </li>
                        <li>
                            <a href="">
                                <span class="thumb"><img src="images/photos/user5.png" alt="" /></span>
                                <span class="desc">
                                          <span class="name">Jim Doe</span>
                                          <span class="msg">Lorem ipsum dolor sit amet...</span>
                                        </span>
                            </a>
                        </li>
                        <li class="new"><a href="">Read All Mails</a></li>
                    </ul>
                </div>
            </li>
            <li>
                <a href="#" class="btn btn-default dropdown-toggle info-number" data-toggle="dropdown">
                    <i class="fa fa-bell-o"></i>
                    <span class="badge">4</span>
                </a>
                <div class="dropdown-menu dropdown-menu-head pull-right">
                    <h5 class="title">Notifications</h5>
                    <ul class="dropdown-list normal-list">
                        <li class="new">
                            <a href="">
                                <span class="label label-danger"><i class="fa fa-bolt"></i></span>
                                <span class="name">Server #1 overloaded.  </span>
                                <em class="small">34 mins</em>
                            </a>
                        </li>
                        <li class="new">
                            <a href="">
                                <span class="label label-danger"><i class="fa fa-bolt"></i></span>
                                <span class="name">Server #3 overloaded.  </span>
                                <em class="small">1 hrs</em>
                            </a>
                        </li>
                        <li class="new">
                            <a href="">
                                <span class="label label-danger"><i class="fa fa-bolt"></i></span>
                                <span class="name">Server #5 overloaded.  </span>
                                <em class="small">4 hrs</em>
                            </a>
                        </li>
                        <li class="new">
                            <a href="">
                                <span class="label label-danger"><i class="fa fa-bolt"></i></span>
                                <span class="name">Server #31 overloaded.  </span>
                                <em class="small">4 hrs</em>
                            </a>
                        </li>
                        <li class="new"><a href="">See All Notifications</a></li>
                    </ul>
                </div>
            </li>
            <li>
                <a href="#" class="btn btn-default dropdown-toggle" data-toggle="dropdown">
                    <img src="images/photos/user-avatar.png" alt="" />
                    <!--                            John Doe-->
                    [[${session.loginUser.userName}]]  <!--设置账户名为session.loginUser.userName-->
                    <span class="caret"></span>
                </a>
                <ul class="dropdown-menu dropdown-menu-usermenu pull-right">
                    <li><a href="#"><i class="fa fa-user"></i>  Profile</a></li>
                    <li><a href="#"><i class="fa fa-cog"></i>  Settings</a></li>
                    <li><a href="#"><i class="fa fa-sign-out"></i> Log Out</a></li>
                </ul>
            </li>

        </ul>
    </div>
    <!--notification menu end -->

</div>
<!-- header section end-->

<div id="commonscript">  <!--使用id声明抽取部分-->
    !-- Placed js at the end of the document so the pages load faster -->
    <script th:src="@{/js/jquery-1.10.2.min.js}"></script>
    <script th:src="@{/js/jquery-ui-1.9.2.custom.min.js}"></script>
    <script th:src="@{/js/jquery-migrate-1.2.1.min.js}"></script>
    <script th:src="@{/js/bootstrap.min.js}"></script>
    <script th:src="@{/js/modernizr.min.js}"></script>
    <script th:src="@{/js/jquery.nicescroll.js}"></script>
</div>

</body>
</html>
```

然后在main.html以及各个与table相关的HTML文件的左侧和上侧部分进行公共部分的引用

```html
<section>
<!--左侧菜单和主菜单已经移到了common.html   -->
    <div th:insert="common :: #leftmenu"></div><!--将common中声明的部分加进来-->
    <!--主菜单    -->
    <!-- main content start-->
    <div class="main-content" >

        <div th:insert="common :: headermenu"></div>
```



## 数据访问

### SQL

[参考](https://blog.csdn.net/wangmx1993328/article/details/81834974)

#### 数据源的自动配置-**HikariDataSource**

> 数据源（DataSource）是在JDBC2.0中引入的一个概念。在JDBC扩展包中定义了java.sql.DataSource接口，它负责建立与数据库的连接，在应用程序访问数据库是不必编写连接数据库的代码，可直接从数据源获得数据库连接。
>
> 在数据源中事先建立了多个数据库连接，这些数据库连接保存在连接池（Connection Pool）中。Java程序访问数据库时，只需从连接池中取出空闲状态的数据库连接，当访问结束时，再将数据库连接返回给连接池，这样做可以提高数据库的访问效率。
>
> 数据源（DataSource）的作用是获取数据库连接，而连接池则是对已经创建好的数据库连接对象进行管理
> ------------------------------------------------
> 版权声明：本文为CSDN博主「hhrxp373317」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
> 原文链接：https://blog.csdn.net/hhrxp/article/details/20446915

##### 导入JDBC场景

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jdbc</artifactId>
        </dependency>
        
```

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210302161613.png)

默认配置的数据源为HikariDataSource

数据库驱动？

为什么导入JDBC场景，官方不导入驱动？官方不知道我们接下要操作什么数据库。

数据库版本和驱动版本对应

```xml
默认版本：<mysql.version>8.0.22</mysql.version>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
<!--            <version>5.1.49</version>-->
        </dependency>

想要修改版本
1、直接依赖引入具体版本（maven的就近依赖原则）
2、重新声明版本（maven的属性的就近优先原则）
    <properties>
        <java.version>1.8</java.version>
        <mysql.version>5.1.49</mysql.version>
    </properties>
```

##### 分析自动配置

- DataSourceAutoConfiguration ： 数据源的自动配置

- - 修改数据源相关的配置：**spring.datasource**
  - **数据库连接池的配置，是自己容器中没有DataSource才自动配置的**
  - 底层配置好的连接池是：**HikariDataSource**

```java
    @Configuration(proxyBeanMethods = false)
    @Conditional(PooledDataSourceCondition.class)
    @ConditionalOnMissingBean({ DataSource.class, XADataSource.class })
    @Import({ DataSourceConfiguration.Hikari.class, DataSourceConfiguration.Tomcat.class,
            DataSourceConfiguration.Dbcp2.class, DataSourceConfiguration.OracleUcp.class,
            DataSourceConfiguration.Generic.class, DataSourceJmxConfiguration.class })
    protected static class PooledDataSourceConfiguration
```

- DataSourceTransactionManagerAutoConfiguration： 事务管理器的自动配置
- JdbcTemplateAutoConfiguration： **JdbcTemplate的自动配置，可以来对数据库进行crud**

- - 可以修改这个配置项@ConfigurationProperties(prefix = **"spring.jdbc"**) 来修改JdbcTemplate
  - @Bean@Primary   JdbcTemplate；容器中有这个组件

- JndiDataSourceAutoConfiguration： jndi的自动配置
- XADataSourceAutoConfiguration： 分布式事务相关的



##### 修改配置项

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/demo1?serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf-8
    username: root
    password: 123456
    driver-class-name: com.mysql.cj.jdbc.Driver #数据库驱动程序
```



##### 测试

```java
package com.example.demo;

import lombok.extern.slf4j.Slf4j;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.jdbc.core.JdbcTemplate;

@Slf4j
@SpringBootTest
class DemoApplicationTests {

    @Autowired
    JdbcTemplate jdbcTemplate;  //会自动在容器中创建JdbcTemplate对象，数据库操作的所有 CRUD 方法都在 JdbcTemplate 中

    @Test
    void contextLoads() {
        Long along=jdbcTemplate.queryForObject("select count(*) from student",Long.class);//查询表中的记录数
        log.info("记录总数：{}",along);
    }

}
```



#### 使用Druid数据源

##### druid官方github地址

##### 自定义方式

##### 使用官方starter方式



#### 整合MyBatis操作

##### 配置模式

- 添加依赖（pom文件中）

```xml
<!--        jdbc-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>
<!--      mybatis-spring-boot适配器  -->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.4</version>
        </dependency>
<!--       添加mysql驱动包 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
<!--web依赖-->
		<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
```

- 修改配置文件（application.yaml）；指定Mapper配置文件的位置，以及指定全局配置文件的信息 （建议；**配置在mybatis.configuration**）

```yaml
#mybatis设置
mybatis:
#  config-location:
#  sql映射文件的位置
  mapper-locations: classpath:mybatis/Mapper/*.xml
#  全局配置文件的内容可以直接在此处进行设置
  configuration:
    map-underscore-to-camel-case: false


spring:
  application:
    name: demo

#设置数据源
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/mybatis_test?serverTimezone=GMT%2B8
    username: root
    password: 123456

```

- 编写mapper接口(EmployeeMapper.java)。标准@Mapper注解

```java
@Mapper//表示这是一个Mapper
public interface EmployeeMapper {
    Employee getEmployeeById(int id);
}
```

- 编写sql映射文件(EmployeeMapper.xml)并绑定mapper接口

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.example.demo.dao.EmployeeMapper">
    <select id="getEmployeeById" resultType="com.example.demo.bean.Employee">
        select * from tbl_employee where id = #{id}
    </select>
</mapper>
```



- 目录结构

<img src="https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210503094634.png" style="zoom:67%;" />

##### 注解模式

直接在mapper接口(EmployeeMapper.java)中写sql语句，这样就不用sql映射文件(EmployeeMapper.xml)了。

```java
@Mapper
public interface CityMapper {

    @Select("select * from city where id=#{id}")
    public City getById(Long id);

    public void insert(City city);

}
```







##### 混合模式

将上面的两种方式相结合

##### 最佳实战：

- 引入mybatis-starter
- **配置application.yaml中，指定mapper-location位置即可**
- 编写Mapper接口并标注@Mapper注解
- 简单方法直接注解方式
- 复杂方法编写mapper.xml进行绑定映射
- *@MapperScan("com.atguigu.admin.mapper") 简化，其他的接口就可以不用标注@Mapper注解*

#### 整合 MyBatis-Plus 完成CRUD

##### 什么是MyBatis-Plus

##### 整合MyBatis-Plus

##### CRUD功能



### NoSQL

#### Redis自动配置

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
```





自动配置：

- RedisAutoConfiguration 自动配置类。RedisProperties 属性类 --> **spring.redis.xxx是对redis的配置**
- 连接工厂是准备好的。**Lettuce**ConnectionConfiguration、**Jedis**ConnectionConfiguration
- **自动注入了RedisTemplate**<**Object**, **Object**> ： xxxTemplate；
- **自动注入了StringRedisTemplate；k：v都是String**
- **key：value**
- **底层只要我们使用** **StringRedisTemplate、****RedisTemplate就可以操作redis**

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210302220440.png)

redis环境搭建

1、阿里云按量付费redis。经典网络

2、申请redis的公网连接地址

3、修改白名单  允许0.0.0.0/0 访问





#### RedisTemplate与Lettuce

```java
    @Test
    void testRedis(){
        ValueOperations<String, String> operations = redisTemplate.opsForValue();

        operations.set("hello","world");

        String hello = operations.get("hello");
        System.out.println(hello);
    }
```



#### 切换至jedis





## 单元测试

## 指标监控

## 原理解析

# 参考

[链接](https://www.yuque.com/atguigu/springboot)

[链接](https://www.bilibili.com/video/BV19K4y1L7MT)
