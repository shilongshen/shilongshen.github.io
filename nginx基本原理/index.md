# Nginx基本原理

[参考](https://baijiahao.baidu.com/s?id=1677770814186358219&wfr=spider&for=pc)

[参考](http://tengine.taobao.org/book/chapter_02.html#id12)

(engine x)是一个高性能的HTTP和反向代理web服务器，同时也提供了IMAP/POP3/SMTP服务。其特点是占有内存少，并发能力强，事实上nginx的并发能力在同类型的网页服务器中表现较好，中国大陆使用nginx网站用户有：百度、京东、新浪、网易、腾讯、淘宝等。

Nginx不仅能做**反向代理**，实现负载均衡；还能可以作正向代理来进行上网等功能。

## 反向代理

客户端不需要任何配置就能访问，只需要将请求发送到反向代理服务器，由反向代理服务器去选择目标服务器，获取数据后再返回给客户端。对外就一个服务器，暴露的是反向代理服务器地址，隐藏了真实服务器IP地址。代理对象是服务端，不知道客户端是谁。

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210525152429.png)

## 负载均衡

客户端发送多个请求到服务器，服务器处理请求，有些可能要访问数据库，服务器处理完毕后再将结果返回客户端。

这种架构模式单一，适合并发请求少的情况，但并发量大的时候如何解决？

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210525152659.png)

首先可能想到升级服务器配置，但硬件的性能提升不能满足日益增长的需求，此时想到服务器集群，增加服务器数量，然后将原先请求单个服务器的情况改为将请求分发到多个服务器上，将负载分发到多个服务器上，也就是我们讲的负载均衡。

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210525152735.png)

## 动静分离

为了加快网站的解析速度，可以把动态页面和静态页面交由不同的服务器来解析，减少服务器压力，加快解析速度。

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210525152822.png)

## 原理

### master和worker

master接收信号后将任务分配给worker进行执行，worker可有多个。

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210525153149.png)

### worker如何工作

客户端发送一个请求到master后，worker获取任务的机制不是直接分配也不是轮询，而是一种争抢的机制，“抢”到任务后再执行任务，即选择目标服务器tomcat等，然后返回结果。

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210525153308.png)


