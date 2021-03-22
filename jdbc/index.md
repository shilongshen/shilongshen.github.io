# JDBC


### 什么是JDBC

JDBC API 允许用户访问任何形式的表格数据，尤其是存储在关系数据库中的数据。

`JDBC(Java DataBase Connectivity, 简称JDBC)是Java中用于规范应用程序如何来访问数据库的应用程序接口(API),它提供了查询和更新数据库中数据的方法。`

执行流程：

- 连接数据源，如：数据库。
- 为数据库传递查询和更新指令。
- 处理数据库响应并返回的结果。

在Java程序和数据库之间的一个桥梁。



### JDBC架构

#### 两层

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210302192718.png)

#### 三层

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210302192756.png)



### JDBC 编程步骤

加载驱动程序：

```java
Class.forName(driverClass)
//加载MySql驱动
Class.forName("com.mysql.jdbc.Driver")
//加载Oracle驱动
Class.forName("oracle.jdbc.driver.OracleDriver")
```

获得数据库连接：

```java
DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/imooc", "root", "root");
```

创建Statement或PreparedStatement对象：

```java
conn.createStatement();
conn.prepareStatement(sql);
```



### 完整实例

```java
package com.example.demo;

import com.mysql.jdbc.Driver;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;

public class JdbcTest {
    public  static final String url="jdbc:mysql://localhost:3306/demo1?serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf-8";
    public static final  String user="root";
    public static final  String password="123456";

    public static void main(String[] args) throws Exception {
//        1.加载驱动程序
        Class.forName("com.mysql.cj.jdbc.Driver");
//        2.获得数据库连接
        Connection connection= DriverManager.getConnection(url,user,password);
//        3.操作数据库，实现增删查改
        Statement statement=connection.createStatement();  //Statement对象用于向数据库发送SQL语句
        ResultSet resultSet=statement.executeQuery("select name from student");
//        4.如果有数据，rs.next()返回true
        while (resultSet.next()){
            System.out.println(resultSet.getString("name"));
        }

    }

}

```

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210302194502.png)

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210302194557.png)

### 参考

[链接](https://www.runoob.com/w3cnote/jdbc-use-guide.html)

[链接](https://blog.csdn.net/qq_22172133/article/details/81266048)
