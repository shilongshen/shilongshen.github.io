# Spring


# 1.Spring框架概述

## 是什么

- 是一个轻量级的开源的JavaEE框架
- 解决企业应用开发的复杂性
- 有两个核心部分：IOC和AOP
  - IOC:控制反转，把创建对象的过程交给Spring进行管理
  - AOP:面向切面，不修改源代码进行功能增强

## 特点

- 方便解耦，简化开发
- AOP的支持方便程序的测试
- 方便和其他框架进行整合
- 降低Java EE API的开发难度
- 方便事务的操作

## 入门案例

1.下载地址：
https://repo.spring.io/release/org/springframework/spring/
#spring-5.2.6.RELEASE-dist.zip     28-Apr-2020 08:22  82.25 MB

2.使用IDEA创建project

3.导入Spring的jar包
![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210215092854.png)

commons-logging-1.1.1.jar
spring-beans-5.2.6.RELEASE.jar
spring-context-5.2.6.RELEASE.jar
spring-core-5.2.6.RELEASE.jar
spring-expression-5.2.6.RELEASE.jar

4.创建普通类，在这个类创建普通方法

```java
package com.bean1;
public class User {
    public void add(){
        System.out.println("hello world!");
    }
}
```



5.创建Spring配置文件，在配置文件配置创建的对象
(1)Spring配置文件使用XML格式

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
        <!--配置User对象创建-->
    <bean id="user2" class="com.bean1.User"></bean>
</beans>

```

6.进行测试代码编写

```java
package com.bean1.test_demo;

import com.bean1.User;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class TestSpring {
    @Test
    public void testadd(){
        //1. 加载Spring配置文件
        ApplicationContext context=new ClassPathXmlApplicationContext("bean1.xml");
        //2. 获取配置创建的对象
        User user=context.getBean("user2", User.class);
        System.out.println(user);
        user.add();
    }
}

/*
com.bean1.User@9a7504c
hello world!
*/
```



# 2.IOC容器

## IOC底层原理

### 是什么

- 控制反转，把对象创建和对象之间的调用过程，交给Spring进行管理
- 使用IOC目的：降低耦合度

> [参考](https://blog.csdn.net/ivan820819/article/details/79744797)
>
> ![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210216100323.png)
>
> ![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210216100446.png)
>
> ![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210216100537.png)
>
> [参考](https://www.cnblogs.com/king8/p/11629695.html)
>
> ![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210216102618.png)

### 底层原理

- xml解析、工厂模式、反射（**通过得到字节码文件，然后可以操作类中所有内容**）

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210216094639.png)

1. xml配置文件，配置创建的对象

   ```java
   <bean id="dao" class="com.bean1.UserDao"></bean>
   ```

2. 有service类和dao类，创建工厂类

   ```java
   class UserFactory{
       public static UserDao getDao(){
           //1. xml解析
           String classValue = class属性值; 
           //2. 通过反射创建对象
           Class clazz = Class.forName(classValue);
           return (UserDao)clazz.newInstance();
       }
   }
   ```

   



## IOC接口

1. IOC思想基于IOC容器完成，IOC容器底层就是对象工厂

2. Spring提供IOC容器实现的两种方式

   1. **BeanFactory**：IOC容器中基本实现方式，是Spring内部使用接口，不提供开发人员使用；<u>加载配置文件的时候不会创建对象，而是在使用的时候创建对象</u>。

      ```java
      //1. 加载Spring配置文件
      BeanFactory context=new ClassPathXmlApplicationContext("bean1.xml");
      //2. 获取配置创建的对象
      User user=context.getBean("user2", User.class);
      ```

      

   2. **ApplicationContext**：BeanFactory的一个子接口，提供更多的功能，一般由开发人员使用；<u>在加载配置文件的时候就会将配置文件中的对象进行创建</u>。

3. **ApplicationContext**接口中的主要实现类：

   ![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210216104549.png)

   ```java
   FileSystemXmlApplicationContext #通过文件路径导入xml文件
   ClassPathXmlApplicationContext #直接在类中导入xml文件
   ```



## IOC操作Bean管理

1. 什么是Bean管理（Bean管理指的是两个操作）
   1. Spring创建对象
   2. Spring注入属性
2. Bean管理操作有两种方式
   1. 基于xml配置文件方式
   2. 基于注解方式



### 基于xml方式

基于xml方式创建对象

```xml
<!--配置User对象创建-->
<bean id="user2" class="com.bean1.User"></bean>
```

1.在Spring配置文件中，使用bean标签，标签里添加对应属性，就可以实现对象创建

2.bean标签中常用属性

- id属性：给对象确定一个标识
- class属性：类全路径（包类路径）

3.**创建对象的时候，默认执行无参构造方法**，（注意）





#### DI：依赖注入，就是注入属性

```java
/*
一般的方式
*/
public class Book {
    private String name;
    //1. 通过有参构造进行属性注入
    public Book(String name) {
        this.name = name;
    }
   //2.通过set方法进行属性注入
    public void setName(String name) {
        this.name = name;
    }
}
```

#### 使用set方法进行属性注入

1. 创建类，定义属性和对应的set方法

```java
public class Book {
    private String name;
   //通过set方法进行属性注入
    public void setName(String name) {
        this.name = name;
    }
    public void test(){
        System.out.println("name="+name);
    }
}
```

 2.在Spring配置文件中配置对象的创建以及属性的注入

```xml
<!--使用set进行对象创建和属性注入 -->
    <bean id="book" class="com.bean1.Book">
        <!--使用property完成属性注入
        name:field字段
        value：为filed字段进行赋值
        -->
        <property name="name" value="xiaoming"></property>
    </bean>
```

```java
#测试代码
package com.bean1.test_demo;

import com.bean1.Book;
import com.bean1.User;
import org.junit.Test;
import org.springframework.beans.factory.BeanFactory;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class TestSpring {
    @Test
    public void testbook1(){
        //1. 加载Spring配置文件
        ApplicationContext context=new ClassPathXmlApplicationContext("bean1.xml");
        //2. 获取配置创建的对象

        ////Book.class：Book类的class文件，通过class文件获取类的内容(反射)，book是Book的一个实例
        Book book=context.getBean("book", Book.class);
        System.out.println(book);
        book.test();
    }
}

/*
com.bean1.Book@69b0fd6f
name=xiaoming
*/
```



#### 使用有参构造方法进行属性注入

1.创建类，定义属性，创建属性对应的有参构造方法

```java
package com.bean1;

public class Order {
    private String name;

    public Order(String name) {
        this.name = name;
    }
    public void test(){
        System.out.println("name="+name);
    }
}
```

2.在Spring配置文件中进行配置

```xml
<!--使用有参构造方法进行对象创建和属性注入 -->
    <bean id="order" class="com.bean1.Order">
        <!--使用constructor-arg完成属性注入
         name:field字段
         value：为filed字段进行赋值
         -->
        <constructor-arg name="name" value="xiaoming"/>
    </bean>
```

```java
#测试代码
    package com.bean1.test_demo;

import com.bean1.Book;
import com.bean1.Order;
import com.bean1.User;
import org.junit.Test;
import org.springframework.beans.factory.BeanFactory;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class TestSpring {
    @Test
    public void testorder(){
        ApplicationContext context=new ClassPathXmlApplicationContext("bean1.xml");
        Order order=context.getBean("order",Order.class);
        System.out.println(order);
        order.test();
    }
}
/*
com.bean1.Order@18bf3d14
name=xiaoming
*/
```

#### 使用xml设置其他类型的值

```xml
#注入空值
<property name="name">
            <null/>
        </property>

#属性值包含特殊符号
<property name="name">
            <value> <![CDATA[<<nanjing>>]]> </value>
        </property>
```

#### 属性注入：外部Bean

**将一个外部对象作为一个属性注入**

1. 创建两个类：service类和dao类

2. 在service调用dao中方法

   ```java
   package dao;
   
   public interface UserDao {
       public void update();
   }
   
   package dao;
   
   public class UserDaoImpl implements UserDao{
       @Override
       public void update() {
           System.out.println("dao updating...");
       }
   }
   
   ```

   ```java
   package service;
   
   import dao.UserDao;
   import dao.UserDaoImpl;
   
   public class UserService {
       //创建UserDao类型属性，生成set方法
       private UserDao userDao;
   
       public void setUserDao(UserDao userDao) {
           this.userDao = userDao;
       }
   
       public void add(){
           System.out.println("service add...");
           //目标：UserService中调用UserDao中的方法
           //一般做法，直接在UserService创建UserDao对象，调用方法
   //        UserDao userDao=new UserDaoImpl();
   //        userDao.update();
   
           //现在想把这一过程通过Spring配置方法实现,即将UserDao对象作为一个属性注入到UserService中
           userDao.update();
       }
   }
   
   ```

   

3. 在Spring配置文件中进行配置

   ```xml
   <!--创建UserService和UserDao对象-->
       <bean name="userservice" class="service.UserService">
           <!--使用set方法进行属性注入，将属性userDao设置为UserDao对象：userdao；ref表示将外部Bean注入-->
           <property name="userDao" ref="userdao"></property>
       </bean>
       <bean name="userdao" class="dao.UserDaoImpl"></bean>
   ```

   ```java
   #测试代码
   package com.bean1.test_demo;
   
   import org.junit.Test;
   import org.springframework.context.ApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   import service.UserService;
   
   public class TestSpring2 {
       @Test
       public void testadd(){
           ApplicationContext context=new ClassPathXmlApplicationContext("bean2.xml");
           UserService userService=context.getBean("userservice",UserService.class);
   
           userService.add();
       }
   }
   
   /*
   service add...
   dao updating...
   */
   ```

   

#### 属性注入：内部Bean和级联赋值

1. 一对多：部门和员工

   一个部门有多个员工，部门是一，员工是多

2. 在实体类之间表示一对多关系，员工属于某一个部门，使用对象类型属性进行表示

   ```java
   package com.bean2;
   //部门类
   public class Dept {
       private String Dname;
   
       public void setDname(String dname) {
           Dname = dname;
       }
   
       public String getDname() {
           return Dname;
       }
   }
   
   ```

   ```java
   package com.bean2;
   //员工类
   public class Emp {
       private String ename;
       private String gender;
   
       //员工属于某个部门，使用对象形式表示
       private Dept dept;
   
       public void setDept(Dept dept) {
           this.dept = dept;
       }
   
       public void setEname(String ename) {
           this.ename = ename;
       }
   
       public void setGender(String gender) {
           this.gender = gender;
       }
   
       public void add(){
           System.out.println("dname="+dept.getDname()+" ename="+ename+" gender="+gender);
       }
   }
   
   ```

3. 在配置文件中进行配置

   ```xml
   <!--使用内部Bean方式创建对象和属性注入-->
       <bean name="emp" class="com.bean2.Emp">
           <!--先设置两个普通的属性-->
           <property name="ename" value="xiaoming"></property>
           <property name="gender" value="men"></property>
           <!--再设置对象类型属性  使用内部Bean的方式-->
           <!--在一个Bean中嵌套另一个Bean-->
           <property name="dept">
               <bean id="dept" class="com.bean2.Dept">
                   <property name="dname" value="A"></property>
               </bean>
           </property>
       </bean>
   ```

   ```xml
   #或者使用级联赋值的方式进行配置
   <!--创建对象和属性注入-->
       <bean name="emp" class="com.bean2.Emp">
           <!--先设置两个普通的属性-->
           <property name="ename" value="xiaoming"></property>
           <property name="gender" value="men"></property>
           <property name="dept" ref="dept"></property>
       </bean>
       <bean name="dept" class="com.bean2.Dept">
           <property name="dname" value="A"></property>
       </bean>
   ```

   ```java
   #测试类
   package com.bean2;
   
   import org.junit.Test;
   import org.springframework.context.ApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   
   public class testDemo {
       @Test
       public void addtest(){
           ApplicationContext context=new ClassPathXmlApplicationContext("bean3.xml");
           Emp emp=context.getBean("emp",Emp.class);
           emp.add();
       }
   }
   
   /*
   dname=A ename=xiaoming gender=men
   */
   ```

#### xml注入集合属性

1. 注入数组类型属性

2. 注入List集合类型属性

3. 注入Map集合类型属性
4. 注入Set集合类型属性

操作步骤：

- 创建类，定义数组,List,Map,Set属性及set方法

  ```java
  package collectiondemo;
  
  import java.util.Arrays;
  import java.util.List;
  import java.util.Map;
  import java.util.Set;
  
  public class Stu {
      //数组类型属性
      private String[] courses;
      //List类型属性
      private List<String> lists;
      //    Map类型属性
      private Map<String, String> maps;
  
      //Set类型属性
      private Set<String> sets;
  
  
      public void setCourses(String[] courses) {
          this.courses = courses;
      }
  
      public void setLists(List<String> lists) {
          this.lists = lists;
      }
  
      public void setMaps(Map<String, String> maps) {
          this.maps = maps;
      }
  
      public void setSets(Set<String> sets) {
          this.sets = sets;
      }
  
      public void test(){
          System.out.println("数组="+ Arrays.toString(courses)+" list="+lists+" map="+maps+" set="+sets);
      }
  
  }
  
  ```

  

- 再Spring中配置文件

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
  <!--    集合类型注入-->
      <bean id="stu" class="collectiondemo.Stu">
  <!--        数组类型注入-->
          <property name="courses">
              <array>
                  <value>自动控制原理</value>
                  <value>现代控制理论</value>
              </array>
          </property>
  <!--        List；类型属性注入-->
          <property name="lists">
              <list>
                  <value>这是list1</value>
                  <value>这是list2</value>
              </list>
          </property>
  <!--        map类型属性注入-->
          <property name="maps">
              <map>
                  <entry key="key1" value="value1"></entry>
                  <entry key="key2" value="value2"></entry>
              </map>
          </property>
  <!--        set类型属性注入-->
          <property name="sets">
              <set>
                  <value>set1</value>
                  <value>set2</value>
              </set>
          </property>
      </bean>
  </beans>
  ```

  ```java
  package collectiondemo;
  
  import org.junit.Test;
  import org.springframework.context.ApplicationContext;
  import org.springframework.context.support.ClassPathXmlApplicationContext;
  
  public class testdemo {
      @Test
      public void test(){
          ApplicationContext context=new ClassPathXmlApplicationContext("collectiondemo.xml");
          Stu stu=context.getBean("stu" ,Stu.class);
          stu.test();
      }
  }
  /*
  数组=[自动控制原理, 现代控制理论] list=[这是list1, 这是list2] map={key1=value1, key2=value2} set=[set1, set2]
  */
  ```

  

#### xml注入集合属性2

1.在集合中设置对象类型属性

```java
package collectiondemo;

public class course {
    private String cname;

    public void setCname(String cname) {
        this.cname = cname;
    }

    @Override
    public String toString() {
        return "course{" +
                "cname='" + cname + '\'' +
                '}';
    }
}

```

```java
package collectiondemo;

import java.util.Arrays;
import java.util.List;
import java.util.Map;
import java.util.Set;

public class Stu {
    

    //list集合中传入对象
    private List<course> courseList;

    public void setCourseList(List<course> courseList) {
        this.courseList = courseList;
    }

    public void test(){
        System.out.println(courselist="+courseList);
    }

}

```

```xml
<!--        Spring配置文件-->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
<!--    集合类型注入-->
    <bean id="stu" class="collectiondemo.Stu">
<!--        list中传入对象-->
        <property name="courseList">
            <list>
                <!--使用ref进行对象传入-->
                <ref bean="course1"></ref>
                <ref bean="course2"></ref>
            </list>
        </property>
    </bean>

    <!--        创建course对象并进行属性注入-->
    <bean id="course1" class="collectiondemo.course">
        <property name="cname" value="course1"></property>
    </bean>
    <bean id="course2" class="collectiondemo.course">
        <property name="cname" value="course2"></property>
    </bean>
</beans>
```

```java
#测试代码
    package collectiondemo;

import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class testdemo {
    @Test
    public void test(){
        ApplicationContext context=new ClassPathXmlApplicationContext("collectiondemo.xml");
        Stu stu=context.getBean("stu" ,Stu.class);
        stu.test();
    }
}
/*
courselist=[course{cname='course1'}, course{cname='course2'}]

*/
```

#### FactoryBean

1. Spring有两种类型Bean,一种普通bean，另外一种工厂bean(FactoryBean)

2. 普通Bean:在配置文件中定义Bean类型就是返回类型

3. 工厂Bean：在配置文件定义Bean类型可以和返回类型不同

   1. 创建类，让这个类作为工厂Bean，实现接口FactoryBean
   2. 实现接口方法，在实现的方法中定义返回的Bean类型

   ```java
   package factorybean;
   
   public class Course {
       private String name;
   
       public void setName(String name) {
           this.name = name;
       }
   
       @Override
       public String toString() {
           return "Course{" +
                   "name='" + name + '\'' +
                   '}';
       }
   }
   ```

   ```java
   package factorybean;
   
   import org.springframework.beans.factory.FactoryBean;
   
   public class Mybean implements FactoryBean<Course> {//实现FactoryBean接口
   
       // 覆写getObject，   定义返回，此时返回的是course
       @Override
       public Course getObject() throws Exception {
           Course course=new Course();
           course.setName("abc");
           return course;
       }
   
   
       @Override
       public Class<?> getObjectType() {
           return null;
       }
   
       @Override
       public boolean isSingleton() {
           return false;
       }
   }
   
   ```

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
       <bean id="mybean" class="factorybean.Mybean">
   
       </bean>
   </beans>
   ```

   ```java
   #测试代码
       package factorybean;
   
   import org.junit.Test;
   import org.springframework.context.ApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   
   public class testdemo {
       @Test
       public void test(){
           ApplicationContext context=new ClassPathXmlApplicationContext("factorybean.xml");
           Course course= context.getBean("mybean" ,Course.class);
           System.out.println(course);
       }
   }
   /*
   Course{name='abc'}
   */
   
   
   ```

   
#### Bean的作用域
   1. 在Spring中，设置创建的bean实例是单实例还是多实例

   2. 在Spring中，默认情况下，bean是单实例对象

      ```java
      public class testdemo {
          @Test
          public void test(){
              ApplicationContext context=new ClassPathXmlApplicationContext("collectiondemo.xml");
              Stu stu=context.getBean("stu" ,Stu.class);
              Stu stu2=context.getBean("stu" ,Stu.class);
              System.out.println(stu);
              System.out.println(stu2);
      //        stu.test();
          }
      }
      
      /*
      collectiondemo.Stu@6eceb130
      collectiondemo.Stu@6eceb130
      */
      ```

   3. 如何设置是单实例还是多实例

      可以通过bean标签的`scope`进行设置；

      ```xml
      <bean id="stu" class="collectiondemo.Stu" scope="prototype"></bean>
      
      scope="singleton"时表示单实例，默认值；在一加载配置文件的时候就创建对象(使用ApplicationContext的时候就创建)
      scope="prototype"时表示创建多实例对象；在调用的时候才创建（使用getBean的时候才创建）
      
      此时输出的结果为
      collectiondemo.Stu@6eceb130
      collectiondemo.Stu@10a035a0
          
      ```


#### Bean的生命周期

生命周期：从对象创建到对象销毁的过程

- 通过构造器创建bean实例（无参构造）
- 为bean的属性设置值和对其他bean引用(调用set方法)
- 调用bean的初始化方法(需要进行配置的初始化方法)
- bean可以使用了(对象获取到了)
- 当容器关闭时，调用bean的销毁方法(需要进行配置销毁的方法)

#### xml自动装配

根据指定装配规则（属性名称或属性类型），Spring自动将匹配的属性进行注入

```java
package demo1;

public class Dept {
    @Override
    public String toString() {
        return "Dept{}";
    }
}

```

```java
package demo1;

public class Emp {
    private Dept dept;//类属性名

    @Override
    public String toString() {
        return "Emp{" +
                "dept=" + dept +
                '}';
    }

    public void test(){
        System.out.println(dept);
    }

    public void setDept(Dept dept) {
        this.dept = dept;
    }
}

```



```xml
<!--    实现自动装配
        autowire标签
        byName:根据属性名称进行注入，注入值bean的id值和类属性名称一样

-->
    <bean id="emp" class="demo1.Emp" autowire="byName">
    </bean>

    <bean id="dept" class="demo1.Dept"></bean>

```

```xml
#或者
<!--    实现自动装配
        autowire标签
        byType:根据属性类型进行注入，注入值bean的类型值和类属性类型一样

-->
    <bean id="emp" class="demo1.Emp" autowire="byType">
    </bean>

    <bean id="dept" class="demo1.Dept"></bean>
```

#### 外部属性文件

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210217212511.png)

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210217212656.png)

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210217212733.png)

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210217212839.png)



### 基于注解方式

什么是注解：注解是代码特殊标记，格式：@注解名称（属性名称=属性值，属性名称=属性值）

# 3.AOP

## 什么是AOP

- 面向切面编程，利用AOP可以对业务逻辑的各个部分进行隔离，从而使业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210218092242.png)

- 不通过修改源代码，在主干功能里面添加新的功能

> [参考](https://www.zhihu.com/question/24863332/answer/48376158)
>
> 面向切面编程（AOP是Aspect Oriented Program的首字母缩写） ，我们知道，面向对象的特点是继承、多态和封装。而封装就要求将功能分散到不同的对象中去，这在软件设计中往往称为职责分配。实际上也就是说，让不同的类设计不同的方法。这样代码就分散到一个个的类中去了。这样做的好处是降低了代码的复杂程度，使类可重用。
>       但是人们也发现，在分散代码的同时，也增加了代码的重复性。什么意思呢？比如说，我们在两个类中，可能都需要在每个方法中做日志。按面向对象的设计方法，我们就必须在两个类的方法中都加入日志的内容。也许他们是完全相同的，但就是因为面向对象的设计让类与类之间无法联系，而不能将这些重复的代码统一起来。
>     也许有人会说，那好办啊，我们可以将这段代码写在一个独立的类独立的方法里，然后再在这两个类中调用。但是，这样一来，这两个类跟我们上面提到的独立的类就有耦合了，它的改变会影响这两个类。那么，有没有什么办法，能让我们在需要的时候，随意地加入代码呢？**这种在运行时，动态地将代码切入到类的指定方法、指定位置上的编程思想就是面向切面的编程。** 
>       一般而言，我们管切入到指定类指定方法的代码片段称为切面，而切入到哪些类、哪些方法则叫切入点。有了AOP，我们就可以把几个类共有的代码，抽取到一个切片中，等到需要时再切入对象中去，从而改变其原有的行为。
> 这样看来，AOP其实只是OOP的补充而已。OOP从横向上区分出一个个的类来，而AOP则从纵向上向对象中加入特定的代码。有了AOP，OOP变得立体了。如果加上时间维度，AOP使OOP由原来的二维变为三维了，由平面变成立体了。从技术上来说，AOP基本上是通过代理机制实现的。 
>      AOP在编程历史上可以说是里程碑式的，对OOP编程是一种十分有益的补充。

## AOP底层原理

AOP底层使用动态代理，有两种情况的动态代理

- 有接口情况，使用JDK动态代理

  创建接口实现类代理对象，增强类的方法

  ![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210218094717.png)

- 没有接口情况，使用CGLIB动态代理

  创建子类的代理对象，增强类的方法

  ![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210218095354.png)

## JDK动态代理

使用[Proxy](https://www.matools.com/api/java8)类里面的方法`newProxyInstance`创建代理对象

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210218100646.png)

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210218100738.png)

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210218100947.png)

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210218101355.png)

newProxyInstance的三个参数（newProxyInstance是一个静态方法，属于类，可以直接调用）

- loader:类加载器
- interface：增强方法所在的类，这个类实现的接口；支持多个接口
- h：实现这个接口InvocationHandler，创建代理对象，写增强方法

编写JDK动态代理代码

1. 创建接口，定义方法

```java
package AOP.demo1;

public interface UserDao {
    public int add(int a,int b);
    public String update(String id);
}

```



2. 创建接口实现类，实现方法

```java
package AOP.demo1;

public class UserDaoImpl implements UserDao{
    @Override
    public int add(int a, int b) {
        System.out.println("add方法执行...");
        return a+b;
    }

    @Override
    public String update(String id) {
        return id;
    }
}

```



3. 使用Proxy

```java
package AOP.demo1;

import java.lang.reflect.Array;
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;
import java.util.Arrays;

public class JDKProxy {
    public static void main(String[] args) {
//        创建接口实现类代理对象
        Class[] interfaces = {UserDao.class};
        
//			使用匿名内部类的方式
//        Proxy.newProxyInstance(JDKProxy.class.getClassLoader(), interfaces, new InvocationHandler() {
//            @Override
//            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
//                return null;
//            }
//        });
        
        UserDao userDao = new UserDaoImpl();
        UserDao res= (UserDao) Proxy.newProxyInstance(JDKProxy.class.getClassLoader(), interfaces, new UserDaoProxy(userDao));
        int result=res.add(1,2);
        System.out.println("result="+result);
    }
}

//创建代理对象代码
class UserDaoProxy implements InvocationHandler {

    //    把创建的是谁的代理对象，把谁传递过来
//    有参数构造传递，
    private Object object;

    public UserDaoProxy(Object object) {
        this.object = object;
    }

    //在invoke中实现逻辑的增强，增强的是UserDao中的方法->add方法和update方法
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
//        方法之前
        System.out.println("方法执行之前执行..." + method.getName() + ":传递的参数" + Arrays.toString(args));
//        被增强的方法执行
        Object res =  method.invoke(object, args);//invoke返回的是Object

//        方法之后
        System.out.println("方法之后..." + object);

        return res;
    }
}
/*
方法执行之前执行...add:传递的参数[1, 2]
add方法执行...
方法之后...AOP.demo1.UserDaoImpl@6f94fa3e
result=3
*/
```

## AOP相关术语

1. 连接点

   类里面哪些方法可以被增强，这些方法就称为连接点

2. 切入点

   实际被增强的方法，称为切入点

3. 通知（增强)

   实际增强的逻辑部分称为通知

   通知有多种类型

   - 前置通知：方法前增强
   - 后置通知：方法后增强
   - 环绕通知：方法之前和之后都增强
   - 异常通知：当方法出现了异常会执行
   - 最终通知：永远会执行，类似于try...fanally

4. 切面

   是一个动作

   把通知应用到切入点的过程称为切面

## AOP操作

Spring中一般基于**AspectJ**实现AOP操作

什么是**AspectJ**

- AspectJ不是Spring组成部分，独立AOP框架，一般把AspectJ和Spring一起使用，进行AOP操作。

基于AspectJ实现AOP操作的两种方式：

- 基于XML配置文件
- 基于注解方式（常用）

准备工作

1. 在项目工程中引入AOP依赖

   ![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210218145828.png)

2. 切入点表达式

   1. 作用：知道对哪个类里面的哪个方法进行增强

   2. 语法结构

      ```java
      execution([权限修饰符][返回类型][类全路径][方法名称]([参数列表]))
      
      #例1：对AOP.demo1类中的add方法进行增强
          execution(* AOP.demo1.add(..))  # *表示任意权限修饰符，返回类型可以省略
      #例2：对AOP.demo1类中的所有方法进行增强
          execution(* AOP.demo1.*(..))  # *表示所有方法
      #例3：对AOP包中的所有类，类中的所有方法进行增强
          execution(* AOP.*.*(..))
          
      ```

## AspectJ注解方式

1. 创建类，类中定义方法

   ```java
   package AOPAnno;
   //被增强类
   public class User {
       public void add(){
           System.out.println("add...");
       }
   }
   
   ```

2. 创建增强类，编写增强逻辑

   1. 在增强类中创建方法，让不同的方法表示不同的通知类型

      ```java
      package AOPAnno;
      
      //增强的类
      public class UserProxy {
      //    前置通知
          public void before(){
              System.out.println("before...");
          }
      }
      
      ```

3. 进行通知配置

   1. 在Spring配置文件中，开启注解扫描

      ```xml
      <!--    开启注解扫描-->
      <context:component-scan base-package="AOPAnno"></context:component-scan>
      ```

      

   2. 使用注解创建User和UserProxy对象

      ![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210218152932.png)

      ![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210218152907.png)

   3. 在增强类上面添加注解@AspectJ

      ![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210218153042.png)

   4. 在Spring配置文件中开启生成代理对象

      ```xml
      <!--    开启Aspect-->
          <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
      ```

4. 配置不同类型的通知

   在增强类中，在作为通知方法上面添加通知类型注解，使用切入点表达式配置

   ```java
   package AOPAnno;
   
   import org.aspectj.lang.ProceedingJoinPoint;
   import org.aspectj.lang.annotation.*;
   import org.springframework.stereotype.Component;
   
   //增强的类
   @Component
   @Aspect
   public class UserProxy {
   //    前置通知
   //    使用@Before明确哪个类中的哪个方法要做前置增强
       @Before(value = "execution(* AOPAnno.User.add(..))")
       public void before(){
           System.out.println("before...");
       }
   
   
   //    返回通知
       @AfterReturning(value = "execution(* AOPAnno.User.add(..))")
       public void afterreturn(){
           System.out.println("afterreturning...");
       }
   
   //    最终通知
       @After(value = "execution(* AOPAnno.User.add(..))")
       public void after(){
           System.out.println("after...");
       }
   
   //    异常通知
       @AfterThrowing(value = "execution(* AOPAnno.User.add(..))")
       public void afterthrowing(){
           System.out.println("afterafterthrowing...");
       }
   
   //    环绕通知
       @Around(value = "execution(* AOPAnno.User.add(..))")
       public void around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable{
           System.out.println("环绕前...");
           proceedingJoinPoint.proceed();
           System.out.println("环绕后...");
   
       }
   
   
   }
   
   ```

   ```java
   #测试类
       package AOPAnno;
   
   import org.junit.Test;
   import org.springframework.context.ApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   
   public class test {
       @Test
       public void testadd(){
           ApplicationContext context=new ClassPathXmlApplicationContext("AopAnno.xml");
           User user=context.getBean("user",User.class);
           user.add();
       }
   }
   /*
   环绕前...
   before...
   add...
   环绕后...
   after...
   afterreturning...
   
   */
   ```

   

   

5. 相同切入点 

   

   ```java
   #
       //    相同切入点抽取,可以将value进行抽取出来，简化写法
       @Pointcut(value = "execution(* com.Dancebytes.spring5.aopanno.User.add(..))")
       public void pointdemo() {
   
       }
   
       //前置通知
       //@Before注解表示作为前置通知
       @Before(value = "pointdemo()")
       public void before() {
           System.out.println("brfore....");
       }
   
   //    返回通知
       @AfterReturning(value = "pointdemo()")
       public void afterReturning() {
           System.out.println("afterReturning....");
       }
   
   //    最终通知
       @After(value = "pointdemo()")
       public void after() {
           System.out.println("after....");
       }
   ```

6. 有多个增强类对同一个方法进行增强，可以设置增强类的优先级

   在增强类的上面添加注解@Order(数字)，数字越小，优先级越高



## AspectJ XML方式

1. 创建被增强类和增强类

   ```java
   package AopXml;
   
   public class User {
       public void add(){
           System.out.println("add...");
       }
   }
   
   ```

   ```java
   package AopXml;
   
   public class UserProxy {
       public void before(){
           System.out.println("before...");
       }
   }
   
   ```

   

2. xml配置文件中进行配置

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:aop="http://www.springframework.org/schema/aop"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
           http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">
       <!--创建对象-->
       <bean id="user" class="AopXml.User"></bean>
       <bean id="userproxy" class="AopXml.UserProxy"></bean>
       <!--配置AOP增强-->
       <aop:config>
           <!--切入点-->
           <aop:pointcut id="p" expression="execution(* AopXml.User.add(..))"/>
           <!--切面 -->
           <aop:aspect ref="userproxy">
   		<!--增强作用在具体的方法上,将userproxy中的before方法作用在add方法上-->
               <aop:before method="before" pointcut-ref="p"></aop:before>
           </aop:aspect>
   
       </aop:config>
   </beans>
   ```

   ```java
   #测试
       public class testadd {
       @Test
       public void test(){
           ApplicationContext context=new ClassPathXmlApplicationContext("AopXml.xml");
           User user=context.getBean("user",User.class);
           user.add();
       }
   }
   
   /*
   before...
   add...
   */
   ```



# JdbcTemplate

......
