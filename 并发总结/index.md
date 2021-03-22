# 并发总结


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
   //        start()方法的作用是：启动一个分支线程，在JVM中开辟一个新的栈空间，这段代码任务完成后，瞬间就结束了
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
（二）、同步阻塞：运行的线程在获取对象的同步锁时，若该同步锁被别的线程占用，则JVM会把该线程放入锁池中。当锁池中的线程拿到对象的锁时会重新进入Ready转态
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
		1.1.2 所以必定是这一次必定是生产者线程进行生产，此时，所以会添加一个元素，然后执行notify，尽管此时等待队列中没有线程，			   run方法结束，生产者线程释放cpu时间片和对象锁。
```





### 线程同步

#### 什么时候会存在线程安全问题

当满足以下条件（竞态条件）：

- 多线程并发
- 有共享数据的行为
- 共享数据有修改数据的行为

多线程可能会导致共享数据被破坏，引起线程安全问题。



#### 如何解决线程安全问题

用排队执行的机制来解决线程安全问题，这种机制称为线程同步机制。

如何实现线程同步？使用

#### synchronized关键字

1.首先明确：

- 每个对象都有一把锁，用来保护代码片段
- 锁可以管理试图进入被保护代码段的线程（如wait,notify等方法）



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
- volatile 关键字能保证数据的可见性，但不能保证数据的原子性。synchronized 关键字两者都能保证。
- volatile 关键字主要用于解决变量在多个线程之间的可见性，而 synchronized 关键字解决的是多个线程之间访问资源的同步性。



### 并发编程的三个重要特性

1. **原子性** : 一个的操作或者多次操作，要么所有的操作全部都得到执行并且不会收到任何因素的干扰而中断，要么都不执行。`synchronized` 可以保证代码片段的原子性。（即`synchronized`中的代码块一定是全部一起执行的或者是不执行）
2. **可见性** ：当一个变量对共享变量进行了修改，那么另外的线程都是立即可以看到修改后的最新值。`volatile` 关键字可以保证共享变量的可见性。
3. **有序性** ：代码在执行的过程中的先后顺序，Java 在编译器以及运行期间的优化，代码的执行顺序未必就是编写代码时候的顺序。`volatile` 关键字可以禁止指令进行重排序优化。

### ThreadLocal

通常情况下，我们创建的变量是可以被任何一个线程访问并修改的。**如果想实现每一个线程都有自己的专属本地变量该如何解决呢？** JDK 中提供的`ThreadLocal`类正是为了解决这样的问题。 **`ThreadLocal`类主要解决的就是让每个线程绑定自己的值，可以将`ThreadLocal`类形象的比喻成存放数据的盒子，盒子中可以存储每个线程的私有数据。**

**如果你创建了一个`ThreadLocal`变量，那么访问这个变量的每个线程都会有这个变量的本地副本，这也是`ThreadLocal`变量名的由来。他们可以使用 `get（）` 和 `set（）` 方法来获取默认值或将其值更改为当前线程所存的副本的值，从而避免了线程安全问题。**

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

### 线程池

线程池中包含许多准备运行的线程，为线程池提供一个Runnable，就会有一个线程调用run方法。当run方法退出时，这个线程不会死亡，而是留在线程池中准备为下一个请求提供服务。

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
shutdownNow：取消所有尚未开始的任务
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
        ExecutorService executor= Executors.newCachedThreadPool();//Executors中的静态方法来创建线程池

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



线程池几个关键的属性

```
corePoolSize：线程池中最小的工作线程数量 
maximumPoolSize：线程池最大线程数 
keepAliveTime：空闲线程等待执行任务的超时时间（纳秒） 
workQueue：任务缓存队列，用来存放等待执行的任务 
handler：任务拒绝策略
```

#### 常见问题：

执行 execute()方法和 submit()方法的区别是什么呢？

1. **`execute()`方法用于提交不需要返回值的任务，所以无法判断任务是否被线程池执行成功与否；**
2. **`submit()`方法用于提交需要返回值的任务。线程池会返回一个 `Future` 类型的对象，通过这个 `Future` 对象可以判断任务是否执行成功**，并且可以通过 `Future` 的 `get()`方法来获取返回值，`get()`方法会阻塞当前线程直到任务完成，而使用 `get（long timeout，TimeUnit unit）`方法则会阻塞当前线程一段时间后立即返回，这时候有可能任务没有执行完。
