# 

 

1. 下列说法正确的是（A B）

[链接](https://www.nowcoder.com/test/question/done?tid=42155666&qid=15530#summary)

```
A JAVA程序的main方法必须写在类里面
B JAVA程序中可以有多个名字为main方法  -->main方法重载，
C JAVA程序中类名必须与文件名一样   -->内部类可以不同
D JAVA程序的main方法中，如果只有一条语句，可以不用{}（大括号）括起来
```

2. Java是一门支持反射的语言,基于反射为Java提供了丰富的动态性支持，下面关于Java反射的描述，哪些是错误的：(   A D F   )

[链接](https://www.nowcoder.com/test/question/done?tid=42155666&qid=501025#summary)

```
A Java反射主要涉及的类如Class, Method, Filed,等，他们都在java.lang.reflet包下
B 通过反射可以动态的实现一个接口，形成一个新的类，并可以用这个类创建对象，调用对象方法
C 通过反射，可以突破Java语言提供的对象成员、类成员的保护机制，访问一般方式不能访问的成员
D Java反射机制提供了字节码修改的技术，可以动态的修剪一个类
E Java的反射机制会给内存带来额外的开销。例如对永生堆的要求比不通过反射要求的更多
F Java反射机制一般会带来效率问题，效率问题主要发生在查找类的方法和字段对象，因此通过缓存需要反射类的字段和方法就能达到与之间调用类的方法和访问类的字段一样的效率
```

```
A：Class类在java.lang包下，错；
B：动态代理可以通过接口与类实现，通过反射形成新的代理类，这个代理类增强了原来类的方法。对；
C：反射可以强制访问private类型的成员，对；
D：反射并不能对类进行修改，只能对类进行访问，错；
E：反射机制对永生堆要求较多，对；
F：即使使用换成，反射的效率也比调用类的方法低，错；
```

3. 下面有关forward和redirect的描述，正确的是(B C D) ？

[链接](https://www.nowcoder.com/test/question/done?tid=42157203&qid=15388#summary)

```
A forward是服务器将控制权转交给另外一个内部服务器对象，由新的对象来全权负责响应用户的请求-->并不是全权，还是要经过服务器
B 执行forward时，浏览器不知道服务器发送的内容是从何处来，浏览器地址栏中还是原来的地址
C 执行redirect时，服务器端告诉浏览器重新去请求地址
D forward是内部重定向，redirect是外部重定向
E redirect默认将产生301 Permanently moved的HTTP响应
```

```
1.从地址栏显示来说
forward是服务器请求资源,服务器直接访问目标地址的URL,把那个URL的响应内容读取过来,然后把这些内容再发给浏览器.浏览器根本不知道服务器发送的内容从哪里来的,所以它的地址栏还是原来的地址.
redirect是服务端根据逻辑,发送一个状态码,告诉浏览器重新去请求那个地址.所以地址栏显示的是新的URL.

2.从数据共享来说
forward:转发页面和转发到的页面可以共享request里面的数据.
redirect:不能共享数据.

3.从运用地方来说
forward:一般用于用户登陆的时候,根据角色转发到相应的模块.
redirect:一般用于用户注销登陆时返回主页面和跳转到其它的网站等.

4.从效率来说
forward:高.
redirect:低.
```

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210316154329.png)

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210316233245.png)

4. 阅读如下代码。 请问，对语句行 test.hello(). 描述正确的有（）

```java
package NowCoder;
class Test {
	public static void hello() {
	    System.out.println("hello");
	}
}
public class MyApplication {
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Test test=null;
		test.hello();
	}
}

/*
能编译通过，并正确运行
*/
```

```
当程序执行Test tset时：jvm发现还没有加载过一个称为”Test”的类，它就开始查找并加载类文件”Test.class”。它从类文件中抽取类型信息并放在了方法区中，jvm于是以一个直接指向方法区lava类的指针替换了'test'符号引用，以后就可以用这个指针快速的找到Test类了。所以这也是为什么可以直接test.任何静态的东西
```

5. J2EE中，当把来自客户机的HTTP请求委托给servlet时，会调用HttpServlet的（A  ）方法

```
A service
B doget
C dopost
D init
```

```
HttpServlet容器响应Web客户请求流程如下：
1）Web客户向Servlet容器发出Http请求；

2）Servlet容器解析Web客户的Http请求；

3）Servlet容器创建一个HttpRequest对象，在这个对象中封装Http请求信息；

4）Servlet容器创建一个HttpResponse对象；

5）Servlet容器调用HttpServlet的service方法，这个方法中会根据request的Method来判断具体是执行doGet还是doPost，把HttpRequest和HttpResponse对象作为service方法的参数传给HttpServlet对象；

6）HttpServlet调用HttpRequest的有关方法，获取HTTP请求信息；

7）HttpServlet调用HttpResponse的有关方法，生成响应数据；

8）Servlet容器把HttpServlet的响应结果传给Web客户。

doGet() 或 doPost() 是创建HttpServlet时需要覆盖的方法.
```

6. 一个文件中的字符要写到另一个文件中，首先需要（ ）。

```
FileInputStream fin = new FileInputStream(this.filename);。
```

```
程序的逻辑很简单。程序必须打开两个文件，以可读的方式打开一个已有文件和以可写的方式打开一个新文件，后将已有文件中的内容，暂时存放在内存中，再写入新的文件，后关闭所有文件，程序结束。
根据题意，首先需要读入一个文件中的字符，需要FileInputStream fin = new FileInputStream(this.filename);
```

7. 创建数组的两种方式：

```
int[] test;
int test[];
```

8. 下面有关java基本类型的默认值和取值范围，说法错误的是？(B)

```
A 字节型的类型默认值是0，取值范围是-2^7—2^7-1
B boolean类型默认值是false，取值范围是true\false
C 字符型类型默认是0，取值范围是-2^15 —2^15-1   -->应该是\u0000 0 —-\uFFFF
D long类型默认是0，取值范围是-2^63—2^63-1
```

```
默认值         取值范围 				示例

字节型 ：  	0 -2^7—-2^7-1 			 byte b=10;

字符型 ：  ‘ \u0000′ 0—-2^16-1        char c=’c’ ;

short :    0 -2^15—-2^15-1 			short s=10;

int :     0 -2^31—-2^31-1 			int i=10;

long : 0 -2^63—-2^63-1     			long o=10L;

float : 0.0f -2^31—-2^31-1 			float f=10.0F

double : 0.0d -2^63—-2^63-1 		double d=10.0;

boolean: false true\false 			boolean flag=true;
```

9. 关于 Socket 通信编程，以下描述正确的是：（C ）

```
A 客户端通过new ServerSocket()创建TCP连接对象
B 客户端通过TCP连接对象调用accept()方法创建通信的Socket对象
C 客户端通过new Socket()方法创建通信的Socket对象
D 服务器端通过new ServerSocket()创建通信的Socket对象
```

```
客户端通过new Socket()方法创建通信的Socket对象
服务器端通过new ServerSocket()创建TCP连接对象  accept接纳客户端请求
```

10. 非抽象类实现接口后，必须实现接口中的所有抽象方法，除了abstract外，方法头必须完全一致。(错误)

```java
正确
错误 ✌  -->子类的返回类型可以小于父类的返回类型,例如
//------------------------------------
interface A{
    Object a();
}

class B implements A{

    @Override
    public String a() {
        return null;
    }
}
```

```
方法头指：修饰符+返回类型 +方法名（形参列表）
接口的访问权限：public
两同两小一大原则:
返回值和参数列表相同
返回值类型小于等于父类的返回值类型
异常小于等于父类抛出异常
访问权限大于等于父类
```

11. 下列哪个选项是Java调试器？如果编译器返回程序代码的错误，可以用它对程序进行调试。

```
jdb.exe
```

```
java,exe是java虚拟机
javadoc.exe用来制作java文档
jdb.exe是java的调试器
javaprof,exe是剖析工具
javac : 这是java的编译器，它用于把java源文件（以.java后缀结尾）编译成java字节码文件（以.class后缀结尾）；
```

12. 以下哪几种方式可用来实现线程间通知和唤醒：( A C)

```
A Object.wait/notify/notifyAll
B ReentrantLock.wait/notify/notifyAll
C Condition.await/signal/signalAll
D Thread.wait/notify/notifyAll
```

```
wait()、notify()和notifyAll()是 Object类 中的方法 ；
Condition是在java 1.5中才出现的，它用来替代传统的Object的wait()、notify()实现线程间的协作，相比使用Object的wait()、 notify()，使用Condition1的await()、signal()这种方式实现线程间协作更加安全和高效。
```

13. Java中的关键字

```
A null
B true
C sizeof
D implements
E instanceof
```

```
1、null、true、false 是 Java 中的显式常量值，并不是关键字 或 保留字

2、sizeof 是 C/C++ 中的方法，Java 中并没有这个方法，也没有该关键字 或 保留字

3、implements 和 instanceof 都是 Java 中的关键字

4. const和goto是java的保留字
```

14. 在各自最优条件下,对N个数进行排序,哪个算法复杂度最低的是? （A）

```
A 插入排序
B 快速排序
C 堆排序
D 归并排序
```

<img src="https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210317225349.png" style="zoom:80%;" />

15. 运行代码，结果正确的是：

```JAVA

Boolean flag = false;
if(flag = true){
System.out.println("true");
}else{

System.out.println("false");
}
```

```
if(flag = true)的时候flag已经是true了，所以输出true；
要是为if(flag == true)输出才为false

Java里只有boolean类型可以作为判断条件，其他类型必须通过操作反回boolean值,这里本来就是boolean类型，可以充当判断条件。
个人理解，有误的请指出。
```

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210317225725.png)

16. 以下JAVA程序代码的输出是

```java
public static void main(String args[]) {
System.out.println(14^3);  //这是异或运算！
}
```

```
^表示异或 就是相同是0 不同是1
14是1110
3是0011
所以14^3=1101，即13

----------------------------------
按位或|
按位且&
按位取反~
按位异或^
----------------------------------
逻辑与&&
逻辑或||
非!
----------------------------------
左移<<:补0，相当于乘以2
右移>>：补符号位，相当于除以2
无符号右移>>>：补0
```

17. 关于以下application,说法正确是什么？

```java
public class Test {
	static int x=10;-->静态变量
	static {x+=5;}  -->静态代码块
	public static void main(String[] args) //4
        {
		System.out.println("x="+x);//输出为5
	}
	static{x/=3;};-->静态代码块
}//9

/*

编译通过，执行结果是：x=5
*/
```

```
首先明确一下执行顺序，静态代码块先于主方法执行，静态代码块之间遵从代码顺序执行。
所以：先初始化静态变量x=10；//x=10
执行第一个静态代码块，x=x+5; //x=15
执行第二静态代码块 x=x/3; //x=5
执行主方法： 输出x=5
拓展一下，在类中定义的{}之间被称为构造块，构造块相对于构造方法先执行，构造块之间按照代码编译顺序执行
此外还有普通代码块，存在于方法之中。
```

18. 下列描述正确的是（ A C ）？

```
A 类不可以多继承而接口可以多实现
B 抽象类自身可以定义成员而接口不可以  -->接口可以定义静态成员
C 抽象类和接口都不能被实例化
D 一个类可以有多个基类和多个基接口
```

19. 下列代码输出结果为（  D ）

```java
class Animal{
    public void move(){
        System.out.println("动物可以移动");
    }
}
class Dog extends Animal{
    public void move(){
        System.out.println("狗可以跑和走");
    }
    public void bark(){
        System.out.println("狗可以吠叫");
    }
}
public class TestDog{
    public static void main(String args[]){
        Animal a = new Animal();
        Animal b = new Dog(); 
        a.move();
        b.move();
        b.bark();//会报错
    }
}
```

```
A
动物可以移动
狗可以跑和走
狗可以吠叫
--------------
B
动物可以移动
动物可以移动
狗可以吠叫
--------------
C
运行错误
--------------
D
编译错误
```

```
如果父类引用指向子类对象，那父类引用不能够使用只在子类中存在的方法。
编译看左边，运行看右边。 父类型引用指向子类型对象，无法调用只在子类型里定义的方法
编译错误：The method bark() is undefined for the type Animal。Animal中没有定义bark()方法。
Dog继承自Animal。
当用Dog对象初始化Animal类对象时，完成了对Animal对象中方法与变量的覆盖与隐藏，也就是b.move()调用的是Dog中move()方法。而Animal中本身并没有bark()方法，不存在被覆盖的情况，亦无法访问，也就是b.bark()会报错。
```

20. 下列java程序的输出结果为____。

```java
public class Example{
    String str=new String("hello");//在堆中new一个对象，同时在字符串常量池中创建"hello"
    char[]ch={'a','b'};
    public static void main(String args[]){
        Example ex=new Example();
        ex.change(ex.str,ex.ch);
        System.out.print(ex.str+" and ");
        System.out.print(ex.ch);
    }
    public void change(String str,char ch[]){
        str="test ok";//在字符串常量池中创建一个"test ok"
        ch[0]='c';
    }
}
```

```
hello and cb
```

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210318081337.png)
