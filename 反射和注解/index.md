# 反射和注解


# 反射

## 定义

在运行状态中，

- 对于任意一个类，都能够知道这个类的所有属性和方法
- 对于任意一个对象，都可以调用这个对象的任意属性和方法

这种动态获取信息以及调用方法的功能称为反射机制。

## 作用

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210407201936.png)

> 我的理解是class对象就是将.class文件加载到方法区中的二进制类型的类的数据

- 根据类名创建实例（类名可以从配置文件读取，不用new，达到解耦）  -->**在运行的时候才动态加载类**
- 用Method.invoke执行方法

```
 反射是一种可以间接操作目标对象的机制。当使用反射时，JVM 在运行的时候才动态加载类，对于任意类，知道其属性和方法，并不需要提前在编译期知道运行的对象是谁，允许运行时的 Java 程序获取类的信息并对其进行操作。
    对象的类型在编译期就可以确定，但程序运行时可能需要动态加载一些类（之前没有用到，故没有加载进 jvm），使用反射可以在运行期动态生成对象实例并对其进行操作。
在获取到 Class 对象之后，反向获取和操作对象的各种信息

```

> 假如你写了一段代码：Object o=new Object();
>
> 运行了起来！
>
> 首先JVM会启动，你的代码会编译成一个.class文件，然后被类加载器加载进jvm的内存中，你的类Object加载到方法区中，创建了Object类的class对象到堆中，注意这个不是new出来的对象，而是类的类型对象，每个类只有一个class对象，作为方法区类的数据结构的接口。jvm创建对象前，会先检查类是否加载，寻找类对应的class对象，若加载好，则为你的对象分配内存，初始化也就是代码:new Object()。
>
> 上面的流程就是你自己写好的代码扔给jvm去跑，跑完就over了，jvm关闭，你的程序也停止了。
>
> 为什么要讲这个呢？因为要理解反射必须知道它在什么场景下使用。
>
> 题主想想上面的程序对象是自己new的，程序相当于写死了给jvm去跑。假如一个服务器上突然遇到某个请求哦要用到某个类，哎呀但没加载进jvm，是不是要停下来自己写段代码，new一下，哦启动一下服务器。
>
>   反射是什么呢？当我们的程序在运行时，需要动态的加载一些类这些类可能之前用不到所以不用加载到jvm，而是在运行时根据需要才加载。
>
> 
>
> 作者：CaryTseng
> 链接：https://www.zhihu.com/question/24304289/answer/147529485
> 来源：知乎
> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

获取 Class 类对象三种方式：

•使用 Class.forName（String className）静态方法，该方法最常用。如JDBC中使用这个方法来加载驱动。

•使用类的.class方法。任何数据类型都拥有的 class 属性。

•使用实例对象的 getClass() 方法。



```text
Java的反射机制的实现要借助于4个类：class，Constructor，Field，Method;
```

其中class代表的是类对 象，Constructor－类的构造器对象，Field－类的属性对象，Method－类的方法对象。通过这四个对象我们可以粗略的看到一个类的各个组 成部分。

## Demo

作者：蛙课网
链接：https://www.zhihu.com/question/24304289/answer/964247763
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。



```java
public class Apple {

    private int price;

    public int getPrice() {
        return price;
    }

    public void setPrice(int price) {
        this.price = price;
    }

    public static void main(String[] args) throws Exception{
        //正常的调用
        Apple apple = new Apple();
        apple.setPrice(5);
        System.out.println("Apple Price:" + apple.getPrice());
        //使用反射调用
        Class clz = Class.forName("com.chenshuyi.api.Apple");
        Method setPriceMethod = clz.getMethod("setPrice", int.class);
        Constructor appleConstructor = clz.getConstructor();
        Object appleObj = appleConstructor.newInstance();
        setPriceMethod.invoke(appleObj, 14);
        Method getPriceMethod = clz.getMethod("getPrice");
        System.out.println("Apple Price:" + getPriceMethod.invoke(appleObj));
    }
}
```

从代码中可以看到我们使用反射调用了 setPrice 方法，并传递了 14 的值。之后使用反射调用了 getPrice 方法，输出其价格。上面的代码整个的输出结果是：

```java
Apple Price:5
Apple Price:14
```

从这个简单的例子可以看出，一般情况下我们使用反射获取一个对象的步骤：

- 获取类的 Class 对象实例

```text
Class clz = Class.forName("com.zhenai.api.Apple");
```

- 根据 Class 对象实例获取 Constructor 对象  -->**必须保证有一个无参构造方法**

```java
Constructor appleConstructor = clz.getConstructor();
```

- 使用 Constructor 对象的 newInstance 方法获取反射类对象

```text
Object appleObj = appleConstructor.newInstance();
```

而如果要调用某一个方法，则需要经过下面的步骤：

- 获取方法的 Method 对象

```java
Method setPriceMethod = clz.getMethod("setPrice", int.class);
```

- 利用 invoke 方法调用方法

```java
setPriceMethod.invoke(appleObj, 14);
```

---

**反射就是在运行时才知道要操作的类是什么，并且可以在运行时获取类的完整构造，并调用对应的方法。**

作者：蛙课网
链接：https://www.zhihu.com/question/24304289/answer/964247763
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。



## **反射常用API**

### **获取反射中的Class对象**

在反射中，要获取一个类或调用一个类的方法，我们首先需要获取到该类的 Class 对象。

在 Java API 中，获取 Class 类对象有三种方法：

**第一种，使用 Class.forName 静态方法。**当你知道该类的全路径名时，你可以使用该方法获取 Class 类对象。

```java
Class clz = Class.forName("java.lang.String");
```

**第二种，使用 .class 方法。**

这种方法只适合在编译前就知道操作的 Class。

```java
Class clz = String.class;
```

**第三种，使用类对象的 getClass() 方法。**

```java
String str = new String("Hello");
Class clz = str.getClass();
```

### **通过反射创建类对象**

通过反射创建类对象主要有两种方式：通过 Class 对象的 newInstance() 方法、通过 Constructor 对象的 newInstance() 方法。

第一种：通过 Class 对象的 newInstance() 方法。

```java
Class clz = Apple.class;
Apple apple = (Apple)clz.newInstance();
```

第二种：通过 Constructor 对象的 newInstance() 方法

```java
Class clz = Apple.class;
Constructor constructor = clz.getConstructor();
Apple apple = (Apple)constructor.newInstance();
```

通过 Constructor 对象创建类对象可以选择特定构造方法，而通过 Class 对象则只能使用默认的无参数构造方法。下面的代码就调用了一个有参数的构造方法进行了类对象的初始化。

```java
Class clz = Apple.class;
Constructor constructor = clz.getConstructor(String.class, int.class);
Apple apple = (Apple)constructor.newInstance("红富士", 15);
```

### **通过反射获取类属性、方法、构造器**

我们通过 Class 对象的 getFields() 方法可以获取 Class 类的属性，但无法获取私有属性。

```java
Class clz = Apple.class;
Field[] fields = clz.getFields();
for (Field field : fields) {
    System.out.println(field.getName());
}
```

输出结果是：

```java
price
```

而如果使用 Class 对象的 getDeclaredFields() 方法则可以获取包括私有属性在内的所有属性：

```java
Class clz = Apple.class;
Field[] fields = clz.getDeclaredFields();
for (Field field : fields) {
    System.out.println(field.getName());
}
```

输出结果是：

```java
name
price
```

与获取类属性一样，当我们去获取类方法、类构造器时，如果要获取私有方法或私有构造器，则必须使用有 declared 关键字的方法。



# 注解



## **注解的作用**

**格式**

```java
public @interface 注解名称{
    属性列表;
}
```

格式有点奇怪，我们稍后再研究。

**分类**

大致分为三类：自定义注解、JDK内置注解、还有第三方框架提供的注解。

自定义注解就是我们自己写的注解。JDK内置注解，比如@Override检验方法重写，@Deprecated标识方法过期等。第三方框架定义的注解比如SpringMVC的@Controller等。

**使用位置**

实际开发中，注解常常出现在类、方法、成员变量、形参位置。当然还有其他位置，这里不提及。

**作用**

如果说注释是写给人看的，那么注解就是写给程序看的。**它更像一个标签，贴在一个类、一个方法或者字段上。它的目的是为当前读取该注解的程序提供判断依据。**比如程序只要读到加了@Test的方法，就知道该方法是待测试方法，又比如@Before注解，程序看到这个注解，就知道该方法要放在@Test方法之前执行。

**级别**

注解和类、接口、枚举是同一级别的。

------

## **注解的本质**

@interface和interface这么相似，我猜注解的本质是一个接口。

为了验证这个猜测，我们做个实验。先按上面的格式写一个注解

![img](https://pic3.zhimg.com/80/v2-cf7d3faafd55af5faa5b7c0dbfc638ea_720w.jpg)属性先不写

编译后得到字节码文件

![img](https://pic2.zhimg.com/80/v2-3944093fa24382397bdde952b78243d1_720w.png)

通过XJad工具反编译MyAnnotation.class

![img](https://pic4.zhimg.com/80/v2-eb80156703d811f8d57c11840272f01b_720w.jpg)

我们发现，@interface变成了interface，而且自动继承了Annotation

![img](https://pic1.zhimg.com/80/v2-4fca230e3eddace36a242176c6b3bcdc_720w.jpg)

既然确实是个接口，那么我们自然可以在里面写方法

![img](https://pic1.zhimg.com/80/v2-24a76ea2b964b4c18115b5e62d3544ec_720w.jpg)

得到class文件后反编译

![img](https://pic4.zhimg.com/80/v2-374f6221e27fd35cd366894de93fd103_720w.jpg)

由于接口默认方法的修饰符就是public abstract，所以可以省略，直接写成：

![img](https://pic1.zhimg.com/80/v2-9dec5abb162b5484fe151e88ddcfdbc4_720w.jpg)

虽说注解的本质是接口，但是仍然有很多怪异的地方，比如使用注解时，我们竟然可以给getValue赋值：

![img](https://pic3.zhimg.com/80/v2-7506d841deabeecbbf021f45e469a8c2_720w.jpg)

你见过给方法赋值的操作吗？（别闹了，你脑中想到的是给方法传参）。虽然这里的getValue可能不是指getValue()，底层或许是getValue()返回的一个同名变量。但不管怎么说，还是太怪异了。所以在注解里，类似于String getValue()这种，被称为“属性”。给属性赋值显然听起来好接受多了。

另外，我们还可以为属性指定默认值：

![img](https://pic4.zhimg.com/80/v2-fdd0c92796d162345fee12133b8a98cb_720w.jpg)

![img](https://pic3.zhimg.com/80/v2-cd77bb34b3cc593ca5a6a87d7d8b1802_720w.jpg)

当没有赋值时，属性将使用默认值，比如上面的defaultMethod()，它的getValue就是“no description"。

基于以上差异，以后还是把注解单独归为一类，不要当成接口使用。

------

## **反射注解信息**

上文已经说过，注解就像一个标签，是贴在程序代码上供另一个程序读取的。所以三者关系是：

![img](https://pic3.zhimg.com/80/v2-3c4bab856af60100a255bdafecb3ce1e_720w.jpg)

**要牢记，只要用到注解，必然有三角关系：定义注解，使用注解，读取注解。**仅仅完成前两步，是没什么卵用的。就好比你写了一本武林秘籍却没人去学，那么这门武功还不如一把菜刀。

所以，接下来我们写一个程序读取注解。读取注解的思路是：

![img](https://pic4.zhimg.com/80/v2-cf58ff26dcce9378cf0d237d7f35cb17_720w.jpg)

反射获取注解信息：

![img](https://pic3.zhimg.com/80/v2-23366472182783e8b2f116c7e336dc9e_720w.jpg)

我们发现，Class、Method、Field对象都有个getAnnotation()，可以获取各自位置的注解信息。

但是控制台提示“空指针异常”，IDEA提示我们：Annotation 'MyAnnotation.class' is not retained for reflective。直译的话就是：注解MyAnnotation并没有为反射保留。

这是因为注解其实有所谓“保留策略”的说法。大家学习JSP时，应该学过<!-- -->和<%-- -->的区别：前者可以在浏览器检查网页源代码时看到，而另一个在服务器端输出时就被抹去了。同样的，注解通过保留策略，控制自己可以保留到哪个阶段。保留策略也是通过注解实现，它属于元注解，也叫元数据。

------

## **元注解**

所谓元注解，就是加在注解上的注解。作为普通程序员，常用的就是：

- @Documented

用于制作文档，不是很重要，忽略便是

- @Target

加在注解上，限定该注解的使用位置。不写的话，好像默认各个位置都是可以的。如果需要限定注解的使用位置，可以在自定义的注解上使用该注解。我们本次默认即可，不特别限定。

![img](https://pic1.zhimg.com/80/v2-b118cf13e71e7c0fd8698de17e018274_720w.jpg)

- @Retention（注解的保留策略）

![img](https://pic2.zhimg.com/80/v2-f338eaee843420505b24548bc63eb8b9_720w.jpg)

注解的保留策略有三种：SOURCE/ClASS/RUNTIME

![img](https://pic2.zhimg.com/80/v2-d8c81ca3bcf6b35370d45e0035da1f69_720w.jpg)

- 注解主要被反射读取
- 反射只能读取内存中的字节码信息
- RetentionPolicy.CLASS指的是保留到字节码文件，它在磁盘内，而不是内存中。虚拟机将字节码文件加载进内存后注解会消失
- 要想被反射读取，保留策略只能用RUNTIME，即运行时仍可读取

![img](https://pic3.zhimg.com/80/v2-9443c07bb515c3c29aac25086cc1dd7e_720w.jpg)

![img](https://pic2.zhimg.com/80/v2-136297dc24497f9d722c165f1e5d573d_720w.jpg)

重新运行程序，成功读取注解信息：

![img](https://pic3.zhimg.com/80/v2-a84dbe9c316c1f99bf05f6d901e4d89a_720w.jpg)

注意，defaultMethod()反射得到的注解信息是：no description。就是MyAnnotion中getValue的默认值。

但是，注解的读取并不只有反射一种途径。比如@Override，它由编译器读取（你写完代码ctrl+s时就编译了），而编译器只是检查语法错误，此时程序尚未运行。所以，我猜@Override的保留策略肯定不是RUNTIME：

![img](https://pic1.zhimg.com/80/v2-bce935e0f2d982358491ae2b9fc0f358_720w.jpg)保留策略为SOURCE，仅仅是源码阶段，编译成.class文件后就消失

------

## **属性的数据类型及特别的属性：value和数组**

**属性的数据类型**

- 八种基本数据类型
- String
- 枚举
- Class
- 注解类型
- 以上类型的一维数组

![img](https://pic3.zhimg.com/80/v2-d376ea9865ad5f9519ebe2630ee1e356_720w.jpg)

![img](https://pic3.zhimg.com/80/v2-9349bfe7cc9a47d20751751e6fafb65e_720w.jpg)



**value属性**

如果注解的属性只有一个，且叫value，那么使用该注解时，可以不用指定属性名，因为默认就是给value赋值：

![img](https://pic3.zhimg.com/80/v2-c5a8091357e0d9096ba4edac15ce0ed2_720w.jpg)

![img](https://pic1.zhimg.com/80/v2-6603c035c52a4e4931a672438319e600_720w.jpg)

![img](https://pic4.zhimg.com/80/v2-c23e384f1cb5f308f873a744299a909f_720w.jpg)

但是注解的属性如果有多个，无论是否叫value，都必须写明属性的对应关系：

![img](https://pic1.zhimg.com/80/v2-e27de8947efcae856be6e4a5296c59b4_720w.jpg)

![img](https://pic3.zhimg.com/80/v2-7c3a72003f1ac3e31b487a48a78f0f02_720w.jpg)



**数组属性**

如果数组的元素只有一个，可以省略{}：

![img](https://pic2.zhimg.com/80/v2-9e9c5fbbf9b66690581d4402b7d6b265_720w.jpg)

------

小结

- 注解就像标签，是程序判断执行的依据。比如，程序读到@Test就知道这个方法是待测试方法，而@Before的方法要在测试方法之前执行
- 注解需要三要素：定义、使用、读取并执行
- 注解分为自定义注解、JDK内置注解和第三方注解（框架）。自定义注解一般要我们自己定义、使用、并写程序读取，而JDK内置注解和第三方注解我们只要使用，定义和读取都交给它们
- 大多数情况下，三角关系中我们只负责使用注解，无需定义和执行，框架会将**注解类**和**读取注解的程序**隐藏起来，除非阅读源码，否则根本看不到。**平时见不到定义和读取的过程，光顾着使用注解，久而久之很多人就忘了注解如何起作用了！**

![img](https://pic1.zhimg.com/80/v2-7310679f5c3ca0159b93132afe8ba3c0_720w.jpg)

关于注解的使用案例，请参考注解（下）

# 参考

[参考1](https://www.jianshu.com/p/9be58ee20dee)

[参考2](https://www.runoob.com/w3cnote/java-annotation.html)

[参考3](https://zhuanlan.zhihu.com/p/60941426)

[参考4](https://zhuanlan.zhihu.com/p/60966151)
