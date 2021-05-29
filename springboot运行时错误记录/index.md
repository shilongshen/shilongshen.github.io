# SpringBoot运行时错误记录


#### java.sql.SQLException: The server time zone value '�й���׼ʱ��' is unrecognized

[参考](https://blog.csdn.net/qq_43072399/article/details/103102934)

解决方法:

```
在URL上加上：?serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf-8
```

#### Establishing SSL connection without server‘s identity verification is not recommende

[参考](https://blog.csdn.net/qq_41785135/article/details/85118329)

解决方法

```
在你连接数据库的url后面加上参数即可
 jdbc:mysql://localhost:3306/testdb?useSSL=false

使用上述标红的URL,即可解决该警告,标红参数前面为你数据库连接URL,如果有多个参数记得用&连接,例如

 jdbc:mysql://localhost:3306/testdb?characterEncoding=utf-8&useSSL=false

```

#### template might not exist or might not be accessible by any of the configured Template Resolvers

解决方法：

在application.yaml中设置

```yaml
spring:
  thymeleaf:
    prefix: classpath:/templates/
```

注意：thymeleaf 的classpath要多一个斜杠的，漏斜杠就会有问题

#### SLF4J:Failed to load class “org.slf4j.impl.StaticLoggerBinder”

[参考]([idea springboot启动报SLF4J:Failed to load class "org.slf4j.impl.StaticLoggerBinder"_星辰海的专栏-CSDN博客](https://blog.csdn.net/u010696630/article/details/84991116))

未加载`SLF4J jar包`

在pom.xml中添加

```xml
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-simple</artifactId>
    <version>1.7.25</version>
    <scope>test</scope>
  </dependency>
```

#### [main] WARN net.sf.ehcache.config.ConfigurationFactory - No configuration found

[参考](http://www.voidcn.com/article/p-qqtfjhqn-kb.html)

从ehcache.jar 中把文件ehcache-failsafe.xml 解压出来改名 ehcache.xml 复制到classes下面就行了！
