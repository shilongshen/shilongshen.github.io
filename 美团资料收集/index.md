# 

[参考](https://www.nowcoder.com/discuss/626697?type=all&order=time&pos=&page=1&channel=-1&source_id=search_all_nctrack)

[参考](https://www.nowcoder.com/discuss/630466?type=post&order=time&pos=&page=1&channel=-1&source_id=search_post_nctrack)

[参考](https://www.nowcoder.com/discuss/430055?type=post&order=time&pos=&page=1&channel=1009&source_id=search_post)

[参考](https://www.nowcoder.com/discuss/424119?type=post&order=time&pos=&page=1&channel=1009&source_id=search_post)

[参考](https://www.nowcoder.com/discuss/416239?type=post&order=time&pos=&page=1&channel=1009&source_id=search_post)





作者：怪地平小
链接：https://www.nowcoder.com/discuss/626697?type=all&order=time&pos=&page=1&channel=-1&source_id=search_all_nctrack
来源：牛客网



自我介绍

是考过研的, 对计算机网络有了解吗? 讲讲TCP协议, 讲讲TCP的三次握手. 为什么要三次握手, 两次可以吗?

​	TCP协议用于应用程序之间实现可靠通信。因为TCP是面向有连接的传输层协议，所以在通信之前会做好通信两端的准备工作。三次握手就是用于建立可靠的通信连接。第一次握手是客户端向服务端发送SYN同步信号，服务端接收到SYN同步信号后，会返回同步信号和确认应答信号。客户端接受到确认应答信号后就向服务端返回一个确认应答信号，服务端接收到一个确认应答信号双方就可以建立连接了。

​	进行三次握手的目的是为了防止失效的请求连接到达服务端后，服务端建立错误的连接。

讲讲数据链路层在网络中的作用, 比如物理层就是代表电信号传播, 那数据链路层是干嘛的呢? (这题我感觉没有太答上来, 我回答了数据链路层的MAC协议, 面试官说这个是从协议上看的. 于是我又从数据链路层对应的是交换机这一点上继续作答了)

​	数据链路层的作用是在互连的设备之间传输数据帧。

对操作系统有了解吗? 能讲讲进程与线程的区别吗?

​	进程是正在运行程序的实例，是资源调度的最小单位、一个程序中可以有多个进程；而线程可以看作是更小的进程，是CPU调度的最小单位，一个进程中的线程共享进程中的资源，包括打开的文件、地址空间以及全局变量

线程有调度[算法](), 进程也有调度[算法](), 能讲讲你知道哪些进程调度[算法]()吗? (LRU(不太算); 先进的先运行; 按优先级顺序运行; 按占用时间最少优先)(这个答得不太好)

​	1.先到先得 2.短作业优先 3.时间片轮转 4.按照优先级 5.多级反馈

知道死锁吗? 能讲讲编码中怎么搞出来一个死锁吗? 如何预防死锁? (这里我答得银行家[算法]()以及不同线程按同样顺序获取资源可以预防死锁)

​	当有两个或者是两个以上的进程互相等待对方的资源就会造成死锁。这里可以先分析死锁产生的条件：第一个条件是资源只能够被一个进程占用；第二个条件是一个资源被一个进程占用后不可以强行抢占；第三个条件是一个进程占用一个资源后，还可以继续等待其他的资源；第四个条件是至少有两个或者两个以上的进程在相互等待对方的资源，存在环路。针对这些条件就可以提出相应的解决方案：第一个就是手动抢占资源；第二个就是在占用资源前提前申请所需要的所有资源，如果不能够申请到，就放弃占用资源，等待一段时间后再申请；第三个是对资源进行编号，按照顺序占用资源。

​	

[算法题](): [有效括号](https://leetcode-cn.com/problems/valid-parentheses/) [手撕快排](https://leetcode-cn.com/problems/sort-an-array/)

Java里, `equals()`方法和`==`的区别? 都是怎么用的?

​	对于基本数据类型`==`比较的是内容，对于引用数据类型`==`比较的是内存地址；

​	Object类的`equals`比较的是内存地址是否相等，如果想要比较内容是否相等就要重写`equals`

为什么说重写了`equals()`方法就得重写`hashCode()`方法呢?

​	`hashcode`计算的是该对象的哈希码，用于确定该对象在哈希表中的位置，如果`equals`相等那么哈希码一定相等，所以...

Java里, 你用过那些集合?

​	`ArrayList,LinkList,HashSet,TreeSet`

知道那些并发的关键字或者类?

​	`synchronized,volatile,ThreadLocal,`

之前你提到了AQS, 能再讲讲你之前说的那些类里面, 有哪些是使用了AQS实现的?

用线程的时候, 肯定是用线程池来获得线程的. 讲讲线程池的参数. 每个参数都是什么意思?

​	corepoolsize:核心线程数量：线程池中允许运行的最小线程数

​	maximunpoolsize :最大线程数：当等待任务等于等待队列的容量时，线程池中的运行线程数为最大线程数

​	keepAliveTime: 当任务数小于核心线程数时，核心线程外的线程不会立刻销毁，而是等到keepAliveTime后才销毁

​	unit: keepAliveTime的时间单位

​	workqeque： 等待队列，当任务数大于核心线程数，会将任务放在等待队列中进行等待

​	ThreadFactory:  饱和策略，当运行的线程数等于最大线程数，并且等待队列满了，如果这个时候有新的任务来了，会根据任务拒绝策略处理这个新来的任务

​	handler:饱和策略

Spring两个重要的特性是什么(AOP, IOC), 都干嘛的?

Spring中, 你用过`@Controller`, `@Service`, `@Component`注解吗? 他们的作用是什么? (作用一样) 为什么要单独弄出来这么多个作用一样的?

`@Autowired`和`@Inject`住解都是用来干嘛的, 有什么区别?

你的[项目]()里用的数据库是MySQL吗? 有多少条数据(10条不到)? 那你有没有用到索引?(innodb默认给主键加上了聚簇索引)

看来你知道索引的, 能讲讲索引是以什么数据结构组织的吗? (b+树, 而且索引单独以一个文件保存)

作者：wao3
链接：https://www.nowcoder.com/discuss/630466?type=post&order=time&pos=&page=1&channel=-1&source_id=search_post_nctrack
来源：牛客网

讲[项目]()
Spring、SpringMVC
IOC和DI的关系
依赖注入的方式
看过Spring底层[源码]()没？
看过SpringMVC底层[源码]()没？
SpringMVC的注解
@RequestBody的原理
SpringMVC 如何将URL映射到指定的方法上
JVM对象内存布局

​	JVM对象内存布局分为对象头，实例数据和对齐指针。

JVM运行时数据区

​	包括了堆、方法区、栈和程序计数器。其中对用于存放对象；方法区用于存放加载的类型信息字段信息、方法信息、静态变量和常量；每一个方法的开始调用和结束都对应一个栈帧的入栈和出栈，栈帧包含了局部变量表、操作数栈、动态连接、方法出口等信息、、。

字符串常量存在哪个位置？

​	存放在方法区中的运行时常量池中

程序计数器的作用

​	class文件的行号指示器

异常或者递归时，程序计数器是怎样的？
讲讲垃圾回收[算法]()

​	垃圾收集算法包括了，标记-清除算法；标记-复制算法以及标记-整理算法；标记-清除算法就是标记存活的对象，将没有被标记的对象进行回收，这种方法有可能会产生空间碎片，比较适合在老年代使用；标记-复制算法就是将内存一分为2，每次只将对象放在其中一部分，当这部分空间不足的时候，就将存活的对象复制到另一部分，使用这种方法可用于存放对象的空间就减半了，适合新生代进行垃圾回收。标记-整理算法就是在标记-清除算法的基础上，标记存活的对象然后将存活的对象移动到内存的一个角落，然后进行回收，因为这个方法多了一个移动的步骤，所以效率较低。

如何解决跨代引用



JDK8是怎样解决跨代引用的？
了解CMS吗？讲讲CMS的步骤
了解JVM调优吗？
重写finalize后的垃圾收集
JUC是基于什么搭建起来（想问volatile，我答的AQS）
讲讲volatile
AQS和volatile的关系？（state用volatile修饰）
讲讲重[排序]()
线程池用过吗？讲讲线程池参数



有哪些拒绝策略

	- 直接异常，阻止系统正常工作
	- 放弃执行无法处理的任务，不做任何处理
	- 将任务队列中最老的n那个任务丢弃
	- 使用调用者线程来执行任务

讲讲线程池加入任务的步骤

​	

[项目]()中用过线程池吗？
[redis]()的set底层原理
[redis]()的IO模型
讲讲IO多路复用
讲讲HTTP协议
讲讲HTTP2.0
讲讲MVCC
讲讲数据库中的锁
讲讲你熟悉的设计模式？
手写一个适配器模式（有点卡壳，然后让我写一个观察者模式也行）
给你A,B两个文件，各存放50亿条URL，每条URL占用64字节，内存限制是4G，让你找出A,B文件共同的URL？
[算法题]()：[链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

作者：wao3
链接：https://www.nowcoder.com/discuss/630466?type=post&order=time&pos=&page=1&channel=-1&source_id=search_post_nctrack
来源：牛客网



你认为你一面有哪些问题回答得不好？
谈谈你对MVC的理解？
MVC和MVVM的区别？谈谈各自的优缺点
在学校有实验室[项目]()吗？（有个[前端]()的）
找实习为什么不选[前端]()？
怎么学习框架和中间件的？
怎么看的书？
为什么不考研？
问[项目]()
还有过什么团队合作经验吗？
你觉得后端开发需要做哪些工作？

​	1.参与产品评议会议

​	2.前后端沟通确定API文档

​	3.数据库变更设计

​	4.第三方服务选型

​	5.实现业务逻辑

​	6.自测并跑通产品业务流程

​	7.组织前后端联调

​	8.交付产品经理确认

​	9.交付测试确认

​	10.正式上线产品功能

写代码，三个线程按顺序轮流打印5个数，打印到60为止。

> 比如：
> 线程1：1 2 3 4 5
> 线程2：6 7 8 9 10
> 线程3：11 12 13 14 15
> 线程1：16 17 18 19 20
> ...
> 顺序打印到60为止

作者：十二月的流浪肖邦
链接：https://www.nowcoder.com/discuss/430055?type=post&order=time&pos=&page=1&channel=1009&source_id=search_post
来源：牛客网



http属于七层网络协议的哪一层 

  http协议是否可靠 

​	不可靠、通过http协议可能会存在几个问题，第一个是通信过程中信息可能会被窃听，第二个是通信双方的身份无法进行确定，第三个是通信的信息有可能会被篡改；

​	因此可以采用https进行传输；https就是在http+通信加密+身份认证+完整性保护；通信加密是通过非对称加密和对称加密相结合的方式实现；身份认证是通过第三方认证机构进行实现；完整性保护是通过确认报文摘要来实现；

  session与cookie 

​	因为http协议是不保存状态的协议；因此需要借助一些其他的手段来达到保存通信状态的目的；cookie就是将用户的信息保存在浏览器中，浏览器对服务器发送请求的时候会将cookie带上，服务器通过cookie来识别来访者的状态，比较常见的应用场景就是在一个购物网站中跳转不同的页面，登录的状态是一直保持不变的。session就是将用户的信息保存在服务端，通过sessionID来进行识别，浏览器对服务器发送请求的时候会在cookie中带上sessionID，比较常见的应用场景就是给购物车添加商品，通过session来跟踪用户。

  四次挥手 

**操作系统：**

  线程与进程 

​	进程是正在运行程序的实例、是资源调度的最小单位；而线程可以看成是更小的进程、一个进程中可以有多个线程、这些线程是相互合作的、并且共享进程的资源。

  进程的状态转变 

​	进程的状态有：新建、就绪、运行、阻塞和结束

**MySQL：**

  innoDB引擎与MyIASM引擎的区别 

​	innoDB：支持事务；MyIASM：不支持事务

​	innoDB：即支持行锁也支持表锁；MyIASM只支持表锁

​	innoDB：支持外键；MyIASM不支持外键

​	innoDB：支持MVCC；MyIASM: 不支持MVCC

  事务隔离级别 

​	读未提及

​	读提交

​	可重复读

​	串行

  mysql索引最左前缀原则 

  mysql的mvcc怎么实现的 

**Java基础：**

  static关键字 

  threadLocal 

  线程池七大参数 

​	核心线程参数：线程池中最小正在运行的线程数

​	最大线程参数：当等待队列满时，会调用其他的线程，这个参数规定了线程池中的最大线程参数

​	等待队列：当新任务来时，如果核心线程全部处于运行状态，会将新任务放在等待队列中

​	keeplivetime：当任务数小于核心线程数时。其他的线程不会立刻销毁，而是等待keeplivetime时间后才销毁

​	keeplivetime的时间单位

​	Handel：任务拒绝策略

​	threadfactory: 

​	当线程数等于最大线程数，并且等待队列也放满，这个时候如果又来了新的任务，该如何处理呢：

​	1..直接抛出异常	2.直接忽略新任务，不处理 3.丢弃等待队列中最老的任务	4.将队列中的任务放在调用者线程中执行

  协程 

  equal与hashcode 

  hashMap 


  **linux:** 常用指令 

  常用查找方式 

​	where\whereis\find\



  **[算法题]()** ：[合并有序链表]()

作者：十二月的流浪肖邦
链接：https://www.nowcoder.com/discuss/430055?type=post&order=time&pos=&page=1&channel=1009&source_id=search_post
来源：牛客网



浏览器输入 [www.meituan.come](http://www.meituan.come)之后发生的事 

  TCP如何保证可靠性 

  http与https 

​	

  http状态码 

​	1xx：正在进行处理

​	2xx: 处理成功；200：成功处理；204:不包含内容实体；203：返回范围内的部分内容

​	3xx: 重定向：301：永久重定向；302：临时重定向；303：与302类似，明确要求客户端应该采用 GET 方法获取资源

​	4xx: 客户端出错：403：禁止访问；404：页面不存在

​	5xx： 服务端出错；500：服务端正在执行请求时出现错误；503：超负载或正在维修

  CountDownLatch、CyclicBarrier、Semaphore 实际用法与原理比较 

  线程之间的通信 

​		volatile

​		等待/通知机制  -->wait  和  notify

  synchronized与Lock的区别 

​	synchronized：重量级锁，是基于JVM实现的，是隐式的获得锁和释放锁，只能够实现非公平锁，是悲观锁

​	Lock是一个接口：ReentrantLock通过lock和unlock显式的获得锁和释放锁，是基于API层面实现的，既能够实现公平锁又能够实现非公平锁。是乐观锁。

  AQS原理 

​	AQS就是如果共享变量是空闲的，不被其他线程占用，就可以直接占用共享变量，并加锁。如果共享变量被其他线程占用了，就将线程封装成一个节点加入到同步队列中。

  负载均衡[算法]() 

  阻塞队列 

  多线程创建方式并比较 

​	1.通过继承Thread直接创建

​	2.通过实现running接口创建，重写run()方法创建

​	3.通过实现Callable接口，重写call()方法创建

​	2，3的区别在于3可以返回一个future类型，而2没有返回值，并且call方法会抛出异常而2不会



  [算法]()：归并[排序]()

作者：求offer求鞭挞
链接：https://www.nowcoder.com/discuss/416239?type=post&order=time&pos=&page=1&channel=1009&source_id=search_post
来源：牛客网

spring的IOC，AOP原理
springmvc的工作流程
handlemapping接收的是什么
[项目]()中你用到了MyBatis，说说#和$的区别？
MyBatis你写的XML怎么绑定对应的接口？namespace.id
Spring有几种方式定义Bean。
git你用过哪些命令？

​	`git add.`

​	`git commit -m ""`

​	`git push origin master`

那你知道怎么强制push吗？

​	`git push -f origin master`

linux怎么在查找目录下的一个文件？

​	`find . -name "文件名"`-->在整个磁盘进行寻找

​	`locate `、`whereis`  -->在数据库进行寻找

集合类了解吗？知道哪些集合？
集合类之间的继承结构是怎样的？
this和super关键字的区别？

​	this相当于指向当前对象的一个指针，而super相当于指向当前对象最近一个父类的一个指针

你说你了解HashMap，说说1.7和1.8的底层结构，以1.7为例说说put的流程
HashMap线程安全吗？不安全的场景？
HashMap里负载因子设置大会怎样？设置小会怎样？
JVM有哪些GC[算法]()，各自优缺点
一个表有学生、成绩、科目三列，怎么查各科最好成绩

```
+-------+-------+--------+
| name  | score | kemu   |
+-------+-------+--------+
| zhang |    98 | yuwen  |
| zhang |    90 | shuxue |
| zhang |    95 | yingyu |
| li    |    94 | yuwen  |
| li    |    95 | shuxue |
| li    |    80 | yingyu |
| wang  |    87 | yuwen  |
| wang  |    97 | shuxue |
| wang  |    93 | yingyu |
+-------+-------+--------+
```



```mysql
select name,kemu, max(score) as maxscore from students group by kemu;
```

```
+--------+----------+
| kemu   | maxscore |
+--------+----------+
| yuwen  |       98 |
| shuxue |       97 |
| yingyu |       95 |
+--------+----------+
```

InnoDB聚簇索引和非聚簇索引的区别

​	

最左前缀原则
怎么删表?drop、truncate、delete

​	drop: 删除整个表 `drop table 表名`  -->清空表的数据以及表的结构

​	truncate: 清空表数据 `truncate table 表名`  -->清空表的数据但是不清空表的结构

​	delete：删除表中的某一行 `delete from 表名 where 指定某一行`

​	update： 将表中某一行数据进行更新 `update 表名 set 列名=值 where 指定的行`

​	alter: 对表的一列进行操作 `alter table 表名 add 列名 数据类型 ` -->给表添加一列 ； `alter table 表名 drop 列名`-->将表的一列删除



线程状态有哪些

​	新建，准备，运行，阻塞，结束

wait和sleep的区别

​	wait会将当前占有的锁释放掉，并进入等待队列，而sleep不会释放掉当前占有的锁，而是进入睡眠状态。

​	sleep是Thread的静态方法，而wait是Object的方法

​	wait需要在同步语句块中使用

​		

start和run方法的区别
[redis]()基本数据结构
[redis]()单线程模型
[leetcode]() 41题，On复杂度写

作者：求offer求鞭挞
链接：https://www.nowcoder.com/discuss/416239?type=post&order=time&pos=&page=1&channel=1009&source_id=search_post
来源：牛客网

描述从输入一个url到得到结果的过程
springMVC的执行过程
MVC设计模式
类加载
双亲委派机制
static变量初始化在哪个阶段
jvm内存区域分布
jvm堆中的内存区域分布
垃圾收集[算法]()
什么时候发生fullgc，minorgc
CMS收集器，为什么要初始标记和重新标记要stop the world？
final、finally、finalize()
synchronized和volatile
volatile是线程安全的吗?举个例子
synchronized是可重入的吗？是公平还是非公平
公平锁和非公平锁的区别
synchronized的偏向锁，轻量级锁，重量级锁
synchronized修饰静态方法和非静态方法的区别？class对象，对象实例作为锁
threadlocal底层原理
[redis]()如何实现高并发
mysql的主从复制机制
设计选课考试系统的表，功能有学生选课、学生考试、学生查询成绩。
有10亿个电话号码，找出重复的。
编程：将阿拉伯数字转换成中文数字。如（int）123456->十二万三千四百五十六



作者：求offer求鞭挞
链接：https://www.nowcoder.com/discuss/416239?type=post&order=time&pos=&page=1&channel=1009&source_id=search_post
来源：牛客网

30min
直接深挖[项目]()
实习过吗？
主要负责哪些功能模块？
登录怎么实现权限验证？
数据库多少用户人数？
数据库隔离级别？
数据库最大的表大概多大？（好细啊）
数据库查询优化怎么做的？
怎么看用expain查sql语句执行计划的结果？
怎么记录用户的登录状态？（业务场景下多种实现思路）
cookie和session？
分布式session？
了解哪些分布式的技术？
zoo[keep]()er的应用场景？
zon[keep]()er如何实现分布式锁？
有什么要问的？







作者：linlinnnn
链接：https://www.nowcoder.com/discuss/637783?type=2&channel=-1&source_id=discuss_terminal_discuss_hot_nctrack
来源：牛客网

1.Object类中都有什么方法呢

​	`clone`, `toString`, `wait`, `notify`, `notifyall`, ``

 2.hashcode()和equals的关系是什么？重写equals一定要重写hashcode吗
 \3. hashmap的底层？[红黑树]()相比于[链表]()好在哪？为什么[链表]()长度大于8的时候要转换为[红黑树]()（为什么是8）？
 \4. 实际应用中是谁在调用wait()方法，线程发生了什么？线程被挂起在哪里？wait和sleep的区别？
 5.wait和notify是使用的时候要配合什么关键字？为什么要在sychronized代码块中调用wait？
 6.可重入锁？重入多次的话锁是怎么释放的？
 7.happen-before原则？满足happen-before原则的关键字有什么？讲一下volatile？JMM，具体讲一下指令重排
 8.解释一下JIT？（什么玩意）
 9.JDK1.8中内存的JVM内存的分配情况？运行时常量池具体在哪里
 10.GC[算法]()
 11.讲一下https。为什么要采用混合加密的方式？相对于直接采用非对称加密效率是快了还是慢了
 12.http状态码3xx是什么？302和303的区别是什么

​	重定向

 13.innoDB底层索引和行锁、表锁的关系
 14.B+树和B树的区别，为什么要使用B+树不用B树？

 [算法题]()：树的层序遍历

---



### 表达式求值

[链接](https://www.nowcoder.com/question/next?pid=21910764&qid=894462&tid=43110059)

思路：将字符串分别进栈操作，当栈顶为"and"时弹出并操作，保证栈中只有"true","false","or",随后再进行判断

```java
import java.util.*;


public class Main {
    public static void main(String[] args) {
//        String s="true or false and false";
        Scanner in = new Scanner(System.in);
        String s = in.nextLine();
        String[] ss = s.split(" ");
        Stack<String> path = new Stack<>();
//        System.out.println(Arrays.toString(ss));
        //将字符串分别进栈操作，当栈顶为"and"时弹出并操作，保证栈中只有"true","false","or",随后再进行判断
        for (int i = 0; i < ss.length; i++) {
            String cur = ss[i];
            if (path.isEmpty()) {//如果栈为空
                if (cur.equals("and")||cur.equals("or")){//如果为and 或 or 直接输出error
                    System.out.println("error");
                    return;
                }else{
                    path.push(cur);
                }
            } else if (cur.equals("true") || cur.equals("false")) {//如果栈不为空，且cur字符串为true或false
                String top = path.peek();//栈顶字符
                if (top.equals("true") || top.equals("false")) {//如果栈顶元素为true或false，说明出现连续的true或false，直接返回error
                    System.out.println("error");
                    return;
                } else if (top.equals("and")) {//如果栈顶元素为and
                    String topdel = path.pop();//将and出栈
                    String predel = path.pop();//将and的前一个字符出栈
                    if (cur.equals("true") && predel.equals("true")) {//进行and操作,
                        path.push("true");//cur and predel -->只有两者同时为true,and的结果才为true
                    } else {
                        path.push("false");
                    }
                } else {//当栈顶元素为or,直接将cur入栈
                    path.push(cur);
                }

            } else if (cur.equals("and") || cur.equals("or")) {//如果栈不为空，且cur字符串为and或or
                String top = path.peek();
                if (top.equals("and")||top.equals("or")){//如果栈顶元素为and或or，直接输出error
                    System.out.println("error");
                    return;
                }else {
                    path.push(cur);
                }
            }
        }

        if (!path.isEmpty()&&(path.peek().equals("and")||path.peek().equals("or"))){
            System.out.println("error");//如果栈顶元素为and 或 or ，直接输出error
            return;
        }
        //此时栈中只剩下 true or false, 只要存在一个true必然输出true,否则输出false
        int flag=0;
        while (!path.isEmpty()){
            String top=path.pop();
            if (top.equals("true")){
                flag=1;
                break;
            }
        }
        if (flag==1){
            System.out.println("true");
        }else{
            System.out.println("false");
        }

    }
}
```

### 通配符匹配

[链接](https://leetcode-cn.com/problems/wildcard-matching/solution/tong-pei-fu-pi-pei-by-shilongshen-e56k/)

```java
import java.util.*;

public class Main{
    public static void main(String[] args){
        Scanner in=new Scanner(System.in);
        String P=in.nextLine();
        String T=in.nextLine();
        int lenp=P.length();
        int lent=T.length();
        boolean[][] dp=new boolean[lent+1][lenp+1];
        dp[0][0]=true;
        for(int i=1;i<=lenp;i++){
            if(P.charAt(i-1)=='*'){
                dp[0][i]=true;
            }else{
                break;
            }
        }
        
        for(int i=1;i<=lent;i++){
            for(int j=1;j<=lenp;j++){
                if(P.charAt(j-1)=='*'){
                    dp[i][j]=dp[i-1][j]||dp[i][j-1];
                }
                if(P.charAt(j-1)=='?'||P.charAt(j-1)==T.charAt(i-1)){
                    dp[i][j]=dp[i-1][j-1];
                }
            }
        }
        
        if(dp[lent][lenp]==true){
            System.out.println(1);
        }else{
            System.out.println(0);
        }
        
            
    }
}
```

### 美团骑手包裹区间分组

[链接](https://www.nowcoder.com/question/next?pid=21910764&qid=894515&tid=43110059)

```java
import java.util.*;

public class Main{
    public static void main(String[] args){
        Scanner in=new Scanner(System.in);
        String s=in.nextLine();
        //先判断当前字符的最后一个位置在哪,再不断的更新另一个指针使得开始指针与结尾指针中间的元素在后面不会出现。

        int i=0;
        int j=0;
        int len=s.length();
        while(j<len){
            char lastvalue=s.charAt(j);
            int lastindex=s.lastIndexOf(lastvalue);
            i=j;
            i++;
            int pre=j;
            j=lastindex;
            
            while(i<j){//为了确保开始指针和结尾指针中间的元素不会在结尾指针后面出现
                char curvalue=s.charAt(i);
                int curindex=s.lastIndexOf(curvalue);
                j=Math.max(j,curindex);
                i++;
            }
            
            j++;
            System.out.print(j-pre+" ");
        }
    }
}
```

### 字符串子序列的个数

```
/**
一天，小美在写英语作业时，发现了一个十分优美的字符串：
这个字符串没有任何两个字符相同。
于是，小美随手写下了一个字符串，
她想知道这个字符串的的所有子序列，有多少个是优美的。
由于答案可能会很大，输出对20210101取模后的结果。
一个字符串的子序列定义为：
原字符串删除0个或多个字符后剩下的字符保持原有顺序拼接组成的字符串为原串的子序列。
如：ab是acba的子序列，但bc则不是。在本题中。空串也为原串的子序列。
 *
 * 两个子序列不相同，当且仅当他们对应原串的下标不相同。如aab则含有两个子序列ab。
 */

```

```java

/*
将每个字符的次数加1，全部相乘  ？没有理解
*/
public class bean1 {
    public static void main(String[] args) {
        String s="abcb";
        int len=s.length();

        int[] cnt=new int[26];
        for (int i=0;i<s.length();i++){
            cnt[i]=s.charAt(i)-'a';//统计每个字符串的次数
        }
        int ans=1;
        for (int i=0;i<26;i++){
            ans=ans*(cnt[i]+1)%20210101;
        }
        System.out.println(ans);


//        int[] dp=new int[len+1];
//        Arrays.fill(dp,1);
//        for (int i=1;i<=len;i++){
//            if (s.substring(0,i).contains(String.valueOf(s.charAt(i)))){
//                dp[i]=dp[i]+dp[i-1];
//            }else{
//                dp[i]=dp[i]+(dp[i-1]+1);
//            }
//
//        }
//        System.out.println(dp[len]);
    }
}
```



### 切蛋糕

```
/**
 * @author zhangbingbing
 * @version 1.0
 * @date 2021/4/4 10:57
今天是小美的生日，妈妈给她专门订制了一个球形的大蛋糕。
小美决定对这个蛋糕切n刀。每次切小美会选择是横着切还是竖着切。
将整个球划分经纬度。
如果是横着切的话那么小美会选择一个纬度将整个球切成上下两部分；
如果是竖着切的话那么小美会选择一个经度将整个球切成两半。
 *
 * 小美想知道，切n刀之后整个球被划分成了多少个部分？
 *
 * 请注意本题中经纬度的表示如图：
 */
```

```java
/*
两个方向切，纬度切 a 刀，经度切 b 刀，那么纬度切的话是数量是 a+1，经度切的话是 b*2 ，那么最终结果就是 b*2 * (a+1)。
但是需要考虑一种特殊情况，就是经度 b == 0 时，这时最终结果就为 a+1

注意：沿经度切分的时候要经过球心！所以是b*2
*/

public class bean2 {
    public static void main(String[] args) {
        Scanner in=new Scanner(System.in);
        String s1=in.nextLine();
        int weiducount=0;
        int jingducount=0;
        while (in.hasNext()){
            String ss=in.nextLine();
//            System.out.println(ss.charAt(0));
            if (ss.charAt(0)=='0'){
                weiducount++;//维度切割的次数
            }else if (ss.charAt(0)=='1'){
                jingducount++;//经度切割的次数
            }

        }

//        int ints1=Integer.parseInt(s1);//总的次数
        System.out.println((weiducount+1)*Math.max(jingducount*2,1));


    }
}
```





### 求不高兴度

```
/**
 * @author zhangbingbing
 * @version 1.0
 * @date 2021/4/4 11:08
 * 小美发明了一个函数：f(x)，表示将x的所有正整数因子提取出来之后从小到大排列，
再将数字拼接之后得到的数字串。例如：10的所有因子为1,2,5,10，
那么将这些因子从小到大排序之后再拼接得到的数字串为12510，即f(10)=12510。
 *
小美十分讨厌数字k，如果f(x)中含有某个子串对应的数字等于k，
那么她的不高兴度就会增加1。例如：f(8)=1248，那么其所有的子串为：1,2,4,8,12,24,48,124,248,1248，
只要k等于其中任意一个值，那么小美的不高兴度就会增加1。
对于每一个数，其不高兴度至多增加1。
 *
 * 现在，有一个长度为n的正整数序列，定义其不高兴度为序列中所有数的不高兴度的总和。
小美想要知道自己对于这个序列的总不高兴度为多少。
 */
```

```java
//方法1 直接把所有1-20000的数字都算一遍然后加起来就行。因为k<100000所以最大有5个数字，然后暴力匹配就行了
```

```java
//方法2： 


public class bean3 {
    public static void main(String[] args) {
        int count=0;
        Scanner in=new Scanner(System.in);
//        String s1=in.nextLine();
        String s1="5 13";
        String[] split1 = s1.split(" ");
        String hatestr=split1[1];
//        char n=s1.charAt(0);
//        int nnum=Integer.parseInt(String.valueOf(n));
//        String s2=in.nextLine();
        String s2="13 31 10 9 20";
        String[] split = s2.split(" ");
        for (int i=0;i<split.length;i++){
            int value=Integer.parseInt(split[i]);//读取数字
            //将数字进行分解
            StringBuilder stringBuilder=new StringBuilder();
            for (int j=1;j<=value;j++){
                if (value%j==0){
                    stringBuilder.append(j);//将因子进行组合
//                    value=value/j;
                }
            }
            String toString = stringBuilder.toString();//将组合后的因子输出为字符串
            boolean contains = toString.contains(hatestr);//判断字符串中是否有讨厌的字符串
            if (contains){
                count++;
            }

        }
        System.out.println(count);

    }
}
```



### 跳方格

```
/**
 * @author zhangbingbing
 * @version 1.0
 * @date 2021/4/4 11:16
小美在路上看到一些小学生在玩跳方格，她也想跟着一起玩。
这个方格被划分为n×n的小方格，即有n行n列。
每一个小方格上面都是一个1~k的正整数。小美想依次从1,2,…,k这个顺序来跳。
一开始小美可以站在任意一个小方格。
从一个方格跳到另一个方格的花费为两个方格的曼哈顿距离。
小美想知道是否可以依照该顺序一直跳到k，如果可以，最小的总花费是多少。

两个格子(x1,y1),(x2,y2)的曼哈顿距离为：|x1-x2|+|y1-y2|。
例如(3,4),(1,6)的曼哈顿距离为|3-1|+|4-6|=4。
 */
```



### 前缀和

```
给定两个数组，连续取这两个数组中的数字使得其和最大
例如：
a=[2, 9, -7, 6]
b=[2, 3, -7, 6]

当在a中取2，9，在b中取2，3时，和是最大的
```

```java
public class Main {
    public static void main(String[] args) {
        int[] a = {2, 9, -7, 6};
        int[] b = {2, 3, -7, 6};

        int[] t1 = new int[a.length+1];
        int[] t2=new int[b.length+1];

        int max1=0;
        int max2=0;
        for (int i=0;i<a.length;i++){
            t1[i+1]=t1[i]+a[i];
            max1=Math.max(max1,t1[i+1]);
        }
        for(int i=0;i<b.length;i++){
            t2[i+1]=t2[i]+b[i];
            max2=Math.max(max2,t2[i+1]);
        }
        System.out.println(max1+max2);
    }
}
```

#### [最长公共前缀](https://leetcode-cn.com/problems/longest-common-prefix/)


