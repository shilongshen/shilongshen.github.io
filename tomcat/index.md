# Toncat

# Toncat运行原理分析

- Tomcat 是一个免费的、开源的、轻量级的 Web 应用服务器。

- **Web项目的本质，是一大堆的资源文件和方法。Web项目没有入口方法(main方法)，，意味着Web项目中的方法不会自动运行起来。**
- Web项目部署进Tomcat的webapp中的目的是很明确的，那就是**希望Tomcat去调用**
  **写好的方法去为客户端返回需要的资源和数据。**
-  对于Tomcat而言，它并不知道我们会有什么样的方法，这些都只是在项目被部署进webapp下后才确定的，由此分析，必然用到了Java的**反射来实现类的动态加载、实例化、获取方法、调用方法**。但是我们部署到Tomcat的中的Web项目必须是按照规定好的接口来进行编写，以便进行调用
- Tomcat如何确定调用什么方法呢。这取却于客户端的请求，http://127.0.0.1:8080/JayKing.Tomcat.Study/index.java?show这样的一个请求，通过http协议，在浏览器发往本机的8080端口，携带的参数show方法，包含此方法的路径为JayKing.Tomcat.Study，文件名为：index.java。

## HTTP工作原理

<img src="https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210502101329.png" style="zoom:80%;" />

浏览器发送给服务端的是一个HTTP格式的请求，HTTP服务器接收到这个请求后，需要调用服务端程序来处理，所谓的服务端程序就是你写的Java类（业务类），一般来说不同的请求需要由不同的Java类来处理。

<img src="https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210502101656.png" style="zoom:80%;" />

图1表示HTTP服务器直接调用具体业务类，他们是紧耦合的

图2中Http服务器不直接调用业务类，而是将请求交给容器进行处理，容器通过Servlet接口调用业务类。

# 功能组件结构

Tomcat 的核心功能有两个，分别是负责接收和反馈外部请求的连接器 Connector，和负责处理请求的容器 Container。其中连接器和容器相辅相成，一起构成了基本的 web 服务 Service。每个 Tomcat 服务器可以管理多个 Service。

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210502102107.png)

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210502102128.png)

## 连接器（Connector）

### 核心功能

一、监听网络端口，接收和响应网络请求。

二、网络字节流处理。将收到的网络字节流转换成 Tomcat Request 再转成标准的 ServletRequest 给容器，同时将容器传来的 ServletResponse 转成 Tomcat Response 再转成网络字节流。

<img src="https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210502103128.png" style="zoom:67%;" />

### **连接器模块设计**

为满足连接器的两个核心功能，我们需要

- 通讯端点：监听端口；

- 处理器来：处理网络字节流；

- 适配器：将处理后的结果转成容器需要的结构。

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210502102917.png)

对应的源码包路径 `org.apache.coyote`

<img src="https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210502103821.png" style="zoom:67%;" />

## 容器（Container）

### 容器结构分析

每个 Service 会包含一个容器。容器由一个引擎可以管理多个虚拟主机。每个虚拟主机可以管理多个 Web 应用。每个 Web 应用会有多个 Servlet 包装器。Engine、Host、Context 和 Wrapper，四个容器之间属于父子关系。

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210502103305.png)

对应的源码包路径 `org.apache.coyote` 

<img src="https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210502103846.png" style="zoom:80%;" />

容器的请求处理过程就是在 Engine、Host、Context 和 Wrapper 这四个容器之间层层调用，最后在 Servlet 中执行对应的业务逻辑。各容器都会有一个通道 Pipeline，每个通道上都会有一个 Basic Valve（如StandardEngineValve）， 类似一个闸门用来处理 Request 和 Response 。其流程图如下。

<img src="https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210502104018.png" style="zoom:80%;" />

# 参考

[参考](https://blog.csdn.net/qq_32951553/article/details/79686367)

[参考]([你还记得 Tomcat 的工作原理么 - ITDragon龙 - 博客园 (cnblogs.com)](https://www.cnblogs.com/itdragon/p/13657104.html))
