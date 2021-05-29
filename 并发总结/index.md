# 并发总结


### 并发和并行的区别

[参考](https://www.zhihu.com/question/33515481)

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210328103335.png)

- 并发是两个队列**交替**使用一台咖啡机，并行是两个队列**同时**使用两台咖啡机
- 并发和并行都可以是很多个线程，就看这些线程能不能同时被（多个）cpu执行，如果可以就说明是并行，而并发是多个线程被（一个）cpu 轮流切换着执行。

### 线程和进程的区别

简单的理解：

- 进程是系统中的一个应用，进程不会相互影响。**进程是程序运行的基本单位**，系统运行一个程序就是一个进程从创建到灭亡的过程

- 一个进程中可以包含多个线程，一个线程开辟一个栈空间，假设有10个线程，就会开辟10个栈空间。但是不同线程之间是共享方法区和堆的。
  - 栈是线程私用的，生命周期和线程相同，栈描述的是Java方法执行的线程内存模型：每个方法执行的时候都会同步创建一个栈帧用于存储局部变量表/操作数栈，动态连接。方法出口等信息。
  - 堆是所有线程共享的区域。这在JVM启动时创建，此内存唯一的目的就是存放对象实例
  - 方法区是所有线程共享的区域，用于存储已被JVM加载的类型信息，常量，静态常量，即时编译后的代码缓存片段。

```
这里要引入一个概念：除了CPU以外所有的执行环境，主要是寄存器的一些内容，就构成了进程的上下文环境。进程的上下文是进程执行的环境。当这个程序执行完了，或者分配给他的CPU时间片用完了，那它就要被切换出去，等待下一次CPU的临幸。在被切换出去做的主要工作就是保存程序上下文，因为这个是下次他被CPU临幸的运行环境，必须保存
---


进程的颗粒度太大，每次的执行都要进行进程上下文的切换。如果我们把进程比喻为一个运行在电脑上的软件，那么一个软件的执行不可能是一条逻辑执行的，必定有多个分支和多个程序段，就好比要实现程序A，实际分成 a，b，c等多个块组合而成。那么这里具体的执行就可能变成：

程序A得到CPU -> CPU加载上下文，开始执行程序A的a小段，然后执行A的b小段，然后再执行A的c小段，最后CPU保存A的上下文。

这里a，b，c的执行是共享了A进程的上下文，CPU在执行的时候仅仅切换线程的上下文，而没有进行进程上下文切换的。进程的上下文切换的时间开销是远远大于线程上下文时间的开销。这样就让CPU的有效使用率得到提高。这里的a，b，c就是线程，也就是说线程是共享了进程的上下文环境的更为细小的CPU时间段。线程主要共享的是进程的地址空间。

作者：zhonyong
链接：https://www.zhihu.com/question/25532384/answer/81152571
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```



小结：**进程和线程都是一个时间段的描述，是CPU工作时间段的描述，不过是颗粒大小不同**。[参考](https://www.zhihu.com/question/25532384)



**进程的实现**：

为了实现进程模型，操作系统维护这一张表格，即进程表（PCB）。每个进程占用一个进程表项（或称为进程控制块）。该表项中包括了进程的状态的重要信息，包括程序计数器，堆栈指针，内存分配状况，所打开文件的状态，账号和调度信息，以及其他在进程由运行态转换到就绪态或阻塞态必须保存的信息，从而保证该进程随后能再次启动，就像从未被中断一样

注意：进程表是在操作系统中的（在内核态中）

**再一次明确**：**进程切换的效率是比较低的**

在切换进程时，首先用户态必须切换到内核态；然后保存当前进程的状态 ，包括在进程表中存储寄存器值以便以后重新加载。在许多系统中，内存映像也必须保存；接着，通过运行调度算法选定一个新进程；之后，应该将新进程的内存映像重新载入MMU（内存管理单元）中；最后，新进程开始运行。除此之外，进程切换还要使得整个内存高速缓存失效，强迫缓存从内存中动态重新载入两次（进入内核一次，出内核一次）。

进程是由内核管理和调度的，所以**进程的切换只能发生在内核态**。

所以，**进程的上下文切换不仅包含了虚拟内存、栈、全局变量等用户空间的资源**，**还包括了内核堆栈、寄存器等内核空间的资源。**

通常，会把交换的信息保存在进程的 PCB，当要运行另外一个进程的时候，我们需要从这个进程的 PCB 取出上下文，然后恢复到 CPU 中，这使得这个进程可以继续执行，如下图所示：

![进程上下文切换](https://mmbiz.qpic.cn/mmbiz_png/J0g14CUwaZcvw4t9kicec370n3cvX2JS9zkoWRzjcm7vsypa1ORR9N9GEEOTCdo3gPUULRuib0sZCYNgF3ibJh6YA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1&ynotemdtimestamp=1607004634118)

**为什么需要线程**：

- 主要原因：在一个进程中可能会同时发生多个活动。其中某一些活动随着时间的推移会被阻塞。通过将进程分解为多个线程，程序设计模式会变得简单
- 线程比进程更加的轻量化，所以线程比进程更容易创建和销毁。



**进程和线程的区别**：

Ⅰ 拥有资源

**进程是资源分配的基本单位**，但是线程不拥有资源，线程可以访问隶属进程的资源。

Ⅱ 调度

**线程是独立调度的基本单位**，在同一进程中，线程的切换不会引起进程切换，从一个进程中的线程切换到另一个进程中的线程时，会引起进程切换。

Ⅲ 系统开销

由于创建或撤销进程时，系统都要为之分配或回收资源，如内存空间、I/O  设备等，所付出的开销远大于创建或撤销线程时的开销。类似地，在进行进程切换时，涉及当前执行进程 CPU 环境的保存及新调度进程 CPU  环境的设置，而线程切换时只需保存和设置少量寄存器内容，开销很小。

Ⅳ 通信方面

线程间可以通过直接读写同一进程中的数据进行通信，但是进程通信需要借助 IPC。

`注意`：一个进程中的所有线程都有完全一样的地址空间，这意味着他们也共享同样的全局变量（共享公共内存）。除了共享内存地址外，所有线程还共享同一个打开的文件集，子进程、定时器以及相关信号等。

一个进程总是由某个用户拥有，该用户创建多个线程是为了他们之间的相互合作而不是竞争。

而不同进程可能由不同用户拥有，不同进程间可能存在敌对关系。



**线程的分类**：

线程可以分为用户级线程和内核级线程，其调度算法与可以是进程的调用算法中的一种。

两者间的差异在于性能。

1. 用户级线程的线程进行切换时只需要少量的机器指令，而内核级线程的线程进行切换时需要完整的上下文切换（修改内存映像，清理高速缓存等内容）
2. 用户级线程可以使用专门为应用程序定制的线程调度算法





**进程和程序的区别**：

举个例子：

有一位科学家在为他的女儿制作生日蛋糕。他有做蛋糕的食谱，厨房里有所需的原料。在这个比喻中，做蛋糕的食谱就是程序（即用适当形式描述的算法），科学家就是CPU，而做蛋糕的各种原料就是输入数据。进程就是科学家阅读食谱，取来各种原料以及烘制蛋糕等一系列动作的总和。

这里的关键思想是：**一个进程是某中类型的一个活动，它有程序、输入、输出以及状态**。



### 创建线程的方式

1. 继承Thread,重写run方法，在创建对象时直接`new`即可使用

   ```java
   public class ThreadTest01 {
       public static void main(String[] args) {
           Student student = new Student();
   //        start()方法的作用是：启动一个分支线程，在JVM中开辟一个新的栈空间，这段代码任务完成后，瞬间就结束了，使线程进入准备状态
   //        启动成功的线程会自动调用run()方法，并且run()方法在分支栈的底部(压栈)
   //        run()方法在分支栈的底部，main()方法在主栈的底部，run()方法和main()方法是平级的。
   //        如果直接调用run()方法，无法启动分支线程，所以必须写start()方法
           student.start();
           for (int i=0;i<500;i++){
               System.out.println("这是主线程"+i);
           }
   
       }
   
   }
   
   class Student extends Thread {//Thread实现Runnable接口
       @Override
       public void run() {
   //        编写程序，这段程序直接运行在分支线程中(分支栈中)
           for (int i = 0; i < 500; i++) {
               System.out.println("这是分支线程" + i);
           }
       }
   }
   ```

   

2. 实现接口Runnable，并重写run方法,在创建对象是直接`new`，此时创建出来的对象称为`可运行对象`，然后将可运行对象封装成一个线程对象。

   ```java
   public class ThreadTest02 {
       public static void main(String[] args) {
   //        创建一个可运行对象
           User u = new User();
   //        将可运行对象封装成一个线程对象
           Thread thread = new Thread(u);//Thread构造方法的参数类型为Runnable
   //        启动线程
           thread.start();
   
           for (int i = 0; i < 100; i++) {
               System.out.println("这是主线程" + i);
           }
       }
   }
   
   class User implements Runnable {
   
       @Override
       public void run() {
           for (int i = 0; i < 100; i++) {
               System.out.println("这是分支线程" + i);
           }
       }
   }
   ```

   ![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201130100422.png)

   小结：

   由于Java中不允许多继承，但是能够实现多个接口。如果要继承其他的类就要选择实现Runnable接口。

3. 通过实现callable接口，重写`call()`方法，因为在使用`Thread`创建对象时需要传入一个`Runnable`类型的对象，而callable接口实现类为`callable`类型，因此需要通过`FutureTask`进行包装为`Runnable`类型

   ```java
   public class CallableTest01 {
       public static void main(String[] args) {
           MyThread01 myThread01=new MyThread01();
           /*
           创建一个线程需要通过 new Thread('Runnable')
           但是Thread内的参数应该为'Runnable'类型(Thread构造方法的参数类型为Runnable)
           我们通过Callable创建的对象为Callable,怎么办呢？
           这时我们可以通过Runnable下的一个实现类: FutureTask,来解决这一问题。
           FutureTask传入的参数类型为Callable类型(通过构造函数来包装Callable)
           再将FutureTask的对象作为Thread的参数
           
           */
           FutureTask futureTask=new FutureTask(myThread01);
   
           Thread thread01=new Thread(futureTask);
           thread01.setName("t1");
           thread01.start();
           System.out.println(futureTask.get());//可以得到call()方法的返回值
   
       }
   }
   
   class MyThread01 implements Callable<Integer> {
       @Override
       public Integer call() throws Exception {
           System.out.println(Thread.currentThread().getName()+ " is running");
           return 1024;
       }
   }
   /*
   t1 is running
   1024
   */
   ```

   



### 线程的生命周期

线程的生命周期可以分为：新建，准备(ready)，运行(running)，阻塞(blocked)和终止

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201128091932.png)

注意：

```
处于running下的线程因为某些原因进行阻塞状态后，会释放掉抢占到的CPU时间片，并且不会直接进入ready，而需要等待引起阻塞的原因消除时才进入ready.进入ready还要重新抢CPU时间片，当抢到cpu时间片后会接着上次运行的代码继续运行，而不是重新运行
```

其中阻塞又可以细分为三种状态：

```
 
（一）、等待阻塞：运行的线程执行wait()方法，JVM会把该线程放入等待池中。(wait会释放持有的锁)。直到调用notify/notifyAll或wait时间到，线程会进入锁池。
（二）、同步阻塞：运行的线程在获取对象的同步锁时，若该同步锁被别的线程占用，则JVM会把该线程放入锁池（又称为同步队列）中。当锁池中的线程拿到对象的锁时会重新进入Ready转态
（三）、其他阻塞：运行的线程执行sleep()或join()方法，或者发出了I/O请求时，JVM会把该线程置为阻塞状态。当sleep()状态超时、join()等待线程终止或者超时、或者I/O处理完毕时，线程重新转入就绪状态。（注意,sleep是不会释放持有的锁） 

```

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201128091609.png)



#### 常见问题

为什么我们调用 start() 方法时会执行 run() 方法，为什么我们不能直接调用 run() 方法？

调用线程的start()方法会为使得线程进入ready状态，当线程抢到cpu时间片后会执行线程中的run方法

直接调用run()方法会将run（）方法视为main()线程下的一个普通方法





### 线程中的常用函数

#### 1.获取当前线程对象

`Thread.currentThread`，属于静态方法

#### 2.获取线程名字

`getName()`

#### 3.设置线程名字

`setName()`

```java

public class ThreadTest04 {
    public static void main(String[] args) {
//  获取当前线程对象，
//        这个代码出现在main方法中，所以当前线程就是主线程
        Thread currentthread=Thread.currentThread();
        System.out.println(currentthread.getName());


        MyThread01 myThread01=new MyThread01();
        Thread thread01=new Thread(myThread01);

//          设置线程名字
        thread01.setName("beijing");
//        获取线程名字
        System.out.println("分支线程名字为："+thread01.getName());

        Thread thread02=new Thread(myThread01);
        System.out.println(thread02.getName());
//        thread01.start();

    }
}

class MyThread01 implements Runnable{

    @Override
    public void run() {
        System.out.println("这是分支线程");
    }
}

/*
main
分支线程名字为：beijing
Thread-1
*/
```

#### 4.线程睡眠方法

`Thread.sleep(long millis)`方法，使当前线程转到阻塞状态。millis参数设定睡眠的时间，以毫秒为单位。当睡眠结束后，就转为准备（ready）状态,需要重新抢夺CPU时间片。

```java
public class ThreadTest06 {
    public static void main(String[] args) throws InterruptedException {
        Mythread02 mythread02=new Mythread02();
        Thread thread=new Thread(mythread02);

//        thread.setName("t");

        thread.start();

        Thread.sleep(1000*5);//让当前线程进入睡眠，也就是main进入睡眠
        System.out.println(Thread.currentThread().getName());
        System.out.println("主线程");
    }
}

class Mythread02 implements Runnable{
    @Override
    public void run() {
//        让子线程进入睡眠
        try {
            Thread.sleep(1000*5);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        for (int i=0;i<10;i++){
            System.out.println(Thread.currentThread().getName()+"--->"+i);
        }
    }
}
```

#### 5.中断线程睡眠的方法

`interrupt()`

```java
public class ThreadTest07 {
    public static void main(String[] args) throws InterruptedException {
        Thread thread=new Thread(new MyThread03());
//        thread.setName("t");
        thread.start();

    //主线程睡眠5秒，但是分支线程还在执行，5秒后执行thread.interrupt();分支线程中断睡眠
        Thread.sleep(1000*5);

//        中断thread线程的睡眠，这种中断睡眠的方式依靠的是异常处理机制
        thread.interrupt();


    }

}

class MyThread03 implements Runnable{
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName()+"  start");
        try {
            Thread.sleep(1000*60);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(Thread.currentThread().getName()+"  end");
    }
}
/*
Thread-0  start
java.lang.InterruptedException: sleep interrupted
	at java.base/java.lang.Thread.sleep(Native Method)
	at ThreadTest.MyThread03.run(ThreadTest07.java:37)
	at java.base/java.lang.Thread.run(Thread.java:832)
Thread-0  end
*/
```

#### 6.如何合理的终止线程

`通过标记进行终止`

```java

public class ThreadTest08 {
    public static void main(String[] args) throws InterruptedException {
        MyThread04 myThread04=new MyThread04();
        Thread thread=new Thread(myThread04);
        thread.start();

//        主线程睡眠5秒，但是分支线程还在执行,5秒后执行myThread04.run=false;此时分支线程结束
        Thread.sleep(1000*5);

        myThread04.run=false;
    }
}


class MyThread04 implements Runnable{
    boolean run=true;
    @Override
    public void run() {
        for (int i=0;i<100;i++){
            if (run){
//                如果run=true，每隔一秒输出一次
                System.out.println(Thread.currentThread().getName()+i);
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            else {
//                可以在return前将需要保存的数据进行保存
                return;//return表示方法结束
            }
        }
    }
}
/*
Thread-00
Thread-01
Thread-02
Thread-03
Thread-04

Process finished with exit code 0
*/
```

#### 7.线程让位

`yield`，属于静态方法

使得处于running的线程主动放弃占用的CPU时间片，回到ready状态，以允许具有相同优先级的其他线程获得运行机会，因此，使用yield()的目的是让相同优先级的线程之间能适当的轮转执行。但是，**实际中无法保证yield()达到让步目的**，因为让步的线程还有可能被线程调度程序再次选中。 

```java
public class ThreadTest09 {
    public static void main(String[] args) throws InterruptedException {
        Mythread009 mythread009=new Mythread009();
        Thread thread1=new Thread(mythread009);
        thread1.start();

         Thread.yield();//属于静态方法，使得running的线程让出cpu时间片

//        System.out.println("主线程开始");
        for (int i=0;i<3;i++){
            System.out.println(Thread.currentThread().getName()+i);
        }
//        System.out.println("主线程结束");

    }
}

class Mythread009 implements Runnable{
    @Override
    public void run() {
        for (int i=0;i<3;i++){
            System.out.println(Thread.currentThread().getName()+i);
        }
    }
}
```





#### 8.线程合并

`join`

在当前线程中调用另一个线程的join()方法，则当前线程转入阻塞状态，直到另一个进程运行结束，当前线程再由阻塞转为就绪状态。

```java
public class ThreadTest09 {
    public static void main(String[] args) throws InterruptedException {
        Mythread009 mythread009=new Mythread009();
        Thread thread1=new Thread(mythread009);
        thread1.start();
/*
* join()方法会将thread1合并到主线程中，直到thread1线程结束才开始执行主线程，两个栈之间发生了等待
* */
        thread1.join();

//        System.out.println("主线程开始");
        for (int i=0;i<3;i++){
            System.out.println(Thread.currentThread().getName()+i);
        }
//        System.out.println("主线程结束");

    }
}

class Mythread009 implements Runnable{
    @Override
    public void run() {
        for (int i=0;i<3;i++){
            System.out.println(Thread.currentThread().getName()+i);
        }
    }
}
/*
Thread-01
Thread-02
main0
main1
main2
*/
```



#### 9.线程等待和线程唤醒

线程等待：Object类中的`wait()`方法，导致当前的线程等待; 线程唤醒：Object类中的`notify()`方法或`notifyAll()`

`wait()`和`notify`必须在`synchronized`语句块中使用，也就是说`wait()`和`notify`是针对获取对象锁的操作。

- 其中`wait()`就是让已获得对象锁的线程主动释放锁，进入等待队列。

- `notify`就是随机选择选择一个在这个对象上调用了`wait()`方法的线程，使其进入锁池。（即在等待队列中随机挑一个线程进入锁池）
- `notifyAll`就是将等待队列中的所有线程放入锁池



注意：

1. 如果一个线程直接抢到了CPU时间片和对象锁就直接执行了，不会进入锁池。但是当这个对象调用了`wait()`方法，这个线程就会主动放弃对象锁和cpu时间片，进入等待队列。

2. 在锁池中线程的特点：抢到了cpu时间片，但是没有对象锁，所以进入锁池等待获取对象锁

3. 在锁池中的线程只有当抢到对象锁后才能够进入ready状态，重新抢占CPU时间片
4. `notify`只是将等待队列中的线程放入锁池中，并不会使得当前拥有对象锁的线程马上释放对象锁，只有当synchronized语句块中的代码执行结束才会释放锁。释放锁后，锁池中的线程会抢占对象锁，只有当抢到对象锁后才能够进入ready状态，重新抢占CPU时间片。这个时候抢到了对象锁的这个线程，就算一开始没有抢到CPU时间片而是其他的线程抢到了，但是其他的线程没有对象锁，也只能释放掉CPU时间片进入锁池等待。

例子：生产者，消费者模式：

```java
package ThreadTest;

import javax.print.attribute.standard.PrinterURI;
import java.util.ArrayList;
import java.util.List;

/**
 * ClassName:    ThreadTest11
 * Package:    ThreadTest
 * Description:
 * Datetime:    2020/10/18   下午4:21
 * Author:   shilongshen
 */
/*
 * 使用wait和notify方法实现生产者和消费者模式
 *“生产者和消费者模式”： 生成线程负责生产，消费线程负责消费，两者要保持均衡
 *
 * 模拟这样一个需求：
 *       用list集合表示一个仓库
 *       list集合中最多只能存一个元素
 *       一个元素表示仓库满了。0个元素表示仓库空了
 *       必须做到生产一个消费一个
 *
 * */
public class ThreadTest11 {
    public static void main(String[] args) {
//        创建一个仓库，共享的
        List list = new ArrayList();
//        创建生产者线程
        Thread t1 = new Thread(new Producer(list));
//        创建消费者线程
        Thread t2 = new Thread(new Consumer(list));

        t1.setName("生产者线程");
        t2.setName("消费者线程");

        t1.start();
        t2.start();


    }
}


//生产线程
class Producer implements Runnable {
    private List list;

    public Producer(List list) {
        this.list = list;
    }

    @Override
    public void run() {
//生产者一直进行生产，使用死循环进行模拟
        while (true) {
            synchronized (list) {//首先获取对象锁
                if (list.size() > 0) {//如果list中元素对象大于0(等于1),调用wait()方法，
                    // 使得当前线程进入等待状态，并释放获得的锁，释放掉锁之后，消费者线程进行消费
                    try {
                        list.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
                //如果list中的元素等于0,往list中添加元素
                Boolean is=list.add("String");//消费者进入等待状态，释放锁，说明，list集合元素为0,
                // 此时会执行此处代码(而不会执行if语句内的代码)，将list集合中的元素进行增加
                System.out.println(Thread.currentThread().getName() + "--->" + "String");
                list.notify();//唤醒等待的消费者进程

            }
        }
    }
}

//消费线程
class Consumer implements Runnable {
    private List list;

    public Consumer(List list) {
        this.list = list;
    }

    @Override
    public void run() {
//消费者模式一直进行消费
        while (true) {
            synchronized (list) {
                if (list.size() == 0) {//如果list中元素对象等于0,调用wait()方法，
                    // 使得当前线程进入等待状态，并释放获得的锁,释放掉锁之后，生产者线程进行生产
                    try {
                        list.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }

                Object o = list.remove(0);//生产者进入等待状态，释放锁，说明，list集合元素为1,
                // 此时会执行此处代码(而不会执行if语句内的代码)，将list集合中的元素进行删除
                System.out.println(Thread.currentThread().getName() + "--->" + o);
                list.notify();//notify()为唤醒生产者线程(唤醒list对象上等待的线程)
            }
        }
    }
}
```

逻辑：

```
假设最开始是生产者线程抢到了对象锁和CPU时间片，此时list中元素为0，所以会添加一个元素，然后执行notify，尽管此时等待队列中没有线程，run方法结束，生产者线程释放cpu时间片和对象锁。
因为是一个循环语句所以此时处于ready的有生产者线程和消费者线程
1.如果还是生产者线程抢到cpu时间片和对象锁，由于list元素大于1,会调用list.wait()使得生产者线程释放锁，进入等待池。然后	必定是消费者线程执行，此时list元素不为0,所以会删除元素，然后调用list.notify()唤醒等待池中的生产者线程，使其进入锁池，因为锁池中只有生产者线程一个线程，所以生产者线程必定拿到对象锁进入ready。
	1.1. 此时ready的有生产者线程和消费者线程，
		1.1.1.如果是消费者拿到CPU时间片，但是由于没有对象锁，所以会释放CPU时间片进入锁池
		1.1.2 所以必定是这一次必定是生产者线程进行生产，此时，所以会添加一个元素，然后执行notify，尽管此时等待队列中没有线程， run方法结束，生产者线程释放cpu时间片和对象锁。
```





### 线程同步

#### 什么时候存在线程安全问题

当满足以下条件（竞态条件）：

- 多线程并发
- 有共享数据的行为
- 共享数据有修改数据的行为

多线程可能会导致共享数据被破坏，引起线程安全问题。



#### 如何解决线程安全问题

用排队执行的机制来解决线程安全问题，这种机制称为线程同步机制。

如何实现线程同步？使用

### synchronized关键字

1.首先明确：

- 每个对象都有一把锁，用来保护代码片段
- 锁可以管理试图进入被保护代码段的线程（如wait,notify等方法）

**当一个线程试图访问同步代码块时，它首先必须得到锁，退出或抛出异常时必须释放锁。**



2. synchronized关键字的作用域

- 某个对象实例（获取的是对象锁）

`synchronized  aMethod(){}`可以防止多个线程同时访问这个对象的synchronized方法（如果一个对象有多个synchronized方法，只要一个线程访问了其中的一个synchronized方法，其它线程不能同时访问这个对象中任何一个synchronized方法）。这时，不同的对象实例的synchronized方法是不相干扰的。也就是说，其它线程照样可以同时访问相同类的另一个对象实例中的synchronized方法

- 某个类（获取的是类锁）

  `synchronized static aStaticMethod{}`防止多个线程同时访问这个类中的synchronized static 方法。它可以<u>对类的所有对象实例起作用</u>。 

  ```java
  //将synchronized作用静态方法
  //或作用与类名称字面常量
  Class Foo{
  public synchronized static void methodAAA()   // 同步的static 函数
   {
  //….
   }
  public void methodBBB()
   {
         synchronized(Foo.class)   //  class literal(类名称字面常量)
   }
  }
  ```
  

  


3. synchronized可以通过调用**同步方法**来获得锁

   即如果一个方法声明时有synchronized关键字，那么对象的锁将保护整个方法。也就是说，要调用这个方法必须获得对象的锁

   ```java
   public synchronized void methed(){
       method body
   }
   ```

   

4. synchronized关键字还可以用于方法中的某个区块中（即**同步语句块**），表示只对这个区块的资源实行互斥访问。用法是: `synchronized(this){/*区块*/}`，它的作用域是当前对象；

   ```java
   public synchronized void methed(){
       synchronized（this）{
           
       }
   }
   ```

   

5. synchronized关键字是不能继承的，也就是说，基类的方法`synchronized f(){}` 在继承类中并不自动是`synchronized f(){}`，而是变成了`f(){}`。继承类需要你显式的指定它的某个方法为synchronized方法； 

小结：

- synchronized关键字可以作为函数的修饰符，也可作为函数内的语句的修饰符，也就是平时说的**同步方法**和**同步语句块**
- 如果再细的分类，synchronized可作用于instance变量、object reference（对象引用）、static函数和class literals(类名称字面常量)身上。
- 无论synchronized关键字加在方法上还是对象上，**它取得的锁都是对象**，而不是把一段代码或函数当作锁
- 每个对象**只有一个锁**（lock）与之相关联

明确：

- 同一个类中new出来的不同对象的锁是不同的，即`每个对象只有一把锁`

假设现在有一个类,new出来了两个对象p1,p2，这两个的对象锁是不一样的。但是`类锁是唯一的`



#### synchronized底层原理

[参考](https://blog.csdn.net/liupeifeng3514/article/details/79111565)

在HotSpot虚拟机中，对象在内存中存储的布局可以分为3块区域：对象头(Header)，实例数据( Instance Data)和对齐填充(Padding)。

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210408155223.png)

对象头：

第一部分用于存储对象自身的运行时数据，如哈希码（HashCode）、GC 分代年龄、锁状态标志、线程持有的锁、偏向线程ID、偏向时间戳、对象分代年龄，这部分信息称为“Mark Word”；Mark Word被设计成一个**非固定**的数据结构以便在极小的空间内存储尽量多的信息，它会根据自己的状态复用自己的存储空间；

第二部分是类型指针，即对象指向它的类元数据的指针，**虚拟机通过这个指针来确定这个对象是哪个类的实例；**
如果对象是一个 Java 数组，那在对象头中还必须有一块用于记录数组长度的数据。因为虚拟机可以通过普通 Java 对象的元数据信息确定Java 对象的大小，但是从数组的元数据中无法确定数组的大小。这部分数据的长度在 32 位和 64 位的虚拟机（未开启压缩指针）中分别为32bit 和 64bit。

实例数据：

实例数据部分是对象真正存储的有效信息，也既是我们在程序代码里面所定义的**各种类型的字段内容**，无论是从父类继承下来的，还是在子类中定义的都需要记录下来。

对齐填充：

对齐填充并不是必然存在的，也没有特别的含义，它**仅仅起着占位符的作用**。由于HotSpot VM的自动内存管理系统要求对象起始地址必须是8字节的整数倍，换句话说就是对象的大小必须是8字节的整数倍。对象头正好是8字节的倍数（1倍或者2倍），因此当对象实例数据部分没有对齐的话，就需要通过对齐填充来补全。





重点介绍**mark word**   

[参考](https://blog.csdn.net/javazejian/article/details/72828483)

其中Mark Word在默认情况下存储着对象的HashCode、分代年龄、锁标记位等，以下是32位JVM的Mark Word默认存储结构（当对象未被同步锁锁定的状态下）

| 锁状态   | 25bit          | 4bit             | 1bit是否偏向锁 | 2bit锁标志位 |
| -------- | -------------- | ---------------- | -------------- | ------------ |
| 无锁状态 | 对象的hashcode | 对象的GC分代年龄 | 0              | 01           |



由于对象头的信息是与对象自身定义的数据没有关系的额外存储成本，因此考虑到JVM的空间效率，Mark Word 被设计成为一个非固定的数据结构，以便存储更多有效的数据，它会根据对象本身的状态复用自己的存储空间，如32位JVM下，除了上述列出的Mark Word默认存储结构外，还有如下可能变化的结构：

<img src="https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210419161741.png" style="zoom:80%;" />

这里我们主要分析一下**重量级锁**也就是通常说**synchronized**的对象锁，锁标识位为**10**

其中指针指向的是monitor对象（也称为管程或监视器锁）的起始地址。

> <u>每个对象都存在着一个 monitor 与之关联</u>，对象与其 monitor 之间的关系有存在多种实现方式，
>
> 如monitor可以与对象一起创建销毁或当线程试图获取对象锁时自动生成，但当一个 monitor 被某个线程持有后，它便处于锁定状态。

**monitor对象存在于每个Java对象的对象头中(存储的指针的指向)，synchronized锁便是通过这种方式获取锁的，也是为什么Java中任意对象可以作为锁的原因。**



##### synchronized代码块底层原理

定义一个synchronized修饰的同步代码块，在代码块中操作共享变量i，如下

```java
public class SyncCodeBlock {

   public int i;

   public void syncTask(){
       //同步代码库
       synchronized (this){
           i++;
       }
   }
}
```

编译上述代码并使用javap反编译后得到字节码如下(这里我们省略一部分没有必要的信息)：

```java
Classfile /Users/zejian/Downloads/Java8_Action/src/main/java/com/zejian/concurrencys/SyncCodeBlock.class
  Last modified 2017-6-2; size 426 bytes
  MD5 checksum c80bc322c87b312de760942820b4fed5
  Compiled from "SyncCodeBlock.java"
public class com.zejian.concurrencys.SyncCodeBlock
  minor version: 0
  major version: 52
  flags: ACC_PUBLIC, ACC_SUPER
Constant pool:
  //........省略常量池中数据
  //构造函数
  public com.zejian.concurrencys.SyncCodeBlock();
    descriptor: ()V
    flags: ACC_PUBLIC
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: return
      LineNumberTable:
        line 7: 0
  //===========主要看看syncTask方法实现================
  public void syncTask();
    descriptor: ()V
    flags: ACC_PUBLIC
    Code:
      stack=3, locals=3, args_size=1
         0: aload_0
         1: dup
         2: astore_1
         3: monitorenter  //注意此处，进入同步方法
         4: aload_0
         5: dup
         6: getfield      #2             // Field i:I
         9: iconst_1
        10: iadd
        11: putfield      #2            // Field i:I
        14: aload_1
        15: monitorexit   //注意此处，退出同步方法
        16: goto          24
        19: astore_2
        20: aload_1
        21: monitorexit //注意此处，退出同步方法
        22: aload_2
        23: athrow
        24: return
      Exception table:
      //省略其他字节码.......
}
SourceFile: "SyncCodeBlock.java"
```

我们主要关注字节码中的如下代码

```java
3: monitorenter  //进入同步方法
//..........省略其他  
15: monitorexit   //退出同步方法
16: goto          24
//省略其他.......
21: monitorexit //退出同步方法
```

可分析得到：

- 同步代码块的实现是使用`monitorentry`和`monitorexit`

- `monitorenter`指令指向同步代码块的开始位置，`monitorexit`指令则指明同步代码块的结束位置

- 当执行`monitorenter`指令时，当前线程将试图获取 objectref(即对象锁) 所对应的 monitor 的持有权，当 objectref 的 monitor 的计数器为 0，那线程可以成功取得 monitor，并将计数器值设置为 1，取锁成功。
- 如果当前线程已经拥有 objectref 的 monitor 的持有权，那它可以重入这个 monitor (关于重入性稍后会分析)，重入时计数器的值也会加 1
- 倘若其他线程已经拥有 objectref 的 monitor 的所有权，那当前线程将被阻塞，直到正在执行线程执行完毕，即`monitorexit`指令被执行，执行线程将释放 monitor(锁)并设置计数器值为0 ，其他线程将有机会持有 monitor 。
- 编译器将会确保无论方法通过何种方式完成，方法中调用过的每条 `monitorenter` 指令都有执行其对应` monitorexit `指令

##### synchronized方法底层原理

方法级的同步是隐式，即无需通过字节码指令来控制的，它实现在方法调用和返回操作之中

JVM可以从方法常量池中的方法表结构(method_info Structure) 中的 `ACC_SYNCHRONIZED` 访问标志区分一个方法是否同步方法

- 当方法调用时，调用指令将会检查方法的 `ACC_SYNCHRONIZED `访问标志是否被设置，如果设置了，执行线程将先持有monitor（虚拟机规范中用的是管程一词）， 然后再执行方法，最后在方法完成(无论是正常完成还是非正常完成)时释放monitor）。

- 在方法执行期间，执行线程持有了monitor，其他任何线程都无法再获得同一个monitor

- 如果一个同步方法执行期间抛 出了异常，并且在方法内部无法处理此异常，那这个同步方法所持有的monitor将在异常抛到同步方法之外时自动释放。

下面我们看看字节码层面如何实现：

```java
public class SyncMethod {

   public int i;

   public synchronized void syncTask(){
           i++;
   }
}
```

使用javap反编译后的字节码如下：

```java
Classfile /Users/zejian/Downloads/Java8_Action/src/main/java/com/zejian/concurrencys/SyncMethod.class
  Last modified 2017-6-2; size 308 bytes
  MD5 checksum f34075a8c059ea65e4cc2fa610e0cd94
  Compiled from "SyncMethod.java"
public class com.zejian.concurrencys.SyncMethod
  minor version: 0
  major version: 52
  flags: ACC_PUBLIC, ACC_SUPER
Constant pool;

   //省略没必要的字节码
  //==================syncTask方法======================
  public synchronized void syncTask();
    descriptor: ()V
    //方法标识ACC_PUBLIC代表public修饰，ACC_SYNCHRONIZED指明该方法为同步方法
    flags: ACC_PUBLIC, ACC_SYNCHRONIZED
    Code:
      stack=3, locals=1, args_size=1
         0: aload_0
         1: dup
         2: getfield      #2                  // Field i:I
         5: iconst_1
         6: iadd
         7: putfield      #2                  // Field i:I
        10: return
      LineNumberTable:
        line 12: 0
        line 13: 10
}
SourceFile: "SyncMethod.java"
```

从字节码中可以看出，synchronized修饰的方法并没有monitorenter指令和monitorexit指令，取得代之的确实是`ACC_SYNCHRONIZED`标识，该标识指明了该方法是一个同步方法，JVM通过该ACC_SYNCHRONIZED访问标志来辨别一个方法是否声明为同步方法，从而执行相应的同步调用。



小结：

- 每一个对象的对象头中存在一个`monitor`，`synchronized`通过`monitor`来实现加锁操作。
- 同步代码块时通过`monitorentry`和`monitorexit`来实现加锁操作，具体来说`monitorentry`表示进入同步代码块，`monitorexit`表示退出同步代码块
- 同步方法通过`ACC_SYNCHRONIZED`来实现加锁，具体来说当方法调用时会先检查`ACC_SYNCHRONIZED`是否被标记，如果被标记了，执行线程先拿到`monitor`再执行方法，最后释放`monitor`。



#### synchronized的可重入性

从互斥锁的设计上来说，当一个线程试图操作一个由其他线程持有的对象锁的临界资源时，将会处于阻塞状态，但**当一个线程再次请求自己持有对象锁的共享资源时，这种情况属于重入锁，请求将会成功**，在java中synchronized是基于原子性的内部锁机制，是可重入的，<u>因此在一个线程调用synchronized方法的同时在其方法体内部调用该对象另一个synchronized方法，也就是说**一个线程得到一个对象锁后再次请求该对象锁，是允许的**，这就是synchronized的可重入性</u>。如下：


```java
public class AccountingSync implements Runnable{
    static AccountingSync instance=new AccountingSync();
    static int i=0;
    static int j=0;
    @Override
    public void run() {
        for(int j=0;j<1000000;j++){

            //this,当前实例对象锁
            synchronized(this){
                i++;
                increase();//synchronized的可重入性
            }
        }
    }

    public synchronized void increase(){
        j++;
    }


    public static void main(String[] args) throws InterruptedException {
        Thread t1=new Thread(instance);
        Thread t2=new Thread(instance);
        t1.start();t2.start();
        t1.join();t2.join();
        System.out.println(i);
    }
}
————————————————
版权声明：本文为CSDN博主「zejian_」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/javazejian/article/details/72828483
```

正如代码所演示的<u>，在获取当前实例对象锁后进入synchronized代码块执行同步代码，并在代码块中调用了当前实例对象的另外一个synchronized方法，再次请求当前实例锁时，将被允许，进而执行方法体代码</u>，这就是重入锁最直接的体现，需要特别注意另外一种情况，当子类继承父类时，子类也是可以通过可重入锁调用父类的同步方法。注意由于synchronized是基于monitor实现的，因此每次重入，monitor中的计数器仍会加1。





### 死锁

​	多个线程同时被阻塞，其中一个或全部线程等待某个资源的释放。由于线程被无限期的阻塞，因此程序不可能会正常终止



多出现在`synchronized`嵌套使用的情况中，例如：

![https://camo.githubusercontent.com/6196c5b96aa34a94e788d6911eb12bb16e4751036b230c2371a5bb8c9fd508de/68747470733a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f323031392d342f323031392d34254536254144254242254539253934253831312e706e67](https://camo.githubusercontent.com/6196c5b96aa34a94e788d6911eb12bb16e4751036b230c2371a5bb8c9fd508de/68747470733a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f323031392d342f323031392d34254536254144254242254539253934253831312e706e67)

线程A想要获得资源1和资源2的锁，同时线程2也想要获得资源1和资源2的锁。当A抢到资源1的锁，B抢到资源2的锁时，A线程会等待资源2的锁，B线程会等待资源1的锁，此时就会进入一个无限期的等待。

```java
package ThreadDeadLockTest01;

/**
 * ClassName:    DeadLockTest01
 * Package:    ThreadDeadLockTest01
 * Description:
 * Datetime:    2020/10/17   下午8:24
 * Author:   shilongshen
 */
/*
* 死锁
*怎么写死锁:
* 当t1线程锁住o1的锁时，由于t2线程已经锁住了o2对象的锁，所以t1线程永远不会结束，同理t2线程也不会结束。
*
* --->synchronized在开发中最好不要嵌套使用
* */
public class DeadLockTest01 {
    public static void main(String[] args) {
        Object o1=new Object();
        Object o2=new Object();
        //t1,t2线程共享o1,o2
        Thread t1=new Thread(new Mythread1(o1,o2));
        Thread t2=new Thread(new Mythread2(o1,o2));

        t1.setName("t1");
        t2.setName("t2");

        t1.start();
        t2.start();

    }
}

class Mythread1 implements Runnable{
    Object o1;
    Object o2;
    public Mythread1(Object o1,Object o2){
        this.o1=o1;
        this.o2=o2;
    }
    @Override
    public void run() {
        synchronized(o1){
            System.out.println("t1 -->o1");
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            synchronized (o2){
                System.out.println("t1-->o2");
            }
        }
    }
}


class Mythread2 implements Runnable{
    Object o1;
    Object o2;
    public Mythread2(Object o1,Object o2){
        this.o1=o1;
        this.o2=o2;
    }
    @Override
    public void run() {
        synchronized(o2){
            System.out.println("t2-->o2");
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            synchronized (o1){
                System.out.println("t2--->o1");
            }
        }
    }
}
```

#### 解决方法

发生死锁的四个条件即对应解决方式

- 互斥：每一个资源只能够被一个线程占用
- 被分配的资源不能够被强行抢夺               -->手动抢占资源
- 线程获取一个资源后还可以等待获取其他资源         ->提前申请所有需要的资源，如果申请不到就不执行
- 环路条件：至少有两个线程在相互等待对方所占用的资源   ->按照顺序申请资源

### 锁的分类

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210328152254.png)

锁的状态总共有四种，无锁状态、偏向锁、轻量级锁和重量级锁(synchronized)。随着锁的竞争，锁可以从偏向锁升级到轻量级锁，再升级的重量级锁，但是锁的升级是单向的，也就是说只能从低到高升级，不会出现锁的降级

明确：操作系统实现线程之间的切换时需要从用户态转换到核心态，这个状态之间的转换需要相对比较长的时间，时间成本相对较高。

[参考](https://blog.csdn.net/javazejian/article/details/72828483)

#### 偏向锁

偏向锁是Java 6之后加入的新锁，它是一种针对加锁操作的优化手段，经过研究发现，在大多数情况下，锁不仅不存在多线程竞争，而且总是由同一线程多次获得，因此为了减少同一线程获取锁(会涉及到一些CAS操作,耗时)的代价而引入偏向锁。**偏向锁的核心思想是，如果一个线程获得了锁，那么锁就进入偏向模式，此时Mark Word 的结构也变为偏向锁结构，当这个线程再次请求锁时，无需再做任何同步操作，即获取锁的过程，这样就省去了大量有关锁申请的操作，从而也就提升程序的性能。**所以，对于没有锁竞争的场合，偏向锁有很好的优化效果，毕竟极有可能连续多次是同一个线程申请相同的锁。但是对于锁竞争比较激烈的场合，偏向锁就失效了，因为这样场合极有可能每次申请锁的线程都是不相同的，因此这种场合下不应该使用偏向锁，否则会得不偿失，需要注意的是，偏向锁失败后，并不会立即膨胀为重量级锁，而是先升级为轻量级锁。下面我们接着了解轻量级锁。

- 前提：在大多数情况下，一个锁总是由同一个线程多次获得
- 核心思想：如果一个线程获得了锁，那么当这个线程再次请求锁的时候不需要做任何同步操作

#### 轻量级锁

倘若偏向锁失败，虚拟机并不会立即升级为重量级锁，它还会尝试使用一种称为轻量级锁的优化手段(1.6之后加入的)，此时Mark Word 的结构也变为轻量级锁的结构。轻量级锁能够提升程序性能的依据是“**对绝大部分的锁，在整个同步周期内都不存在竞争”**，注意这是经验数据。需要了解的是，轻量级锁所适应的场景是线程交替执行同步块的场合，如果存在同一时间访问同一锁的场合，就会导致轻量级锁膨胀为重量级锁。

> **加锁**
>
> - 线程在执行同步块之前，如果此同步对象没有被锁定（锁标志位为“01”状态，JVM会先在当前线程的栈帧中创建用于存储锁记录（下图的LockRecord）的空间，并将对象头中的MarkWord复制到锁记录中，官方称为 Displaced Mark Word
> - 然后，虚拟机将使用CAS操作，将对象的MarkWord更新为指向(Lock Record)锁记录的指针
>
> 
>
> - 如果这个操作成功，那么该线程就有了该对象的锁，并且对象的MW的锁标志位置为 “00”，表示该对象处于轻量级锁定状态
> - 如果更新失败，表示其他线程竞争锁，当前线程尝试使用自旋来获取锁
>
> **解锁**
>
> - 使用CAS操作将 Displaced Mark Word 替换回到对象头
>   - 如果成功，则说明没有发生竞争
>   - 失败，则表示当前锁存在竞争，锁就会膨胀成重量级锁
>     - 释放锁，并且唤醒等待的线程
>
> 
>
> 轻量级能提升程序同步性能的依据是"对于绝大部分的锁，**在整个同步周期内都是不存在竞争的**，这是一个经验数据。
>
> - 如果没有竞争，轻量级锁使用CAS操作，避免使用互斥量
> - 如果存在竞争，除了互斥量的开销，还有 CAS的操作，不仅没有提升，反而性能会下降
>
> ------------------------------------------------
> 版权声明：本文为CSDN博主「别惹猪儿虫」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
> 原文链接：https://blog.csdn.net/qq_35583772/article/details/94544010

#### 自旋锁

轻量级锁失败后，虚拟机为了避免线程真实地在操作系统层面挂起，还会进行一项称为自旋锁的优化手段。**这是基于在大多数情况下，线程持有锁的时间都不会太长，如果直接挂起操作系统层面的线程可能会得不偿失**，毕竟操作系统实现线程之间的切换时需要从用户态转换到核心态，这个状态之间的转换需要相对比较长的时间，时间成本相对较高，**因此自旋锁会假设在不久将来，当前的线程可以获得锁，因此虚拟机会让当前想要获取锁的线程做几个空循环(这也是称为自旋的原因)**，一般不会太久，可能是50个循环或100循环，在经过若干次循环后，如果得到锁，就顺利进入临界区。如果还不能获得锁，那就会将线程在操作系统层面挂起，这就是自旋锁的优化方式，这种方式确实也是可以提升效率的。最后没办法也就只能升级为重量级锁了。

- 核心思想：如果一个线程想要获得锁，但是这个锁已经被其他线程获得了，那么这个线程不会立即阻塞，而是会自旋一段时间进行等待。
- 自旋等待本身避免了线程切换的开销，但是如果锁被占用的时间很长，那么自旋等待自会白白耗费资源。用户可以使用`-XX:PreBlockSpin`来更改自旋的次数。

---



### CAS原理

[参考](https://blog.csdn.net/javazejian/article/details/72772470)

> 无锁则总是假设对共享资源的访问没有冲突，线程可以不停执行，无需**加锁**，无需等待，一旦发现冲突，无锁策略则采用一种称为CAS的技术来保证线程执行的安全性
>
> 乐观锁（CAS）：去拿数据的时候都认为别人不会修改，所以不会上锁，在更新的时候会判断一下在此期间别人有没有去更新这个数据，**采取在写时先读出当前版本version号**。
>
> 

CAS的全称是Compare And Swap 即比较交换，其算法核心思想如下

> 执行函数：CAS(V , E , N)
>
> 其包含3个参数
>
> - V表示要更新的变量  -->value
> - E表示预期值    -->expect value 
> - N表示新值    -->new  value

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210420152754.png)

- 如果V值等于E值，则将V的值设为N
- 若V值和E值不同，则说明已经有其他线程做了更新，则当前线程什么都不做

通俗的理解:

- CAS操作需要我们提供一个期望值，当期望值与当前线程的变量值相同时，说明还没线程修改该值，当前线程可以进行修改，也就是执行CAS操作

- 但如果期望值与当前线程不符，则说明该值已被其他线程修改，此时不执行更新操作，但可以选择重新读取该变量再尝试再次修改该变量，也可以放弃操作



基于这样的原理，CAS操作即使没有锁，同样知道其他线程对共享资源操作影响，并执行相应的处理措施。同时从这点也可以看出，由于无锁操作中没有锁的存在，因此不可能出现死锁的情况，也就是说**无锁操作天生免疫死锁**。

> **或许我们可能会有这样的疑问，假设存在多个线程执行CAS操作并且CAS的步骤很多，有没有可能在判断V和E相同后，正要赋值时，切换了线程，更改了值。造成了数据不一致呢？答案是否定的，因为CAS是一种系统原语，原语属于操作系统用语范畴，是由若干条指令组成的，用于完成某个功能的一个过程，并且原语的执行必须是连续的，在执行过程中不允许被中断，也就是说CAS是一条CPU的原子指令，不会造成所谓的数据不一致问题**
> 
> 版权声明：本文为CSDN博主「zejian_」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
> 原文链接：https://blog.csdn.net/javazejian/article/details/72772470



#### Unsafe类

Java中CAS操作的执行依赖于Unsafe类的方法，注意Unsafe类中的所有方法都是native修饰的，也就是说**Unsafe类中的方法都直接调用操作系统底层资源执行相应任务**，关于Unsafe类的主要功能点如下：

- 内存管理，Unsafe类中存在直接操作内存的方法

- 提供实例对象新途径。

- 类和实例对象以及变量的操作，主要方法如下

- 数组操作

- CAS 操作相关

  CAS是一些CPU直接支持的指令，也就是我们前面分析的无锁操作，在Java中无锁操作CAS基于以下3个方法实现，在稍后讲解Atomic系列内部方法是基于下述方法的实现的。

  ```java
  //第一个参数o为给定对象，offset为对象内存的偏移量，通过这个偏移量迅速定位字段并设置或获取该字段的值，
  //expected表示期望值，x表示要设置的值，下面3个方法都通过CAS原子指令执行操作。
  public final native boolean compareAndSwapObject(Object o, long offset,Object expected, Object x);                                                                                                  
  public final native boolean compareAndSwapInt(Object o, long offset,int expected,int x);
  
  public final native boolean compareAndSwapLong(Object o, long offset,long expected,long x);
  ```

- 挂起与恢复
- 内存屏障

#### 并发包中的原子操作类(Atomic系列)

下面进一步分析CAS在Java中的应用，即并发包中的原子操作类(Atomic系列)，从JDK 1.5开始提供了`java.util.concurrent.atomic`包，<u>在该包中提供了许多基于CAS实现的原子操作类，用法方便，性能高效，主要分以下4种类型。</u>

##### 原子更新基本类型

...

##### 原子更新引用

...

##### 原子更新数组

...

##### 原子更新属性

...

#### CAS的ABA问题及其解决方案

假设这样一种场景，当第一个线程执行CAS(V,E,N)操作，在获取到当前变量V，准备修改为新值N前，另外两个线程已连续修改了两次变量V的值，使得该值又恢复为旧值，这样的话，我们就无法正确判断这个变量是否已被修改过，如下图

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210420155850.png)



这就是典型的CAS的ABA问题，一般情况这种情况发现的概率比较小，<u>可能发生了也不会造成什么问题</u>，比如说我们对某个做加减法，不关心数字的过程，那么发生ABA问题也没啥关系。但是在某些情况下还是需要防止的，那么该如何解决呢？在Java中解决ABA问题，我们可以使用以下两个原子类

- AtomicStampedReference
- AtomicMarkableReference类

### AQS原理

[参考](https://blog.csdn.net/javazejian/article/details/75043422)

前面我们详谈过解决多线程同步问题的关键字synchronized，**synchronized属于隐式锁，即锁的持有与释放都是隐式的**，我们无需干预，而本篇我们要讲解的是显式锁，即锁的持有和释放都必须由我们手动编写。在Java 1.5中，官方在concurrent并发包中加入了**Lock接口，该接口中提供了lock()方法和unLock()方法对显式加锁和显式释放锁操作进行支持**，简单了解一下代码编写，如下：

```java
Lock lock = new ReentrantLock();
lock.lock();
try{
    //临界区......
}finally{
    lock.unlock();
}
```

正如代码所显示(ReentrantLock是Lock的实现类，稍后分析)，<u>当前线程使用lock()方法与unlock()对临界区进行包围</u>，其他线程由于无法持有锁将无法进入临界区直到当前线程释放锁，<u>注意unlock()操作必须在finally代码块中</u>，这样可以确保即使临界区执行抛出异常，线程最终也能正常释放锁，Lock接口还提供了锁以下相关方法

```java
public interface Lock {
    //加锁
    void lock();

    //解锁
    void unlock();

    //可中断获取锁，与lock()不同之处在于可响应中断操作，即在获
    //取锁的过程中可中断，注意synchronized在获取锁时是不可中断的
    void lockInterruptibly() throws InterruptedException;

    //尝试非阻塞获取锁，调用该方法后立即返回结果，如果能够获取则返回true，否则返回false
    boolean tryLock();

    //根据传入的时间段获取锁，在指定时间内没有获取锁则返回false，如果在指定时间内当前线程未被中并断获取到锁则返回true
    boolean tryLock(long time, TimeUnit unit) throws InterruptedException;

    //获取等待通知组件，该组件与当前锁绑定，当前线程只有获得了锁
    //才能调用该组件的wait()方法，而调用后，当前线程将释放锁。
    Condition newCondition();
```

可见Lock对象锁还提供了synchronized所不具备的其他同步特性，如可中断锁的获取(synchronized在等待获取锁时是不可中的)，超时中断锁的获取，等待唤醒机制的多条件变量Condition等，这也使得Lock锁在使用上具有更大的灵活性。下面进一步分析Lock的实现类重入锁ReetrantLock。

#### 重入锁ReetrantLock

重入锁ReetrantLock，JDK 1.5新增的类，实现了Lock接口，作用与synchronized关键字相当，但比synchronized更加灵活。

- ReetrantLock为可重入锁，即一个线程对资源重复加锁
- 支持公平锁和非公平锁；公平锁：先申请锁的先获得锁；非公平锁：反之

例子：

```java
package Test;

import java.util.concurrent.locks.ReentrantLock;

public class bean37 {
    public static void main(String[] args) throws InterruptedException {
        MyLock t1=new MyLock();
        MyLock t2=new MyLock();
        Thread thread1=new Thread(t1);
        Thread thread2=new Thread(t2);
        thread1.start();
        thread2.start();
        thread1.join();

    }
}

class MyLock implements Runnable {
    public static ReentrantLock lock = new ReentrantLock();
    public static int i = 0;

    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            lock.lock();
            lock.lock();
            try {
                i++;
                System.out.print("当前线程:"+Thread.currentThread()+" i="+i);
                System.out.println();
            } finally {
                lock.unlock();
                lock.unlock();
            }
        }
    }
}

```

ReenterLock其他方法说明如下：

```java
//查询当前线程保持此锁的次数。
int getHoldCount() 

//返回目前拥有此锁的线程，如果此锁不被任何线程拥有，则返回 null。      
protected  Thread   getOwner(); 

//返回一个 collection，它包含可能正等待获取此锁的线程，其内部维持一个队列，这点稍后会分析。      
protected  Collection<Thread>   getQueuedThreads(); 

//返回正等待获取此锁的线程估计数。   
int getQueueLength();

// 返回一个 collection，它包含可能正在等待与此锁相关给定条件的那些线程。
protected  Collection<Thread>   getWaitingThreads(Condition condition); 

//返回等待与此锁相关的给定条件的线程估计数。       
int getWaitQueueLength(Condition condition);

// 查询给定线程是否正在等待获取此锁。     
boolean hasQueuedThread(Thread thread); 

//查询是否有些线程正在等待获取此锁。     
boolean hasQueuedThreads();

//查询是否有些线程正在等待与此锁有关的给定条件。     
boolean hasWaiters(Condition condition); 

//如果此锁的公平设置为 true，则返回 true。     
boolean isFair() 

//查询当前线程是否保持此锁。      
boolean isHeldByCurrentThread() 

//查询此锁是否由任意线程保持。        
boolean isLocked()       
```

#### ReentrantLock和synchronized的区别



| ReentrantLock                      | synchronized                           |
| ---------------------------------- | -------------------------------------- |
| 显式获得锁、释放锁                 | 隐式获得锁、释放锁                     |
| API层面                            | JVM层面                                |
| 既可以实现公平锁又可以实现非公平锁 | 只能实现非公平锁                       |
| 乐观锁实现                         | 悲观锁实现                             |
| 需要在finally中显示释放锁          | 在发生异常时会自动释放锁，不会导致死锁 |



#### AQS工作原理概要

`AbstractQueuedSynchronizer`又称为队列同步器(后面简称AQS)，**它是用来构建锁或其他同步组件的基础框架**

> AQS核心思想如果共享资源空闲的时候，线程可以直接占用共享资源，并将共享资源上锁。
>
> 如果共享资源被其他线程占用了，线程将被封装成一个节点在同步队列中排队。

内部通过一个int类型的成员变量**state**来控制同步状态，**AQS使用CAS对该同步状态对其值进行修改（进行原子操作）。**

- 当state=0时，则说明没有任何线程占有共享资源的锁
- 当state=1时，则说明有线程目前正在使用共享变量，其他线程必须加入<u>同步队列</u>进行等待

AQS内部通过内部类Node构成FIFO的同步队列来完成线程获取锁的排队工作，同时利用内部类ConditionObject构建等待队列

- 当Condition调用wait()方法后，线程将会加入**等待队列**中
- 而当Condition调用signal()方法后，线程将从等待队列转移动**同步队列**中进行锁竞争。

AQS中的同步队列模型，如下

```java
/**
 * AQS抽象类
 */
public abstract class AbstractQueuedSynchronizer
    extends AbstractOwnableSynchronizer{
//指向同步队列队头
private transient volatile Node head;

//指向同步的队尾
private transient volatile Node tail;

//同步状态，0代表锁未被占用，1代表锁已被占用
private volatile int state;

//省略其他代码......
}
```

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210420172355.png)



head和tail分别是AQS中的变量，其中head指向同步队列的头部，注意head为空结点，不存储信息。而tail则是同步队列的队尾。

同步队列采用的是双向链表的结构这样可方便队列进行结点增删操作。

- state变量则是代表同步状态，执行当线程调用lock方法进行加锁后，如果此时state的值为0，则说明当前线程可以获取到锁，同时将state设置为1，表示获取成功。

- 如果state已为1，也就是当前锁已被其他线程持有，<u>那么当前执行线程将被封装为Node结点加入同步队列等待</u>。

其中Node结点是对每一个访问同步代码的线程的封装，

从图中的Node的数据结构也可看出，其包含了需要同步的线程本身（thread）以及线程的状态（waitStatus），如是否被阻塞，是否等待唤醒，是否已经被取消等。每个Node结点内部关联其前继结点prev和后继结点next，这样可以方便线程释放锁后快速唤醒下一个在等待的线程，Node是AQS的内部类，

```java
static final class Node {
    //共享模式
    static final Node SHARED = new Node();
    //独占模式
    static final Node EXCLUSIVE = null;

    //标识线程已处于结束状态
    static final int CANCELLED =  1;
    //等待被唤醒状态
    static final int SIGNAL    = -1;
    //条件状态，
    static final int CONDITION = -2;
    //在共享模式中使用表示获得的同步状态会被传播
    static final int PROPAGATE = -3;

    //等待状态,存在CANCELLED、SIGNAL、CONDITION、PROPAGATE 4种
    volatile int waitStatus;

    //同步队列中前驱结点
    volatile Node prev;

    //同步队列中后继结点
    volatile Node next;

    //请求锁的线程
    volatile Thread thread;

    //等待队列中的后继结点，这个与Condition有关，稍后会分析
    Node nextWaiter;

    //判断是否为共享模式
    final boolean isShared() {
        return nextWaiter == SHARED;
    }

    //获取前驱结点
    final Node predecessor() throws NullPointerException {
        Node p = prev;
        if (p == null)
            throw new NullPointerException();
        else
            return p;
    }

    //.....
}
```

- SHARED：共享模式，共享模式是一个锁允许多条线程同时操作，如信号量Semaphore采用的就是基于AQS的共享模式实现的
- EXCLUSIVE：独占模式，独占模式则是同一个时间段只能有一个线程对共享资源进行操作，多余的请求线程需要排队等待，如ReentranLock。
- waitStatus：表示当前被封装成Node结点的等待状态，共有4种取值CANCELLED、SIGNAL、CONDITION、PROPAGATE。
  - CANCELLED：标识线程已处于结束状态，进入该状态后的结点将不会再变化
  - SIGNAL：等待被唤醒状态，只要前继结点释放锁，就会通知标识为SIGNAL状态的后继结点的线程执行
  - CONDITION：条件状态，与Condition相关，<u>该标识的结点处于等待队列</u>中，结点的线程等待在Condition上，当其他线程调用了Condition的signal()方法后，CONDITION状态的结点将从等待队列转移到同步队列中，等待获取同步锁。
  - PROPAGATE：在共享模式中使用表示获得的同步状态会被传播；在共享模式中，该状态标识结点的线程处于可运行状态。



对于锁的实现存在两种不同的模式，即共享模式(如Semaphore)和独占模式(如ReetrantLock)，无论是共享模式还是独占模式的实现类，其内部都是基于AQS实现的，也都维持着一个虚拟的同步队列，当请求锁的线程超过现有模式的限制时，会将线程包装成Node结点并将线程当前必要的信息存储到node结点中，然后加入同步队列等会获取锁，而这系列操作都有AQS协助我们完成，这也是作为基础组件的原因，无论是Semaphore还是ReetrantLock，其内部绝大多数方法都是间接调用AQS完成的。

ReentrantLock与AQS的关系：

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210420184724.png)







```java
//AQS中提供的主要模板方法，由子类实现。
public abstract class AbstractQueuedSynchronizer
    extends AbstractOwnableSynchronizer{

    //独占模式下获取锁的方法
    protected boolean tryAcquire(int arg) {
        throw new UnsupportedOperationException();
    }

    //独占模式下解锁的方法
    protected boolean tryRelease(int arg) {
        throw new UnsupportedOperationException();
    }

    //共享模式下获取锁的方法
    protected int tryAcquireShared(int arg) {
        throw new UnsupportedOperationException();
    }

    //共享模式下解锁的方法
    protected boolean tryReleaseShared(int arg) {
        throw new UnsupportedOperationException();
    }
    //判断是否为持有独占锁
    protected boolean isHeldExclusively() {
        throw new UnsupportedOperationException();
    }

}
```

从设计模式角度来看，**AQS采用的模板模式的方式构建的，其内部除了提供并发操作核心方法以及同步队列操作外，还提供了一些模板方法让子类自己实现**，如加锁操作以及解锁操作，为什么这么做？这是因为AQS作为基础组件，封装的是核心并发操作，但是实现上分为两种模式，即共享模式与独占模式，而这两种模式的加锁与解锁实现方式是不一样的，但AQS只关注内部公共方法实现并不关心外部不同模式的实现，所以提供了模板方法给子类使用，

- 也就是说实现独占锁，如ReentrantLock需要自己实现`tryAcquire()`方法和`tryRelease()`方法，

- 而实现共享模式的Semaphore，则需要实现`tryAcquireShared()`方法和`tryReleaseShared()`方法，

这样做的好处是显而易见的，无论是共享模式还是独占模式，其基础的实现都是同一套组件(AQS)，只不过是加锁解锁的逻辑不同罢了，

更重要的是如果我们需要自定义锁的话，也变得非常简单，只需要选择不同的模式实现不同的加锁和解锁的模板方法即可。

......







### volatile关键字

JMM（Java内存模型）

在Java内存模型下，线程可以将变量保存到本地内存（比如计算机的寄存器）中，而不是直接在主存中进行读写。这就可能造成当一个线程在主存中修改了一个变量的值，而另外一个线程还继续使用他在寄存器中的拷贝值时数据不一致。

为了保证共享数据的安全，可以使用线程同步机制（通过对象锁）。

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201129211018.png)



`而volatile`关键字为共享**变量**的同步访问提供了一种免锁机制，使得线程每一次使用共享变量都是直接到主存中进行操作。

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201129211616.png)





常见问题：

​	synchronized 关键字和 volatile 关键字的区别

-  `volatile` 只能修饰变量，而`synchronized`能够修饰方法和代码块。volatile 关键字是线程同步的轻量级实现，所以volatile 性能肯定比 synchronized 关键字要好
- volatile 关键字**能保证数据的可见性，但不能保证数据的原子性**。synchronized 关键字两者都能保证。
- volatile 关键字主要用于解决变量在多个线程之间的可见性，而 synchronized 关键字解决的是多个线程之间访问资源的同步性。



### 并发编程的三个重要特性

1. **原子性** : 一个的操作或者多次操作，要么所有的操作全部都得到执行并且不会收到任何因素的干扰而中断，要么都不执行。`synchronized` 可以保证代码片段的原子性。（即`synchronized`中的代码块一定是全部一起执行的或者是不执行）
2. **可见性** ：当一个变量对共享变量进行了修改，那么另外的线程都是立即可以看到修改后的最新值。`volatile` 关键字可以保证共享变量的可见性。
3. **有序性** ：代码在执行的过程中的先后顺序，J<u>ava 在编译器以及运行期间的优化，代码的执行顺序未必就是编写代码时候的顺序</u>。**`volatile` 关键字可以禁止指令进行重排序优化**。--->volatile可以禁止指令重排

### happens-before

JMM中最核心的理论，保证内存可见性
在JMM中，如果一个操作执行的结果需要对另一个操作可见，那么这两个操作之间必须存在happens-before关系。

理论：如果一个操作happens-before另一个操作，那么第一个操作的执行结果将对第二个操作可见，而且第一个操作的执行顺序排在第二个操作之前。

两个操作之间存在happens-before关系，并不意味着一定要按照happens-before原则制定的顺序来执行。如果重排序之后的执行结果与按照happens-before关系来执行的结果一致，那么这种重排序并不非法。

- 重排序：J<u>ava 在编译器以及运行期间的优化，代码的执行顺序未必就是编写代码时候的顺序</u>。



### ThreadLocal

通常情况下，我们创建的变量是可以被任何一个线程访问并修改的。**如果想实现每一个线程都有自己的专属本地变量该如何解决呢？** JDK 中提供的`ThreadLocal`类正是为了解决这样的问题。 **`ThreadLocal`类主要解决的就是让每个线程绑定自己的值，可以将`ThreadLocal`类形象的比喻成存放数据的盒子，盒子中可以存储每个线程的私有数据。**

**如果你创建了一个`ThreadLocal`变量，那么访问这个变量的每个线程都会有这个变量的本地副本，这也是`ThreadLocal`变量名的由来。他们可以使用 `get（）` 和 `set（）` 方法来获取默认值或将其值更改为当前线程所存的副本的值，从而避免了线程安全问题。**

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210328092552.png)

```java
public class ThreadLocalTest {
    public static void main(String[] args) throws InterruptedException {
        ThreadLocal<String> threadLocal=new ThreadLocal<>();//创建一个ThreadLocal变量
        MyThread  myThread=new MyThread(threadLocal);
        Thread thread1=new Thread(myThread);
        thread1.setName("t1");


        threadLocal.set("beijing");//main进程调用了ThreadLocal，并设置为“beijing”，注意设置的是threadLocal在main线程的这个副本
        
        
        System.out.println(Thread.currentThread().getName()+" before ThreadLocal= "+threadLocal.get());

        thread1.start();
        Thread.sleep(1000*1);//使main进程睡眠1s，确保t1先执行

        System.out.println(Thread.currentThread().getName()+" after ThreadLocal= "+threadLocal.get());//输出为beijing,得到的是threadLocal在main线程的这个副本，t1线程中threadLocal副本的更改不会影响到main线程的副本。

    }
}

class  MyThread implements Runnable{
    ThreadLocal<String> local=new ThreadLocal<>();
    public MyThread(ThreadLocal<String> local ){
        this.local=local;
    }
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName()+" before ThreadLocal= "+local.get());//得到的为null，注意得到的是threadLocal在t1线程的这个副本,初始值为null
        local.set("shenzhen");
        System.out.println(Thread.currentThread().getName()+" after ThreadLocal= "+local.get());
    }
}

/*
main before ThreadLocal= beijing
t1 before ThreadLocal= null
t1 after ThreadLocal= shenzhen
main after ThreadLocal= beijing
*/
```

源码分析

`set`方法：

```java
public void set(T value) {
        Thread t = Thread.currentThread();//获取当前线程
        ThreadLocalMap map = getMap(t);//调用getMap获取ThreadLocalMap
        if (map != null) {//如果map存在，则将当前线程对象t作为key，要存储的对象作为value存到map里面去
            map.set(this, value);
        } else {//如果该Map不存在，则初始化一个。
            createMap(t, value);
        }
    }
```



### 线程池

因为频繁的创建线程开销很大，我们就可以将一些线程保留在线程池中，这样就可以即取即用了。

线程池中包含许多准备运行的线程，为线程池提供一个Runnable，就会有一个线程调用run方法。当run方法退出时，这个线程不会死亡，而是留在线程池中准备为下一个请求提供服务。

使用线程池:email:的好处：

- 降低资源消耗：通过重复利用已创建的线程来降低资源消耗
- 提高响应速度：当任务到达时，不用等待线程创建，就可执行任务
- 提高线程的可管理性：使用线程池可以进行统一分配，调优和监控

#### Callable和Runnable接口的区别

- 实现Callable接口的线程能够返回中执行结果->call函数有返回值，返回值的类型为`V`  ; call()方法可抛出异常

  ```java
  public interface Callable<V> {
      /**
       * Computes a result, or throws an exception if unable to do so.
       *
       * @return computed result
       * @throws Exception if unable to compute a result
       */
      V call() throws Exception;
  }
  ```

  

- 实现Runnable接口的线程不能够返回执行结果->run函数没有返回值 ；run()方法是不能抛出异常的

```java
public interface Runnable {
    /*
     * @see     java.lang.Thread#run()
     */
    public abstract void run();
}
```



#### 创建线程的第三种方式

Callable的使用：

```java
public class CallableTest01 {
    public static void main(String[] args) {
        MyThread01 myThread01=new MyThread01();
        /*
        创建一个线程需要通过 new Thread('Runnable')
        但是Thread内的参数应该为'Runnable'类型(Thread构造方法的参数类型为Runnable)
        我们通过Callable创建的对象为Callable,怎么办呢？
        这时我们可以通过Runnable下的一个实现类: FutureTask,来解决这一问题。
        FutureTask传入的参数类型为Callable类型(通过构造函数来包装Callable)
        再将FutureTask的对象作为Thread的参数
        
        */
        FutureTask futureTask=new FutureTask(myThread01);

        Thread thread01=new Thread(futureTask);
        thread01.setName("t1");
        thread01.start();
        System.out.println(futureTask.get());//可以得到call()方法的返回值

    }
}

class MyThread01 implements Callable<Integer> {
    @Override
    public Integer call() throws Exception {
        System.out.println(Thread.currentThread().getName()+ " is running");
        return 1024;
    }
}
/*
t1 is running
1024
*/
```

小结：

```
    Callable规定的方法是call()，而Runnable规定的方法是run()
    Callable的任务执行后可返回值，而Runnable的任务是不能返回值的。
    call()方法可抛出异常，而run()方法是不能抛出异常的。
    运行Callable任务可拿到一个Future对象， Future表示异步计算的结果。 它提供了检查计算是否完成的方法，以等待计算的完成，并检索  计算的结果。
    通过Future对象可了解任务执行情况，可取消任务的执行，还可获取任务执行的结果。
    Callable是类似于Runnable的接口，实现Callable接口的类和实现Runnable的类都是可被其它线程执行的任务。
```



#### FutureTask

FutureTask的关系图：

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201130094003.png)

首先`FutureTask`实现了`RunnableFuture`接口

```java
public class FutureTask<V> implements RunnableFuture<V>
```

`RunnableFuture`接口实现了`Future`接口和`Runnable`接口

```java
public interface RunnableFuture<V> extends Runnable, Future<V> {
    /**
     * Sets this Future to the result of its computation
     * unless it has been cancelled.
     */
    void run();
}
```



看一看`FutureTask`的构造方法：

```java
    public FutureTask(Callable<V> callable) {
        if (callable == null)
            throw new NullPointerException();
        this.callable = callable;
        this.state = NEW;       // ensure visibility of callable
    }
 
    public FutureTask(Runnable runnable, V result) {
        this.callable = Executors.callable(runnable, result);
        this.state = NEW;       // ensure visibility of callable
    }

```

可知FutureTask还可以包装Runnable和Callable

上面代码块可以看出：Runnable注入会被Executors.callable()函数转换为Callable类型，即**FutureTask最终都是执行Callable类型的任务**。

小结：

- FutureTask实现Runnable，所以能通过Thread包装执行，
- FutureTask实现Runnable，所以能通过提交给ExcecuteService来执行，注：ExecuteService：创建线程池实例对象，其中有submit（Runnable）、submit（Callable）方法
- 还可以直接通过get()函数获取执行结果，该函数会阻塞，直到结果返回。
- **因此FutureTask是Future也是Runnable，又是包装了的Callable( 如果是Runnable最终也会被转换为Callable )。**

因为FutureTask实现了Runnable接口，又能够通过构造方法包装Callable，所以能够借由FutureTask来通过Callable来创建线程。

#### 执行器

1.构造线程池:

通过Executors类的静态方法`newCachedThreadPoll`或`newFixedThreadPoll`来创建线程池

<img src="https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210328100211.png" style="zoom:80%;" />

2.将Runnable或Callable对象提交给ExecutorService

调用submit提交Runnable或Callable对象

```
Future<T> submit(Callable<T> task)
Future<?> submit(Runnable task)
Future<T> submit(Runnable task,T result)
```

调用submit时会返回一个Future对象，可用来得到结果或者取消任务

而调用execute时没有返回结果



3.关闭线程池

```
shutdown：被关闭的执行器不再接收新的任务，当所有任务执行完成后，线程池中的线程死亡
shutdownNow：立即取消所有尚未开始的任务
```

继承关系图如下

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201130115800.png)



例子：

```java
package ExecutorTest;

import java.util.concurrent.*;

/**
 * ClassName:    ExecutorTest
 * Package:    ExecutorTest
 * Description:
 * Datetime:    2020/11/30   上午11:12
 * Author:   shilongshen
 */
public class ExecutorTest {
    public static void main(String[] args) {
        ExecutorService executor= Executors.newCachedThreadPool();//Executors中的静态方法来创建线程池，返回类型为ExecutorService

        MyThread myThread=new MyThread();

        executor.submit(myThread);//通过submit提交Runnable对象

        executor.shutdown();//关闭线程

    }
}

class MyThread implements Runnable{
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName()+" is running");
    }
}

```



#### ThreadPoolExecutor的构造方法

注意

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210408200636.png)

```java
/**
     * Creates a new {@code ThreadPoolExecutor} with the given initial
     * parameters.
     *
     * @param corePoolSize the number of threads to keep in the pool, even
     *        if they are idle, unless {@code allowCoreThreadTimeOut} is set
     * @param maximumPoolSize the maximum number of threads to allow in the
     *        pool
     * @param keepAliveTime when the number of threads is greater than
     *        the core, this is the maximum time that excess idle threads
     *        will wait for new tasks before terminating.
     * @param unit the time unit for the {@code keepAliveTime} argument
     * @param workQueue the queue to use for holding tasks before they are
     *        executed.  This queue will hold only the {@code Runnable}
     *        tasks submitted by the {@code execute} method.
     * @param threadFactory the factory to use when the executor
     *        creates a new thread
     * @param handler the handler to use when execution is blocked
     *        because the thread bounds and queue capacities are reached
     * @throws IllegalArgumentException if one of the following holds:<br>
     *         {@code corePoolSize < 0}<br>
     *         {@code keepAliveTime < 0}<br>
     *         {@code maximumPoolSize <= 0}<br>
     *         {@code maximumPoolSize < corePoolSize}
     * @throws NullPointerException if {@code workQueue}
     *         or {@code threadFactory} or {@code handler} is null
     */
    public ThreadPoolExecutor(int corePoolSize,
                              int maximumPoolSize,
                              long keepAliveTime,
                              TimeUnit unit,
                              BlockingQueue<Runnable> workQueue,
                              ThreadFactory threadFactory,
                              RejectedExecutionHandler handler) {
        if (corePoolSize < 0 ||
            maximumPoolSize <= 0 ||
            maximumPoolSize < corePoolSize ||
            keepAliveTime < 0)
            throw new IllegalArgumentException();
        if (workQueue == null || threadFactory == null || handler == null)
            throw new NullPointerException();
        this.corePoolSize = corePoolSize;
        this.maximumPoolSize = maximumPoolSize;
        this.workQueue = workQueue;
        this.keepAliveTime = unit.toNanos(keepAliveTime);
        this.threadFactory = threadFactory;
        this.handler = handler;
    }
```

```java
public class bean33 {
    public static void main(String[] args) {
        //通过构造方法来创建线程
        ThreadPoolExecutor threadPoolExecutor =
                new ThreadPoolExecutor(1,2, 1000,
                        TimeUnit.MILLISECONDS,
                        new SynchronousQueue<Runnable>(),
                        Executors.defaultThreadFactory(),
                        new ThreadPoolExecutor.AbortPolicy());

        //通过Executors创建线程  -->通过预定义线程池创建
        ThreadPoolExecutor executorService = (ThreadPoolExecutor) Executors.newFixedThreadPool(1);
        System.out.println(executorService.getCorePoolSize());

        ThreadPoolExecutor newCachedThreadPool = (ThreadPoolExecutor) Executors.newCachedThreadPool();
    }
}

参考：https://www.jianshu.com/p/f030aa5d7a28
```



#### 线程池几个关键的属性

```
线程池中有特定数量的线程，当有任务到来时就会调用线程进行处理。
如果任务数量大于线程数量，多出来的任务就在等待队列中等待
```

- corePoolSize：核心线程数；定义了线程池中可以同时运行的的最小线程数 
- maximumPoolSize：当队列中存放的任务达到等待队列容量的时候，当前可以同时运行的线程数量变成最大线程数 
- workQueue：任务等待队列，用来存放等待执行的任务 ；当新任务来到时，会先判断当前运行的线程数量是否达到核心线程数，如果达到的话，新任务就会被存放到等待队列中
- keepAliveTime：当线程池中的线程数量大于核心线程数，如果这个时候没有新的任务提交，核心线程外的线程不会立刻销毁，而是会等待，知道超过了keepAliveTime才会被销毁。
- unit：keepAliveTime参数的s时间单位
- handler：任务拒绝策略；饱和策略
- threadFactory：Executor创建新线程时会用到

##### threadFactory

如果当前可运行的线程数量大于最大线程数，并且等待队列也已经放满时，根据threadFactory会做出一些处理

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210408202303.png)



<img src="https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210408203706.png" style="zoom: 33%;" />

#### 常见问题

执行 execute()方法和 submit()方法的区别是什么呢？

1. **`execute()`方法用于提交不需要返回值的任务，所以无法判断任务是否被线程池执行成功与否；**
2. **`submit()`方法用于提交需要返回值的任务。线程池会返回一个 `Future` 类型的对象，通过这个 `Future` 对象可以判断任务是否执行成功**，并且可以通过 `Future` 的 `get()`方法来获取返回值，`get()`方法会阻塞当前线程直到任务完成，而使用 `get（long timeout，TimeUnit unit）`方法则会阻塞当前线程一段时间后立即返回，这时候有可能任务没有执行完。





## 参考

Java核心技术 卷1（第11版）



## 常见问题

关于Java线程说法正确的是（  *ACD* ）

```
*A*线程创建后，调用start()方法进入就绪状态

*B*线程创建后，调用run()方法进入就绪状态

*C*在同一Thread对象上不允许两次调用strat()方法  //同一Thread对象即同一线程，在线程的整个生命过程中，只会调用一次start()，多次启动线程是违法的

*D*线程调用stop()后进入终止状态
```


