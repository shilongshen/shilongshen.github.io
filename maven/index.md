# Maven


![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210220092051.png)

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210220092209.png)

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210220101014.png)

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210220101133.png)

# Maven的核心概念

## 约定的目录结果

例如在com.maven中建立Hello项目

Hello
		|---src
		|---|---main
		|---|---|---java
		|---|---|---resources
		|---|---test
		|---|---|---java
		|---|---|---resources
		|---pom.xml

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210222085656.png)

## POM

Project Object Model,项目对象模型

pom.xml对于Maven工程是核心配置文件，与构建过程相关的一切设置都在这个文件中进行设置。



## 坐标

使用下面三个坐标在仓库中唯一定位一个maven工程

- **g**roup：公司或组织域名倒序+项目名

  ```xml
  <groupid>com.maven</groupid>
  ```

  

- **a**rtifactid：模块名

  ```xml
  <artifactid>Hello</artifactid>
  ```

  

- **v**ersion：版本

  ```xml
  <version>1.0.0</version>
  ```


```xml
<！--例-->
<groupId>junit</groupId>
<artifactId>junit</artifactId>
<version>4.12</version>

    对应路径
C:\Users\ssl\.m2\repository\junit\junit\4.12\junit-4.12.jar
```



## 依赖

- maven解析依赖信息时会到本地仓库中查找被依赖的jar包

  - 对于我们自己开发的maven工程，使用**mvn install **命令安装后就可以进入仓库

- 依赖的范围

  ![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210221221650.png)

  - compile依赖范围

    - 对主程序是否有效：有效
    - 对测试程序是否有效：有效
    - 是否参与打包：参与

  - test依赖范围

    - 对主程序是否有效：无效
    - 对测试程序是否有效：有效
    - 是否参与打包：不参与

  - provided依赖范围

    ![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210221222423.png)

    - 对主程序是否有效：有效
    - 对测试程序是否有效：有效
    - 是否参与打包：不参与
    - 是否参与部署：不参与

    

## 仓库

仓库分类：

- 本地仓库当前电脑上部署的仓库目录，为当前电脑上所有Maven工程服务

- 远程仓库

  - 私服：搭建在局域网环境中，为局域网范围内的所有maven工程服务

    ![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210221215059.png)

  - 中央仓库：架构在Internet上，为全世界所有maven工程服务

  - 中央仓库的镜像：为了分担中央仓库的流量，提升用户的访问速度

仓库中保存的内容：maven工程

- maven本身所需要的插件
- 第三方框架或工具的jar包
- 我们自己开发的maven工程

## 生命周期/插件/坐标

- 各个构建环节执行的顺序：不能打乱顺序，必须按照既定的正确顺序来执行

- maven的核心程序中定义了抽象的生命周期，生命周期中各个阶段的具体任务时由插件来完成的。

- maven的核心程序为了更好的实现自动化构建，按照这一特点执行生命周期中的各个阶段:无论现在要执行生命周期中的哪一个阶段，都是从这个生命周期最初位置开始执行。

- 插件和目标

  - 生命周期的各个阶段仅仅定义了要执行的任务是什么

  - 各个阶段和插件的目标时对应的

  - 相似的目标由特定的插件来完成

    ![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210221225029.png)

  - 可以将目标由特定的插件来完成





## 继承

## 聚合



# 常用maven命令

```sh
mvn clear  : 清理
mvn compile ：编译主程序
mvn test-compile : 编译测试程序
mvn test : 测试程序
mvn package ： 打包
mvn install : 安装
mvn site : 生成站点


注意：所有与构建过程相关的命令，必须进入pom.xml的目录下执行
```

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210222100305.png)

# Maven环境下构建多模块项目

以四个模块为例来搭建项目：

- maven_parent : 基模块，就是常说的parent（pom）
- maven_dao ： 数据库访问层，例如jdbc操作（jar）
- maven_service : 项目的业务逻辑层 （jar)
- maven_controller ：用来接收请求，相应数据 (war)

依赖关系maven_controller->maven_service->maven_dao




