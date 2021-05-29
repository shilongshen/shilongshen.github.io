# Java基础总结




String.getBytes(String decode)方法会根据指定的decode编码返回某**字符串在该编码下的byte数组**表示，如：

~~~java
byte[] a= "shen".getBytes();//java默认的编码方式为unicode
        for (byte s:a){
            System.out.println(s);
        }
/*
115
104
101
110
*/
~~~



### final，static关键字

| final 关键字  | 表示常量，即变量被赋值后不能被更改。例如：final double number=24; 被final修饰的类不能被继承，被final修饰的方法不能被重写，能被重载。final不能修饰接口和抽象类。 |
| ------------- | ------------------------------------------------------------ |
| static 关键字 | 静态字段属于类而不属于对象，通过类名.静态字段来访问；静态方法属于类，通过类名.静态方法来使用。静态方法只能访问静态字段，不能访问实例字段。 |

- 在类中使用`final`定义常量必须进行初始化操作。

### this关键字，super关键字

| this  | this指向当前对象 |
| ----- | ---------------- |
| super | super指向父类    |

### 自动装箱和自动拆箱

- **装箱**：将基本类型用它们对应的引用类型包装起来；
- **拆箱**：将包装类型转换为基本数据类型；

~~~java
//自动装箱
//向ArrayList<Integer>中添加int元素
list.add(3);
//等价于
list.add(Integer.valueOf(3));
~~~

~~~java
//自动拆箱
int n=list.get(3);
//等价于
int n =list.get(3).intValue();  
~~~

### 方法的参数

两个基本概念：

- 按值调用：方法接收调用者提供的值
- 按引用调用：方法接收调用者提供的变量地址

**Java语言总是按值调用**。也就是说，方法得到的参数值是一个**副本**。

- 如果该参数是基本数据类型（和以String str="aaa"这种方式创建的字符串）的话，那方法不能修改该参数。
- 如果方法参数是对象引用（例如对象和数组）的话，那方法可以改变对象参数。（因为原来的对象引用和这个副本都引用同一个对象）

例子：

~~~java
public class    test01 {
    public static void main(String[] args) {

        int num1=10;
        int num2=20;
        Swap(num1,num2);//参数是基本数据类型，按值传递
        System.out.printf("num1=%d\n",num1);
        System.out.printf("num2=%d\n",num2);


    }
    public static void Swap(int a,int b){
        int temp=a;
        a=b;
        b=temp;
        System.out.printf("a=%d\n",a);
        System.out.printf("b=%d\n",b);
    }
}
/*
a=20
b=10
num1=10
num2=20
*/
~~~

![example 1 ](https://camo.githubusercontent.com/8943d6a83be198245233aafea4f5c7bb44fe8beaf24734c4cac105a1fe3cb8f3/687474703a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f31382d392d32372f32323139313334382e6a7067)

在 swap 方法中，a、b 的值进行交换，并不会影响到 num1、num2。因为，a、b 中的值，只是从 num1、num2 的复制过来的。也就是说，a、b 相当于 num1、num2 的副本，副本的内容无论怎么修改，都不会影响到原件本身。

---



~~~java
public static void main(String[] args) {
		int[] arr = { 1, 2, 3, 4, 5 };
		System.out.println(arr[0]);
		change(arr);//引用传递，
		System.out.println(arr[0]);
	}

	public static void change(int[] array) {
		// 将数组的第一个元素变为0
		array[0] = 0;
	}
/*
1
0
*/
~~~



其中array是arr的副本，两者都指向同一数组对象。

<img src="https://gitee.com/shilongshen/image-bad/raw/master/img/20201119215949.png" style="zoom: 80%;" />

### 重载和重写的区别

首先定义方法的签名：方法的签名包括方法名和参数类型。注意返回类型不是方法的签名。也就是说不能有两个名字相同，参数类型也相同却有不同返回类型的方法。

重载（overlode）：如果有多个方法具有相同的方法名，不同的参数，便出现了方法重载。

重写（override）：当子类继承父类，子类必须对父类的某一个方法进行改变时，就发生了重写。注意重写要求方法签名相同，并且返回类型也要相同。

- 注意构造方法不能够被重写，但是能够被重载

### 成员变量和局部变量的区别

- 成员对象也称为实例变量，成员对象属于对象实例，随着对象的创建而创建，随着对象的消失而消失；局部变量属于方法中定义的变量或方法参数，在方法被调用后自动消失

- 成员变量可以被public ,private ,protected,static 修饰符修饰。而局部变量不能。但两个变量都能被final修饰

### 强制对象类型转换

明确：子类的功能比父类的功能更多。父类有的方法子类都有，但是子类有的方法父类不一定有。

向上转型：父类引用指向子类对象。这个时候我们用一个功能较弱的类型引用一个功能较强的对象，我们将子类转换为了父类。例如：

~~~java
Father father =new Son();
~~~

在这里Son 对象实例被向上转型为father了,但是请注意这个Son对象实例在内存中的本质上还是Son类型，只不过它的功能**被临时消弱了**（这时我们只能调用father中的方法）。

向下转型：

分为两种情况

~~~java
//1
Father father =new Son();
Son son=(Son) father;  //ok!

//2
Father father =new Father();
Son son=(Son) father;//error!  //z等价与Son son=new Father();
~~~

对于情况1,即先是父类引用指向子类对象，在将父类引用还原回子类引用。因为我们创建的对象为功能更多的子类对象，子类对象的引用本应该是子类引用。

对于情况2,我们创建了一个功能更少的父类对象，尝试用子类引用指向父类对象，但是由于父类对象的功能少，所以子类对象不能指向父类对象。



```java
public class    test01 {
    public static void main(String[] args) {
        Father father=new Son();//父类引用指向子类对象，此时的father只能调用method1,method2方法
        Son son =(Son) father;//向下转型，此时son可以调用子类中的全部方法
        
        /*
        如果是这样调用则会出错
        Father father=new Father();
        Son son =(Son) father;
        */
}
}

class Father{

    public void method1(){
        System.out.println("father's method 1");
    }

    public void method2(){
        System.out.println("father's method 2");
    }
}

class Son extends Father {
    
    public void method03() {
        System.out.println("son's method 3");
    }

    public void method4() {
        System.out.println("father's method 4");
    }
}
```



### 面向对象的三大特性

#### **继承**

- 将一般的方法放在父类中，将特殊的方法放在子类中。
- 父类中的私有方法和私有属性，子类无法访问  。(保护成员是可以访问的)

```java
class X{
	Y y=new Y();
	public X(){
		System.out.print("X");
	}
}
class Y{
	public Y(){
		System.out.print("Y");
	}
}
public class Z extends X{
	Y y=new Y();
	public Z(){
		System.out.print("Z");
	}
	public static void main(String[] args) {
		new Z();
	}
}

/*
YXYZ
*/

/*
初始化过程： 
1. 初始化父类中的静态成员变量和静态代码块 ； 
2. 初始化子类中的静态成员变量和静态代码块 ； 
3.初始化父类的普通成员变量和代码块，再执行父类的构造方法；
4.初始化子类的普通成员变量和代码块，再执行子类的构造方法； 
 
（1）初始化父类的普通成员变量和代码块，执行  Y y=new Y();  输出Y 
（2）再执行父类的构造方法；输出X
（3） 初始化子类的普通成员变量和代码块，执行  Y y=new   Y();  输出Y 
（4）再执行子类的构造方法；输出Z
 所以输出YXYZ

*/
```



#### **多态**

- 定义：一个对象变量（对象引用）可以指向多个实际类型的现象称为**<u>多态</u>**。 
  - 例如：父类引用指向子类对象，父类引用可以指向父类对象。但是子类引用不能指向父类对象

- 对象引用调用的方法究竟属于哪一个对象（实际类型）是在运行期间动态确定的，这称为**<u>动态绑定</u>**。
  - 当父类引用指向子类对象。如果子类重写了父类的方法，真正执行的是子类覆盖的方法，如果子类没有覆盖父类的方法，执行的是父类的方法。



例：

son的实际类型是Son,它是Father的子类。如果Son定义了方法（重写）method1，则调用Son中的method1,否则调用Father中的method1。这是在运行期间动态决定的。

~~~java
public class   test01 {
    public static void main(String[] args) {
        Son son=new Son();
        son.method1();

}
}

class Father{
    public void method1(){
        System.out.println("father's method 1");
    }
    public void method2(){
        System.out.println("father's method 2");
    }
}

class Son extends Father {
    @Override
    public void method1() {
        System.out.println("son's method 1");
    }
     public void method03() {
        System.out.println("son's method 3");
    }

    public void method4() {
        System.out.println("father's method 4");
    }

}
~~~

**理解在方法调用时发生了什么？**

虚拟机预先为每一个类创建了一个方法表，其中列出了所有的方法签名和调用的实际方法

~~~java
Father:
	method1()->Father.method1()
	method2()->Father.method2()
Son:
	method1->Son.method1()//重写
	method2()->Father.method2()//子类拥有父类的非私有方法
	method3->Son.method3()//子类新增方法
	method4->Son.method4()//子类新增方法
~~~

在调用方法时，如：

~~~java
 Son son=new Son();
 son.method1();
~~~

首先根据son的**实际类型**来获取方法表：

~~~
Son:
	method1->Son.method1()
	method2()->Father.method2()//子类拥有父类的非私有方法
	method3->Son.method3()
	method4->Son.method4()
~~~

虚拟机在方法表中查找定义了method1()签名的类。确定应该调用的方法:Son.method1()。



在调用方法：如

~~~java
 Father father=new Son();
 father.method1();
~~~

根据father的**实际类型**来获取方法表：这里father的实际类型为Father。

~~~
Father:
	method1()->Father.method1()
	method2()->Father.method2()
~~~

虚拟机在方法表中查找定义了method1()签名的类。确定应该调用的方法:Father.method1()。但是由于Son已经将method1()进行重写了，所以实际上调用的是Son.method1()

小结：

多态存在的条件：

- 继承->父类和子类
- 子类重写父类中的方法->
- 父类引用指向子类对象

当父类引用指向子类对象时，原则上来说只能调用父类中的方法，但是一旦子类重写了父类的方法，就可以调用子类中的方法。这就是多态

再举个例子

~~~java
public class    test01 {
    public static void main(String[] args) {
        Father father=new Son();
        father.method1();
}
}

class Father{
   
    public void method1(){
        System.out.println("father's method 1");
    }
}

class Son extends Father {
    @Override
    public void method1() {
        System.out.println("son's method 1");
    }
}

~~~

这里输出的是son's method 1。这里满足了多态的三个条件：继承，父类引用指向子类对象，子类重写父类方法。因此在调用father.method1();时，实际上是动态绑定到了子类的method1方法上。

**多态的特点:**

- 对象类型和引用类型之间具有继承（类）/实现（接口）的关系；
- 对象类型不可变，引用类型可变；
- 方法具有多态性，属性不具有多态性；
- 引用类型变量发出的方法调用的到底是哪个类中的方法，必须在程序运行期间才能确定；
- <u>多态不能调用“只在子类存在但在父类不存在”的方法</u>；
- 如果子类重写了父类的方法，真正执行的是子类覆盖的方法，如果子类没有覆盖父类的方法，执行的是父类的方法。



---

#### **封装**

将对象的字段（field）隐藏在对象内部，不让外部直接操作，而是通过一些公有方法来间接的操作

把过程和数据包围起来，对数据的访问只能通过已定义的接口。面向对象计算始于这个基本概念。<u>封装实际上使用方法将类的数据隐藏起来，控制用户对类的修改和访问数据的程度。 适当的封装可以让程式码更容易理解和维护，也加强了程式码的安全性。</u> 



### 抽象类和接口

|                        |                                                              |                                                              |                  |                                |
| ---------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ---------------- | ------------------------------ |
| 抽象类（abstract修饰） | 抽象方法（用abstract修饰）相当于占位，继承抽象类的类必须重写抽象方法 | 抽象类可以有字段和具体方法；抽象类可以有构造方法。           | 抽象类不能实例化 | 抽象类不能多继承（extends）    |
| 接口（interface修饰）  | 实现接口的类必须重写接口中的方法。在实现方法时，必须把方法声明为public。接口中的所有方法都自动是public方法。 | 接口不能有字段和具体方法，但可以有静态常量；接口不可以有构造方法 | 接口不能实例化   | 可以实现多个接口（implements） |

补充：**JDK8开始，接口中可以定义有方法体的方法，方法必须被default和static修饰。除此之外，其他方法都是抽象方法。**

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210316214101.png)

常见问题：实现接口的类必须实现该接口的所有抽象方法？回答：如果是抽象类的话 就不需要实现所有的方法。

### ==和equals的区别

- == 它的作用是判断两个对象的地址是不是相等。即，判断两个对象是不是同一个对象。如果为基本数据类型==比较的是值。如果是引用数据类型，比较的是内存地址。
- equals 分为两种情况
  - 类没有重写equals,则通过Object类的equals方法进行比较，用来比较两个对象的引用是否相等，即内存地址。
  - 类重写了equals方法，一般我们重写equals方法来比较对象**内容**是否相等，若相等则返回true。

###  hashCode 与 equals

- hashCode()的作用是获得哈希码，返回的是一个int整型，用于确定该对象在哈希表中的索引位置。hashCode()在哈希表中才有用！

  

  注意：

- 如果两个对象相等，那么哈希码一定相等

- 但是两个对象的hashcode相等，两个对象不一定相等 

所以

- 重写equals方法时必须重写hashCode方法，如果equals方法返回true,哈希码一定相等



### 构造函数

构造函数的作用是完成对象的初始化。

下面的对象创建方法中哪些会调用构造方法 ：

```
new语句创建对象
java反射机制使用java.lang.Class或java.lang.reflect.Constructor的newInstance()方法
```



### i++和++i的区别

```java
int m=7;
int n=7;
int a=2* ++m  (此时a=16,m=8)  -->先进行加法运再进行乘法
int b=2* n++  (此时a=14,n=8)  -->先进行乘法运算再进行加法    
```

i--和--i同理

### String s和 new String的区别

String s1 = "a" 时，首先会在**字符串常量池**中查找有无 “a” 这个对象。 若没找到，就创建一个 "a" 对象。然后，以 s1 为它的引用。若在字符串常量池中找到了“a”这个对象， 同样也将s1作为它的引用。若再执行一次 String s2 = "a" , 那么 s1 和 s2 都是同一个对象的引用，即 逻辑判断 s1 == s2 的结果是 true。

String s3 = new String("a") 时，将在**字符串常量池外的堆**里，创建一个 "a" 对象。然后，以 s3 为它的引用。这时，s3 对应的是字符串常量池外的一个对象。因此，无论 s3 == s2，还是 s3 ==s1，其结果都是 false。但是s3.equals( s2) ，结果为true，因为内容是相同的。



Q: String str=new String("hello");我只想问下这个hello是放在堆里面还是放在字符串常量中？  -->`new`操作一定会在堆中建立一个对象

A: 如果字符串常量池里存在hello这个字符串的话，则只在堆中创建一个hello对象，否则也要在字符串常量池中也创建一个hello常量，前者是新建了一个对象，后者是两个对象。



字符串常量池在方法区中，即在运行时常量池中，即方法区中。

[参考题](https://www.nowcoder.com/test/question/done?tid=42152683&qid=3450#summary)

### 基本数据类型

1、整数类型byte（1个字节）short（2个字节）int（4个字节）long（8个字节）

2、字符类型char（2个字节）

3、浮点类型float（4个字节）double（8个字节）

### Java类的访问权限

Java有四种访问权限， 其中三种有访问权限修饰符，分别为private，public和protected，还有一种不带任何修饰符。

1. private: Java语言中对访问权限限制的最窄的修饰符，一般称之为“私有的”。被其修饰的类、属性以及方法只能被该类的对象访问，其子类不能访问，更不能允许跨包访问。
2. default：即不加任何访问修饰符，通常称为“默认访问模式“。该模式下，**只允许在同一个包中进行访问**。
3. protect: 介于public 和 private 之间的一种访问修饰符，一般称之为“保护形”。**被其修饰的类、属性以及方法只能被类本身的方法及子类访问，即使子类在不同的包中也可以访问**。
4. public： Java语言中访问限制最宽的修饰符，一般称之为“公共的”。被其修饰的类、属性以及方法不仅可以跨类访问，而且允许跨包（package）访问。

<img src="https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210317230809.png" style="zoom:80%;" />



### 深拷贝和浅拷贝

浅拷贝：只复制对象本身

深拷贝：不仅会复制对象本身，还会同时克隆所有子对象。

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210326224959.png)

### Java内存模型

在Java内存模型下，线程可以将变量保存到本地内存（比如计算机的寄存器）中，而不是直接在主存中进行读写，线程需要将主存中的变量拷贝到本地内存中进行操作。

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201129211018.png)

### TCP/IP中分为几层，每层的作用是什么

TCP/IP中可以分为5层，分别是应用层、传输层、网络层、数据链路层和物理层。

应用层中针对不同的应用使用不同的应用协议，来实现应用在网络之间的交互。

传输层是用于建立网络中两个节点之间的数据通信，负责数据的可靠传输

网络层用于将网络中的节点的地址管理和路由选择

数据链路用于将同一个链路中的节点进行连接

物理层指的是实际的数据传输的硬件，例如电话线路等，不同的传输设备的特性是不一样的，物理层要尽可能的屏蔽掉这些差异

### String、StringBuilder和StringBuffer

```java

String str = "Test string";
StringBuilder strBuilder = new StringBuilder(str);
strBuilder.setCharAt(1, 'X');
str=Builder.toString();
 
 
String不可变    -->cu
StringBuilder可变
```

- String：字符串常量，不可变
- StringBuilder：字符串常量，可变，线程不安全
- StringBuffer：字符串常量，可变，线程安全
