# MyBatis

[参考](https://blog.csdn.net/Kafkaaa/article/details/114910390)

[参考](https://www.processon.com/view/60346a4b6376896cd6ec03ac?fromnew=1#map)

[中文文档](https://mybatis.org/mybatis-3/zh/index.html)
# 基本概念

业务层—处理业务需求
持久层—和数据库交互，

- 持久层技术解决方案
  JDBC技术（最底层）：Connection、PreparedStatement、ResultSet
  Spring的JdbcTemplate：Spring中对jdbc的简单封装
  Apache的DBUtils：和Spring的JdbcTemplate类似
- 以上都不是框架，jdbc是规范，剩下两个是工具包
  - 封装不够细致，需要关注加载驱动、创建连接、创建Statement等繁杂过程。
  - 功能简单，sql语句编写在java代码里，硬编码高耦合（sql变化时需要修改源码）

框架：整体解决方案 

Hibernate：全自动全映射ORM框架，旨在消除sql。但是存在开发人员无法自主优化sql语句的缺点。

mybatis是优秀的基于java持久层的框架（半自动），它内部封装了jdbc，sql与java编码分离（sql单独提取到配置文件中，由开发者控制），开发者只需要关注sql语句本身（剩下的预编译、设置参数、执行sql、封装结果由框架完成）。它使用了ORM的思想实现了结果集的封装。

<img src="https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210424191542.png" style="zoom:80%;" />

# MyBatis实现总结

1. 全局配置文件MyBatis.xml
   包含数据源、事务管理器、运行环境的信息
2. sql映射文件EmployeeMapper.xml --><u>关键文件，mybatis从该文件抽取出sql</u>
   配置每个sql，以及sql的封装规则（查出相应记录后如何封装）
3. 将sql映射文件注册在全局配置文件中
4. 写测试代码（EmployeeTest.java）
    关键：
    - 根据配置文件获得SqlSessionFactory
- 使用SqlSessionFactory获取到SqlSession对象，使用该SqlSession对象中的方法进行增删查改
    - 一个SqlSession就是和数据库的一次会话，使用完要关闭
    - 使用sql的唯一标识（如selectOne方法的第一个参数）来告诉mybatis执行哪个sql

SqlSession对象中的方法:

```java
//
// Source code recreated from a .class file by IntelliJ IDEA
// (powered by FernFlower decompiler)
//

package org.apache.ibatis.session;

import java.io.Closeable;
import java.sql.Connection;
import java.util.List;
import java.util.Map;
import org.apache.ibatis.cursor.Cursor;
import org.apache.ibatis.executor.BatchResult;

public interface SqlSession extends Closeable {
    <T> T selectOne(String var1);

    <T> T selectOne(String var1, Object var2);

    <E> List<E> selectList(String var1);

    <E> List<E> selectList(String var1, Object var2);

    <E> List<E> selectList(String var1, Object var2, RowBounds var3);

    <K, V> Map<K, V> selectMap(String var1, String var2);

    <K, V> Map<K, V> selectMap(String var1, Object var2, String var3);

    <K, V> Map<K, V> selectMap(String var1, Object var2, String var3, RowBounds var4);

    <T> Cursor<T> selectCursor(String var1);

    <T> Cursor<T> selectCursor(String var1, Object var2);

    <T> Cursor<T> selectCursor(String var1, Object var2, RowBounds var3);

    void select(String var1, Object var2, ResultHandler var3);

    void select(String var1, ResultHandler var2);

    void select(String var1, Object var2, RowBounds var3, ResultHandler var4);

    int insert(String var1);

    int insert(String var1, Object var2);

    int update(String var1);

    int update(String var1, Object var2);

    int delete(String var1);

    int delete(String var1, Object var2);

    void commit();

    void commit(boolean var1);

    void rollback();

    void rollback(boolean var1);

    List<BatchResult> flushStatements();

    void close();

    void clearCache();

    Configuration getConfiguration();

    <T> T getMapper(Class<T> var1);

    Connection getConnection();
}

```



## 使用Maven构建MyBatis环境1

- 创建maven环境，在pom.xml添加依赖

```xml
	   <dependency>   <!--添加单元测试-->
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
            <scope>test</scope>
        </dependency>

        <dependency>   <!--添加mybatis-->
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.1</version>
        </dependency>

        <dependency>   <!--添加mysql驱动包-->
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.11</version>   <!--注意mysql的版本号要与本地的版本号一致，否则会报错-->
        </dependency>
```

- 创建Employee类   -->一个数据库表对应一个类，表中的每一个字段对应类中的一个属性

```java
package org.example.bean;

public class Employee {
    private Integer id;
    private String last_name;
    private String email;
    private String gender;

    @Override
    public String toString() {
        return "Employee{" +
                "id=" + id +
                ", last_name='" + last_name + '\'' +
                ", email='" + email + '\'' +
                ", gender='" + gender + '\'' +
                '}';
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getLast_name() {
        return last_name;
    }

    public void setLast_name(String last_name) {
        this.last_name = last_name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getGender() {
        return gender;
    }

    public void setGender(String gender) {
        this.gender = gender;
    }
}

```

- 在resources中创建MyBatis.xml（全局配置文件）

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                
                <!--填写4个参数-->
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis_test?serverTimezone=GMT%2B8"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
                
            </dataSource>
        </environment>
    </environments>
    
    <mappers>   <!--将sql映射文件进行注册-->
        <mapper resource="EmployeeMapper.xml"/>
    </mappers>
</configuration>

```

- 在resources中创建EmployeeMapper.xml（sql映射文件，以后所有的sql语句都在这在书写）

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--
namespace:名称空间
id:唯一标识
resultType:返回值类型
#{id}：从传递过来的参数中取出id值
-->
<mapper namespace="org.example.bean.Employee">
    <select id="selectEmp" resultType="org.example.bean.Employee">
        select * from tbl_employee where id = #{id}
    </select>
</mapper>
```

- 测试，在test文件夹下建立EmployeeTest

```java
package org.example;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.example.bean.Employee;
import org.testng.annotations.Test;

import java.io.IOException;
import java.io.InputStream;

public class EmployeeTest {
    @Test
    public void test() throws IOException {
        /*
         * 1.根据xml配置文件（全局配置文件）创建一个SqlSessionFactory对象
         *
         * */
        String resource = "MyBatis.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

        /*
         *2. 获取sqlSession实例，能直接执行已经映射的sql语句
         * */
        SqlSession sqlSession = sqlSessionFactory.openSession();

//        selectOne的两个参数：sql的唯一标识、执行sql要用的参数-->在Mapper中定义
        //根据sql的唯一标识确定要执行哪一个sql语句
        try {
            //通过sqlSession来执行sql语句
            Employee selectOne = sqlSession.selectOne("selectEmp", 2);
            System.out.println(selectOne);
        } finally {
//		关闭sqlSession
            sqlSession.close();
        }

    }
}

```

- 目录结构

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210424215643.png)

- 构建中遇到的问题以及解决方案

1. `org.xml.sax.SAXParseException； lineNumber: 12； columnNumber: 111； 对实体 “characterEncoding“ 的引用必须以 ‘；‘`  ，[参考](https://blog.csdn.net/qq_38842107/article/details/109061124)



2. `could not create connection to database server`  ，[参考](https://blog.csdn.net/robinson_911/article/details/88624695)

## 使用Maven构建MyBatis环境2

接口式编程（方便开发扩展）实现总结
1.创建一个接口(EmployeeMapper.java),其中定义抽象方法
2.将EmployeeMapper.xml中的namespace改为接口的全类名
3.将标签中的"id"改为与接口中定义的抽象方法相同的方法名
4.第2、3步实现了动态绑定，<u>在标签中写方法的具体实现</u>
5.在测试方法中不需要创建接口的实现类，mybatis会为接口自动创建一个代理对象去执行增删改查方法

- 创建EmployeeMapper接口

```java
package org.example.bean;

public interface EmployeeMapper {
    Employee getEmplyeeById(Integer id);
}

```

- 修改sql映射xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--
namespace:名称空间,指定为接口的全类名 ->配置文件和接口动态绑定
    public interface EmployeeMapper {
        Employee getEmplyeeById(Integer id);
    }

id:唯一标识   ->将id设为接口中的方法名，就将唯一标识与方法进行绑定
resultType:返回值类型
#{id}：从传递过来的参数中取出id值
-->
<mapper namespace="org.example.bean.EmployeeMapper">
    <select id="getEmplyeeById" resultType="org.example.bean.Employee">
        select * from tbl_employee where id = #{id}
    </select>
</mapper>
```

- 修改测试代码

```java
package org.example;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.example.bean.Employee;
import org.example.bean.EmployeeMapper;
import org.testng.annotations.Test;

import java.io.IOException;
import java.io.InputStream;

public class EmployeeTest {
    @Test
    public void test() throws IOException {
        /*
         * 1.根据xml配置文件（全局配置文件）创建一个SqlSessionFactory对象
         *
         * */
        String resource = "MyBatis.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

        /*
         *2. 获取sqlSession实例，能直接执行已经映射的sql语句
         * */
        SqlSession sqlSession = sqlSessionFactory.openSession();


//        selectOne的两个参数：sql的唯一标识、执行sql要用的参数-->在Mapper中定义
        try {
            //存在的问题：无法对传入的参数进行类型检查
            Employee selectOne = sqlSession.selectOne("selectEmp", 2);
            System.out.println(selectOne);
        } finally {
            sqlSession.close();
        }
    }


    @Test
    public void test2() throws IOException {
        /*
         * 1.根据xml配置文件（全局配置文件）创建一个SqlSessionFactory对象
         *
         * */
        String resource = "MyBatis.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

        /*
         *2. 获取sqlSession实例，能直接执行已经映射的sql语句
         * */
        SqlSession sqlSession = sqlSessionFactory.openSession();


//        selectOne的两个参数：sql的唯一标识、执行sql要用的参数-->在Mapper中定义
        try {
            /*
            * 3. 获取接口的实现类对象
            * 会为接口自动创建一个代理对象，代理对象去执行增删改查方法
            * */
            EmployeeMapper sessionMapper = sqlSession.getMapper(EmployeeMapper.class);
            Employee emplyeeById = sessionMapper.getEmplyeeById(1);//可以实现类型检查
            System.out.println(emplyeeById);

        } finally {
            sqlSession.close();
        }
    }
}

```

# 全局配置xml文件

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210424223757.png)

## properties设置

mybatis可以使用properties标签来引入外部properties配置文件的内容。

- config.properties配置文件编写

```xml
driver=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybatis_test?serverTimezone=GMT%2B8
username=root
password=123456
```

- 修改全局配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--
    resource:引用类路径下的资源
    url：引用网络或者磁盘路径下的资源
    -->
    <properties resource="config.properties">

    </properties>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>   <!--用${}进行传参-->
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="EmployeeMapper.xml"/>
    </mappers>
</configuration>
```



## settings设置

极为重要的调整设置，它们会改变Mybatis运行时行为。

[链接](https://mybatis.org/mybatis-3/zh/configuration.html)

```java
<settings>
  <setting name="cacheEnabled" value="true"/>
  <setting name="lazyLoadingEnabled" value="true"/>
  <setting name="multipleResultSetsEnabled" value="true"/>
  <setting name="useColumnLabel" value="true"/>
  <setting name="useGeneratedKeys" value="false"/>
  <setting name="autoMappingBehavior" value="PARTIAL"/>
  <setting name="autoMappingUnknownColumnBehavior" value="WARNING"/>
  <setting name="defaultExecutorType" value="SIMPLE"/>
  <setting name="defaultStatementTimeout" value="25"/>
  <setting name="defaultFetchSize" value="100"/>
  <setting name="safeRowBoundsEnabled" value="false"/>
  <setting name="mapUnderscoreToCamelCase" value="false"/>
  <setting name="localCacheScope" value="SESSION"/>
  <setting name="jdbcTypeForNull" value="OTHER"/>
  <setting name="lazyLoadTriggerMethods" value="equals,clone,hashCode,toString"/>
</settings>
```



## typeAliases设置

别名处理器，可自定义java类型别名。
包含单个处理和批量处理。
批量处理可能会出现别名冲突问题（包下的类和包下的包中的类名字相同），可以使用注解@Alias("")为某个类型指定新的别名。
不使用别名，使用类型的全类名，查找类时更方便。
typeHandles：类型处理器，架起java类型和数据库类型映射的桥梁。（互相转换，String<–>CHAR/VARCHAR）

## 类型处理器（typeHandlers）

MyBatis 在设置预处理语句（PreparedStatement）中的参数或从结果集中取出一个值时， 都会用类型处理器将获取到的值以合适的方式转换成 Java 类型。下表描述了一些默认的类型处理器。

## plugins设置

插件，拦截对象方法。Executor、ParameterHandler、ResultSetHandler、StatementHandler。

## environments设置

environments：环境，mybatis可以配置多种环境，开发时可以定义标签的default不同值快速切换环境。

## databaseIdProvider设置

用来支持多数据库厂商，在全局配置文件中添加后为各数据库起别名，在mapper.xml中标签中添加databaseId属性，即可指定该sql语句适用的数据库。
然后修改的default值可以切换运行环境

## mappers设置

mappers：将写好的sql映射文件（EmployeeMapper.xml）注册到全局配置文件中

## 标签编写顺序

在全局配置文件中的标签有严格顺序要求：
Content Model : (properties?, settings?, typeAliases?, typeHandlers?, objectFactory?,
objectWrapperFactory?, reflectorFactory?, plugins?, environments?, databaseIdProvider?, mappers?

# sql映射xml文件

[参考](https://mybatis.org/mybatis-3/zh/sqlmap-xml.html)

MyBatis 的真正强大在于它的语句映射，这是它的魔力所在。由于它的异常强大，映射器的 XML 文件就显得相对简单。如果拿它跟具有相同功能的 JDBC 代码进行对比，你会立即发现省掉了将近 95% 的代码。MyBatis 致力于减少使用成本，让用户能更专注于 SQL 代码。

SQL 映射文件只有很少的几个顶级元素（按照应被定义的顺序列出）：

- `cache` – 该命名空间的缓存配置。
- `cache-ref` – 引用其它命名空间的缓存配置。
- `resultMap` – 描述如何从数据库结果集中加载对象，是最复杂也是最强大的元素。
- `parameterMap` – 老式风格的参数映射。此元素已被废弃，并可能在将来被移除！请使用行内参数映射。文档中不会介绍此元素。
- `sql` – 可被其它语句引用的可重用语句块。
- `insert` – 映射插入语句。
- `update` – 映射更新语句。
- `delete` – 映射删除语句。
- `select` – 映射查询语句。

## Mybatis参数处理

### 单个参数

 mybatis不会做特殊处理，只有一个参数，即使参数名与bean中属性不一致，运行效果也一样。
 #{参数名}：取出参数值。

### 多个参数

 mybatis会做特殊处理，返回的多个参数会被封装成一个map。

key是`param1…paramN`，参数的索引（0，1…）
 value才是传入的参数值

```a
多个参数会被封装成一个Map<key,value>

-------------------
-------------------
key			value
-------------------
param1   	value1
param2   	value2
.
.
.
```



 **#{}**就是从map中获取指定key的值
 取值：`#{param1}，#{param2}`

 多个参数建议使用命名参数，即在封装参数为map时明确指定map的key。  --->即将key进行重新命名

 ```java
Employee getEmpByIdAndLastName(@Param(“id”)Integer id, @Param(“lastName”)String lastName);//通过id和lastname共同查询sql


//param1对应id ；param2对应lastName
//同时通过@Param(“...”)设置后，id对应id ；lastName对应lastName


select * from tbl_employee where id = #{id} and last_name=#{lastname}//将id和last_name进行设置


Employee emplyeeByIdandlastname = sessionMapper.getEmplyeeByIdandlastname(2, "shanghai");//调用getEmplyeeByIdandlastname传入参数进行select语句的调用
 ```

 此时`#{指定的key}`即可取出对应的参数

### POJO

> POJO(Plain Ordinary Java Object)译为：简单的java对象，也就是普通JavaBeans，可以看做是作为支持业务逻辑的协助类。
>
> POJO有一些private的参数作为对象的属性。然后针对每个参数定义了get和set方法作为访问的接口。比如简单的实体类

（简单java对象，简单JavaBean）
如果多个参数正好是业务逻辑的数据模型（传入的参数是Employee的属性值），我们可以直接传入pojo；
\#{属性名}：取出传入的pojo的属性值

### Map

 如果不是业务逻辑的数据模型，没有对应的pojo，不经常使用，为了方便也可以直接传入map
​ #{key}：取出map中对应的值

```java

HashMap<String,Object> map=new HashMap<>();
map.put("id",35);//将参数id设置为35
map.put("lastname","jerry");//将参数lastname设置为jerry



```

### TO（Transfer object）

（DTO，可以理解为写一个新的、继承原来model层实体类、添加新的业务处理方法、专注于数据传输的类）
如果不是业务逻辑的数据模型，但是该方法要经常使用，推荐来编写一个TO（Transfer Object）即数据传输对象

```java
Page{
int index;
int size;
}
```



> 表现层与应用层之间是通过数据传输对象（DTO）进行交互的，数据传输对象是没有行为的POCO对象，它 的目的只是为了对领域对象进行数据封装，实现层与层之间的数据传递。为何不能直接将领域对象用于 数据传递？因为领域对象更注重领域，而DTO更注重数据。不仅如此，由于“富领域模型”的特点，这样 做会直接将领域对象的行为暴露给表现层。
>
> 需要了解的是，数据传输对象DTO本身并不是业务对象。数据传输对象是根据UI的需求进行设计的，而不 是根据领域对象进行设计的。比如，Customer领域对象可能会包含一些诸如FirstName, LastName, Email, Address等信息。但如果UI上不打算显示Address的信息，那么CustomerDTO中也无需包含这个 Address的数据。

### 参数处理扩展思考

一些应用场景

```java
public Employee getEmp(@Param("id")Integer id, String lastName);
```

 取值：id==>#{id/param1}      lastName==>#{param2}

即此时既可以用param1也可以用id进行取值



```java
public Employee getEmp(Integer id, @Param("e")Employee emp);
```


 取值：id==>#{param1}    lastName==>#{param2.lastName/e.lastName}



**特别注意**

如果传入的是Collection（List、Set）类型或者是数组，也会特殊处理。

 也是把传入的list或者数组封装在map中；--->**将可以进行特殊化**

> 如果传入参数类型是`Collection`,对应的key就是`collection`
>
> 如果传入参数类型是`List`,对应的key就是`list`
>
> 如果传入参数类型是`Array`,对应的key就是`array`

```java
public Employee getEmpById(List<Integer> ids);
```

 取值：取出第一个id的值--->   `#{list[0]}`



### 小结

如果传入多个参数会将参数封装成map，为了不混乱可以使用`@Param`来指定封装时使用的key；

### 参数值的获取、#{}与${}

> #{}：可以获取map中的值或者pojo对象属性的值
>
> ${}：也可以获取map中的值或者pojo对象属性的值

#{}和${}的区别:

```sql
select id, last_name lastName, email, gender from tbl_employee where id = ${id} and last_name = #{lastName}

---------------------------------------------
运行后：
Preparing: select id, last_name lastName, email, gender from tbl_employee where id = 5 and last_name = ?



```

> **#{}是以预编译形式将参数设置到sql语句中；PreparedStatement：防止sql注入**
>
>  **${}是将取出的值直接封装在sql语句中；会有安全问题**
>
>  大多情况下使用#{}获取参数的值



 原生jdbc不支持占位符的地方就可以使用${}进行取值（即sql语句不支持？的地方）

 如：select * from ${year}_salary where xxx;

 select * from tbl_employee order by ${f_name} ${order}

#{}更丰富的用法：
 取值的时候可以规定参数的一些规则

 javaType、jdbcType、mode（存储过程）、numbericScale、resultMap、typeHandler、jdbcTypeName、expression（未来准备支持的功能）

 jdbcType在某种特定场景下需要被设置，在有的数据为null的时候，有的数据库无法识别mybatis对null的默认处理。比如Oracle会报错，因为mybatis对所有的null都映射的是原生jdbc的OTHER类型，Oracle不兼容该类型。

 由于全局配置中，jdbcTypeForNull=OTHER，Oracle不支持。两种解决办法：

 1.修改为#{email, jdbcType=NULL}，表示为空时设置为NULL类型，运行成功。

 2.修改全局配置中jdbcTypeForNull=NULL。

## 查询相关

### 1.查询返回List集合

```
public List<Employee> getEmpsByLastNameLike(String lastName);
```

**resultType：如果返回的是一个集合，要写集合中元素的类型**

```xml
 <select id="getEmpsByLastNameLike" resultType="com.bean.Employee">
 	select * from tbl_employee where last_name like #{lastName}
 </select>
```

### 2.查询返回Map集合

#### 返回一条记录的Map

key就是列名，value就是对应的值

```
public Map<String, Object> getEmpByIdReturnMap(Integer id);
```

```xml
<select id="getEmpByIdReturnMap" resultType="map">
 	select * from tbl_employee where id = #{id}
 </select>
```

#### 返回多条记录的Map

返回多条记录封装成一个map，key是这条记录的主键，value是记录封装后的JavaBean

```java
@MapKey("id")//用注解告诉mybatis封装这个map的时候用哪个属性值作为主键
public Map<Integer, Employee> getEmpByLastNameLikeReturnMap(String lastName);
```

```xml
 <select id="getEmpByLastNameLikeReturnMap" resultType="com.bean.Employee">
 	select * from tbl_employee where last_name like #{lastName}
 </select>
```

### 延迟加载

[参考](https://www.jb51.net/article/172739.htm)

MyBatis中的延迟加载，也称为懒加载，是指在**进行表的关联查询**时，按照设置延迟规则推迟对关联对象的select查询。例如在进行一对多查询的时候，只查询出一方，当程序中需要多方的数据时，mybatis再发出sql语句进行查询，这样子延迟加载就可以的减少数据库压力。<u>MyBatis 的延迟加载只是对关联对象的查询有迟延设置</u>，对于主加载对象都是直接执行查询语句的。



### 缓存机制

任何持久层框架都要考虑缓存。

Mybatis包含一个非常强大的查询缓存特性，配置和定制非常方便。缓存可以极大地提升查询效率。

Mybatis默认定义了两级缓存：

 一级缓存（SqlSession级别，也成为本地缓存）和二级缓存（namespace级别）。

 默认情况下，只有一级缓存开启，二级缓存需要手动开启和配置
#### 一级缓存（本地缓存）

就是SqlSession级别的一个Map，<u>与数据库同一次会话期间查询到的数据会存放在本地缓存中，以后如果获取相同的数据直接从缓存中拿</u>

```java
@Test
public void testFirstLevelCache() throws IOException {
		
	SqlSessionFactory sqlSessionFactory = getSqlSessionFactory();
	SqlSession openSession = sqlSessionFactory.openSession();
		
	try {
		com.dao.EmployeeMapper mapper = openSession.getMapper(EmployeeMapper.class);
        
		Employee empById = mapper.getEmpById(1);
		System.out.println(empById);//已经拿到Id为1对应的数据了，将该数据放在缓存中
		
		Employee empById2 = mapper.getEmpById(1);//第2次取id为1的数据，实际上是从缓存中取得
		System.out.println(empById2);
		
        System.out.println(empById == empById2);//两次取得的数据是一样的
			
	} finally {
		openSession.close();
		}
}

打印结果：
DEBUG 03-16 16:12:15,617 ==>  Preparing: select id, last_name lastName, email, gender from tbl_employee where id = ?   (BaseJdbcLogger.java:145) 
DEBUG 03-16 16:12:15,649 ==> Parameters: 1(Integer)  (BaseJdbcLogger.java:145) 
DEBUG 03-16 16:12:15,680 <==      Total: 1  (BaseJdbcLogger.java:145) 
Employee [id=1, lastName=Jeans, email=jerrtLong@163.com, gender=0]
Employee [id=1, lastName=Jeans, email=jerrtLong@163.com, gender=0]
true

```

**示例查询结果中两次调用只发送了一次Sql语句，empById == empById2为true表明两次调用是同一个对象**

##### 一级缓存失效的四种情况

- SqlSession不同时
-  SqlSession相同，查询条件不同时（还未缓存该数据）
-  SqlSession相同，两次查询之间进行了增删改操作（对数据产生了影响）
-  SqlSession相同，手动清除了一级缓存（openSession.clearCache();）
  故一级缓存作用比较小。

### 二级缓存（全局缓存）



namespace级别的缓存，一个namespace对应一个二级缓存

>  1. 一个会话查询一条数据，这个数据就会被存放在当前会话的一级缓存中
>
>  2. 如果**<u>会话关闭</u>**，一级缓存中的数据会被保存到二级缓存中
>
>  3. 不同的namespace查出的数据会放在自己对应的缓存中（map）
>
>     sqlSession==EmployeeMapper==>Employee  -->放在2级缓存中
>
>     ​					DepartmentMapper==>Department  -->放在另一个2级缓存中
>
> - 一个namespace中可能有多个sqlSession的建立，每一次sqlSession查询到的数据都可以缓存到二级缓存中

使用
在全局配置文件config.xml中显式开启二级缓存

```xml
<setting name="cacheEnabled" value="true"/>
```

在mapper.xml中配置使用二级缓存

[官方文档](https://mybatis.org/mybatis-3/zh/sqlmap-xml.html#cache)

```xml
<cache eviction="FIFO" flushInterval="6000" readOnly="" size="1024"></cache>
<cache></cache>则表示所有属性使用默认值
```

cache中的属性

 eviction：缓存的回收策略

 LRU（默认）：移除最近最少使用的

 FIFO：先进先出

 SOFT：软引用，移除基于垃圾回收器状态和软引用规则的对象

 WEAK：弱引用，更积极地移除基于垃圾回收器状态和弱引用规则的对象

 flushInterval：缓存刷新间隔，可以设置一个毫秒值。默认不清空。

 readOnly：是否只读

 true：Mybatis直接将数据在缓存中的引用交给用户，速度快，不安全

 false（默认）：Mybatis会利用序列化和反序列化技术克隆一份新的数据交给用户

 size：缓存存放多少元素

 type：指定缓存的全类名。可以自定义，实现cache接口即可。
------------------------------------------------
```java
@Test
public void testSecondCache() throws IOException {
		SqlSessionFactory sqlSessionFactory = getSqlSessionFactory();
    //创建两个不同的sqlSession
		SqlSession openSession = sqlSessionFactory.openSession();
		SqlSession openSession2 = sqlSessionFactory.openSession();
		
	try {
			//两个sqlSession是在同一个namespace中的（EmployeeMapper）
			com.dao.EmployeeMapper mapper = openSession.getMapper(EmployeeMapper.class);
			com.dao.EmployeeMapper mapper2 = openSession2.getMapper(EmployeeMapper.class);
			
			Employee empById = mapper.getEmpById(1);//查询到id为1的数据，将其放在当前会话的一级缓存中
			System.out.println(empById);
			openSession.close();//将当前sqlSession关闭，将以及一级缓存中的数据保存在二级缓存中
			
			Employee empById2 = mapper2.getEmpById(1);//新的sqlSession会先在2级缓存中查询是否有相应的数据，如果没有就在一级缓存中找，如果还没有，才去数据库中查找
			System.out.println(empById2);
			openSession2.close();
				
	} finally {
	}
}

打印结果
DEBUG 03-16 17:08:01,530 Cache Hit Ratio [com.dao.EmployeeMapper]: 0.0  (LoggingCache.java:62) 
DEBUG 03-16 17:08:01,542 ==>  Preparing: select id, last_name lastName, email, gender from tbl_employee where id = ?   (BaseJdbcLogger.java:145) 
DEBUG 03-16 17:08:01,573 ==> Parameters: 1(Integer)  (BaseJdbcLogger.java:145) 
DEBUG 03-16 17:08:01,605 <==      Total: 1  (BaseJdbcLogger.java:145) 
Employee [id=1, lastName=Jeans, email=jerrtLong@163.com, gender=0]
DEBUG 03-16 17:08:01,620 Cache Hit Ratio [com.dao.EmployeeMapper]: 0.5  (LoggingCache.java:62) 
Employee [id=1, lastName=Jeans, email=jerrtLong@163.com, gender=0]
//"Cache Hit Ratio [com.dao.EmployeeMapper]: 0.5"中的0.5表示两次查缓存一次命中
```



#### 缓存工作原理图示

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210430082657.png)

Mybatis是持久层框架，缓存都是比较简单的实现（封装Map），但是提供了Cache接口，可以进行扩展开发（redis、ehcache）

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210430082909.png)



# Mybatis-ehcache整合

[官方文档](https://github.com/mybatis/ehcache-cache)

> ehcache-cache：第三方缓存库

整合步骤：

- 在pom.xml中添加依赖（基于Maven构建）

```xml
<dependency>
    <groupId>org.mybatis.caches</groupId>
    <artifactId>mybatis-ehcache</artifactId>
    <version>1.2.1</version>
</dependency>
```

- 在mapper XML文件中配置添加

```xml
<mapper namespace="...">
  ...
  <cache type="org.mybatis.caches.ehcache.EhcacheCache"/>
  ...
</mapper>
```

- 在资源路径下添加`ehcache.xml`文件，如果没有`ehcache.xml`或者是加载错误，会使用默认的配置





# Mybatis-Spring整合

[官方文档](http://mybatis.org/spring/zh/getting-started.html)



控制器调用service，service调用mapper，mapper中才是实际的对数据库的操作

# Mybatis-SpringBoot整合
