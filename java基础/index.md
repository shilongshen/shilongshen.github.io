# Java基础



# Java简介

Java将源代码编译成一种“字节码”，虚拟机（JVM）负责加载字节码并执行。

#### 名词解释

- JDK：Java Development Kit
- JRE：Java Runtime Environment

简单地说，JRE就是运行Java字节码的虚拟机。但是，如果只有Java源码，要编译成Java字节码，就需要JDK，因为JDK除了包含JRE，还提供了编译器、调试器等开发工具。

在JDK的bin目录下可以找到很多可执行文件：

- javac : 这是java的编译器，它用于把java源文件（以.java后缀结尾）编译成java字节码文件（以.class后缀结尾）；
- java : 这其实就是虚拟机（JVM）,负责执行java字节码。

## 第一个java程序

~~~java
public class Hello{
    public static mean(String[] args){
        System.out.println("hello,world!");
    }
}
~~~

在java程序中

~~~java
public class Hello{
...
}
~~~

为类的定义，Hello为类名，public表示这个类是公开的。

在类中定义了方法：

~~~java
public static mean(String[] args){
    ...
}
~~~

方法是可执行代码块，`mean`为方法名，用`()`括起来的是方法参数，这里的参数类型为`String[]`,参数名为`args`,`public`，`static`用来修饰方法，表明该方法为公开静态方法，`void`为方法的返回类型，`{}`中间为方法的代码，这才是真正执行的代码，必须以`;`结尾。

---

把代码保存为文件时，文件名必须和类名`Hello`一样，为`Hello.java`。

#### 如何运行Java程序

**注意`javac`是编译器，`java`是虚拟机**

~~~java
$ javac Hello.java
~~~

当前目录会产生`Hello.class`文件（字节码文件）

~~~java
$ java Hello
    >hello,world!
~~~

注意：给虚拟机传递的的参数是类名`Hello`。

# Java程序基础

## Java程序基本结构

~~~java
/**
*注释
*/
public class Hello{
    public static main(String[] args){
        ...//真正执行的语句，必须以`;`结尾
    }
}//class定义结束
~~~

## 变量和数据类型

在java中变量分为两种：`基本类型变量`和`引用类型变量`

先讨论基本类型变量。

**在java中，变量必须先定义再使用**。

变量有一个重要特点是可以重新赋值。

~~~java
//重新赋值变量
public class Main{
    public static void main(String[] args){
        int x=100;//定义int类型变量x，并赋予初始值100
        System.out.println(x);
        x=200;//对x重新赋值
        System.out.println(x);
    }
}
~~~

变量不但可以重新赋值，还可以赋值给其他变量。

~~~java
public class Main{
    public static void main(String[] args){
        int x=100;//定义int类型变量x，并赋予初始值100
        System.out.println(x);
        x=200;//对x重新赋值
        System.out.println(x);
        int n=x;//赋值给其他变量
        System.out.println(x);
    }
}
~~~

### 基本数据类型

- 整数类型 ：byte,short,int,long
- 浮点数 ：float,double
- 字符类型：char
- 布尔类型：boolean

计算机内存的最小存储单元是字节（byte），一个字节就是一个8位二进制数，即8个bit,它的二进制表示范围从`00000000`~`11111111`，换算成十进制是0~255，换算成十六进制是`00`~`ff`。

一个字节是1byte，1024字节是1K，1024K是1M，1024M是1G，1024G是1T。

不同的数据类型占用的字节数不一样。我们看一下Java基本数据类型占用的字节数：

```ascii
       ┌───┐
  byte │   │
       └───┘
       ┌───┬───┐
 short │   │   │
       └───┴───┘
       ┌───┬───┬───┬───┐
   int │   │   │   │   │
       └───┴───┴───┴───┘
       ┌───┬───┬───┬───┬───┬───┬───┬───┐
  long │   │   │   │   │   │   │   │   │
       └───┴───┴───┴───┴───┴───┴───┴───┘
       ┌───┬───┬───┬───┐
 float │   │   │   │   │
       └───┴───┴───┴───┘
       ┌───┬───┬───┬───┬───┬───┬───┬───┐
double │   │   │   │   │   │   │   │   │
       └───┴───┴───┴───┴───┴───┴───┴───┘
       ┌───┬───┐
  char │   │   │
       └───┴───┘
```

#### 整型

**java只定义了带符号的整型，最高位的bit表示符号位(0表示正数，1表示负数)**。各种整型能表示的最大范围：

- byte : -128~127
- short :-32768 ~ 32767
- int: -2147483648 ~ 2147483647
- long: -9223372036854775808 ~ 9223372036854775807

#### 浮点数

下面是定义浮点数的例子：

~~~java
float f1=3.14f;
double d1=1.79e308;
~~~

对于`float`类型，需要加上后缀`f`。

浮点数可表示的范围非常大，`float`类型可最大表示3.4x1038，而`double`类型可最大表示1.79x10308。

#### 布尔类型

~~~java
bloolean b1=true;

~~~

#### 字符类型

字符类型`char`表示一个字符。

~~~java
public class Main{
    public static void main(String[] args){
        char a='A';
        char zh='中';
        System.out.println(a);
        System.out.println(zh);
    }
}
~~~

**注意`char`类型使用单引号`'`，且仅有一个字符，要和双引号`"`的字符串类型区分开。**

再讨论引用类型。

#### 引用类型

除了上述基本类型的变量，剩下的都是引用类型。例如，引用类型最常用的就是`String`字符串：

~~~java
String s="hello"
~~~

**引用类型的变量类似于C语言的指针，它内部存储一个“地址”，指向某个对象在内存的位置。**(引用变量在栈内存，指向的对象在堆内存)

#### 常量

定义变量时如果加上`final`修饰符，就变成常量：

~~~java
final double PI=3.14;
double r=50;
double area=PI*r*r;
PI=300;//error!
~~~

**常量在定义时进行初始化后就不可再次赋值。**

#### var关键字

有些时候，类型的名字太长，写起来比较麻烦。例如：

~~~java
StringBuilder sb=new StringBuilder
~~~

这个时候，如果想省略变量类型，可以使用`var`关键字：

~~~java
var sb = new StringBuilder();
~~~

编译器会根据赋值语句自动推断出变量`sb`的类型是`StringBuilder`。

#### 变量的作用范围

在语句块中定义的变量有一个作用域，**就是从定义处开始，到语句块结束**。超出作用域引用这些变量，编译器就会报错。

~~~java
{
    ...
    int i = 0; // 变量i从这里开始定义
    ...
    {
        ...
        int x = 1; // 变量x从这里开始定义
        ...
        {
            ...
            String s = "hello"; // 变量s从这里开始定义
            ...
        } // 变量s作用域到此结束
        ...
        // 注意，这是一个新的变量s，它和上面的变量同名，
        // 但是因为作用域不同，它们是两个不同的变量:
        String s = "hi";
        ...
    } // 变量x和s作用域到此结束
    ...
} // 变量i作用域到此结束

~~~

## 整数运算

整数运算的除法只能得到结果的整数部分：

~~~java
int x=12345/67;//184
~~~

求余运算：

~~~java
int y=12345%67;//17
~~~

### 溢出

要特别注意，整数由于存在范围限制，如果计算结果超出了范围，就会产生溢出，而**溢出*不会出错*，却会得到一个奇怪的结果**：

~~~java
public class Main{
    public static void main(String[] args){
        int x = 2147483640;
        int y = 15;
        int sum = x + y;
        System.out.println(sum); // -2147483641
    }
}
~~~

### 自增/自减

`++`对一个整数进行加1操作，`--`对一个整数进行减1操作。

~~~java
public class Main{
    public static void main(String[] args){
        int x=3300;
        n++;
        System.out.println(n);
    }
}

>>3301
~~~

注意`++`写在前面和后面计算结果是不同的，`++n`表示先加1再引用n，`n++`表示先引用n再加1。

### 移位运算

左移`<<`,右移`>>`

如果对一个负数进行右移，最高位的`1`不动，结果仍然是一个负数：

~~~java
int n = -536870912;
int a = n >> 1;  // 11110000 00000000 00000000 00000000 = -268435456
int b = n >> 2;  // 11111000 00000000 00000000 00000000 = -134217728
int c = n >> 28; // 11111111 11111111 11111111 11111110 = -2
int d = n >> 29; // 11111111 11111111 11111111 11111111 = -1

~~~



还有一种无符号的右移运算，使用`>>>`，它的特点是不管符号位，右移后高位总是补`0`，因此，对一个负数进行`>>>`右移，它会变成正数，原因是最高位的`1`变成了`0`

~~~java
int n = -536870912;
int a = n >>> 1;  // 01110000 00000000 00000000 00000000 = 1879048192
int b = n >>> 2;  // 00111000 00000000 00000000 00000000 = 939524096
int c = n >>> 29; // 00000000 00000000 00000000 00000111 = 7
int d = n >>> 31; // 00000000 00000000 00000000 00000001 = 1

~~~

### 位运算

与，或，非，异或

与运算的规则是，必须两个数同时为`1`，结果才为`1`：

```java
n = 0 & 0; // 0
n = 0 & 1; // 0
n = 1 & 0; // 0
n = 1 & 1; // 1
```

或运算的规则是，只要任意一个为`1`，结果就为`1`：

```java
n = 0 | 0; // 0
n = 0 | 1; // 1
n = 1 | 0; // 1
n = 1 | 1; // 1
```

非运算的规则是，`0`和`1`互换：

```java
n = ~0; // 1
n = ~1; // 0
```

异或运算的规则是，如果两个数不同，结果为`1`，否则为`0`：

```java
n = 0 ^ 0; // 0
n = 0 ^ 1; // 1
n = 1 ^ 0; // 1
n = 1 ^ 1; // 0
```

### 类型的自动提升和强制转型

在运算过程中，如果参与运算的两个数类型不一致，那么计算结果为较大类型的整型。例如，`short`和`int`计算，结果总是`int`，原因是`short`首先自动被转型为`int`：

~~~java
public class Main {
    public static void main(String[] args) {
        short s = 1234;
        int i = 123456;
        int x = s + i; // s自动转型为int
        short y = s + i; // 编译错误!
    }
}

~~~

也可以将结果强制转型，即将大范围的整数转型为小范围的整数。强制转型使用`(类型)`，例如，将`int`强制转型为`short`：

~~~java
int i = 12345;
short s = (short) i; // 12345

~~~

要注意，超出范围的强制转型会得到错误的结果，原因是转型时，`int`的两个高位字节直接被扔掉，仅保留了低位的两个字节。

## 浮点数运算

浮点数运算和整数运算相比，只能进行加减乘除这些数值计算，不能做位运算和移位运算。

在计算机中，浮点数虽然表示的范围大，但是，浮点数有个非常重要的特点，就是浮点数常常无法精确表示。

### 类型提升

如果参与运算的两个数其中一个是整型，那么整型可以自动提升到浮点型。

### 溢出

整数运算在除数为`0`时会报错，而浮点数运算在除数为`0`时，不会报错，但会返回几个特殊值：

- `NaN`表示Not a Number
- `Infinity`表示无穷大
- `-Infinity`表示负无穷大

### 类型强转

可以将浮点数强制转型为整数。在转型时，浮点数的小数部分会被丢掉。如果转型后超过了整型能表示的最大范围，将返回整型的最大值。例如：

~~~java
int n1=(int) 12.3//12
~~~

## 布尔运算

对于布尔类型`boolean`，永远只有`true`和`false`两个值。

布尔运算是一种关系运算，包括以下几类：

- 比较运算符：`>`，`>=`，`<`，`<=`，`==`，`!=`
- 与运算 `&&`
- 或运算 `||`
- 非运算 `!`

~~~java
boolean b=5>3//true
int age =12;
boolean isTeenager = age>6 && age<12 //注意isTeenager是布尔类型，等号右边（age>6 && age<12）成立时，isTeenager=true,如果不成立isTeenager=false
~~~

### 短路运算

布尔运算的一个重要特点是短路运算。**如果一个布尔运算的表达式能提前确定结果，则后续的计算不再执行，直接返回结果。**

因为`false && x`的结果总是`false`，无论`x`是`true`还是`false`，因此，与运算在确定第一个值为`false`后，不再继续计算，而是直接返回`false`。

### 三元运算符

`b?x:y`，根据第一个布尔表达式的结果，分别返回后续两个表达式之一的计算结果。

注意到三元运算`b ? x : y`会首先计算`b`，如果`b`为`true`，则只计算`x`，否则，只计算`y`。此外，`x`和`y`的类型必须相同，因为返回值不是`boolean`，而是`x`和`y`之一。

~~~java
public class Main{
    public static void main(String[] args){
        int n=-100;
        int x=n>=0?n:-n;
        System.out.println(x);//100
    }
}
~~~

上述语句的意思是，判断`n >= 0`是否成立，如果为`true`，则返回`n`，否则返回`-n`。这实际上是一个求绝对值的表达式。

练习：

~~~java
public class Main{
    public static void main(String[] args){
        int age =7;
        boolean isTeenager = age>=6 && age<=12;
        System.out.println(isTeenager ? "yes" : "no");
    }
}
~~~

## 字符和字符串

**在Java中，字符和字符串是两个不同的类型**。

### 字符

**字符类型`char`是基本数据类型**，它是`character`的缩写。一个`char`保存一个Unicode字符。

因为Java在内存中总是使用Unicode表示字符，所以，一个英文字符和一个中文字符都用一个`char`类型表示，它们都占用**两个字节**。

### 字符串

和`char`类型不同，字**符串类型`String`是引用类型**，我们用双引号`"..."`表示字符串。一个字符串可以存储0个到任意个字符。

因为字符串使用双引号`"..."`表示开始和结束，那如果字符串本身恰好包含一个`"`字符怎么表示？例如，`"abc"xyz"`，编译器就无法判断中间的引号究竟是字符串的一部分还是表示字符串结束。这个时候，我们需要借助转义字符`\`：

~~~java
String s ="abc\"xyz";//包含7个字符: a, b, c, ", x, y, z
~~~

常见的转义字符包括：

- `\"` 表示字符`"`
- `\'` 表示字符`'`
- `\\` 表示字符`\`
- `\n` 表示换行符
- `\r` 表示回车符
- `\t` 表示Tab
- `\u####` 表示一个Unicode编码的字符

### 字符串的连接

Java的编译器对字符串做了特殊照顾，**可以使用`+`连接任意字符串和其他数据类型**，这样极大地方便了字符串的处理。例如：

~~~java
public class Main{
    public static void main(String[] args){
        String s1="Hello";
        String s2="World";
        String s=s1+" "+s2+"!";
        System.out.println(s);
    }
}
~~~

如果用`+`连接字符串和其他数据类型，会将其他数据类型先自动转型为字符串，再连接：

~~~java
public class Main {
    public static void main(String[] args) {
        int age = 25;
        String s = "age is " + age;
        System.out.println(s);
    }
}
~~~

### 不可变特性

~~~java
public class Main {
    public static void main(String[] args) {
        String s = "hello";
        System.out.println(s); // 显示 hello
        s = "world";
        System.out.println(s); // 显示 world
    }
}

~~~

观察执行结果，难道字符串`s`变了吗？其实变的不是字符串，而是变量`s`的“指向”。

执行`String s = "hello";`时，JVM虚拟机先创建字符串`"hello"`，然后，把字符串变量`s`指向它：

```ascii
      s
      │
      ▼
┌───┬───────────┬───┐
│   │  "hello"  │   │
└───┴───────────┴───┘
```

紧接着，执行`s = "world";`时，JVM虚拟机先创建字符串`"world"`，然后，把字符串变量`s`指向它：

```ascii
      s ──────────────┐
                      │
                      ▼
┌───┬───────────┬───┬───────────┬───┐
│   │  "hello"  │   │  "world"  │   │
└───┴───────────┴───┴───────────┴───┘
```

原来的字符串`"hello"`还在，只是我们无法通过变量`s`访问它而已。因此，**字符串的不可变是指字符串内容不可变。**

### 空值null

引用类型的变量可以指向一个空值`null`，表示不存在，即不指向任何对象。

~~~java
String s1=null;// s1是null
String s2;// 没有赋初值值，s2也是null
String s3=s1;// s3也是null
String s4="";// s4指向空字符串，不是null
~~~

练习：

请将一组int值视为字符的Unicode编码，然后将它们拼成一个字符串：

~~~java
public class Main{
    public static void main(String[] args){
        int a=72;
        int b=105;
        int c=65281;
        
        a1=(char)a;//类型强转为字符
        b1=(char)b;
        c1=(char)c;
        String s=""+a1+b1+c1;//为什么要加“” ？ 由于a1,b1,c1是字符，而s为字符串，"" + (char)a +(char)b +(char)c 首先执行空字符串与 (char)a 的加运算，右操作数会被转换成字符串类型。后续操作同理，最后计算结果也是 String 类型，才可以赋值给 String 类型的 s。
        System.out.println(s);
    }
}
~~~

## 数组类型

可以使用数组来表示“一组”`int`类型。代码如下：

~~~java
public class Main{
    public static void main(String[] args){
        int[] ns = new int [5];
        ns[0]=1;
        ns[1]=2;
        ns[2]=3;
        ns[3]=4;
        ns[4]=5;
        System.out.println(ns[0]);//1
    }
}
~~~

定义一个数组类型的变量，使用**数组类型“类型[]”**，例如，`int[]`。和单个基本类型变量不同，数组变量初始化必须使用`new int[5]`表示创建一个可容纳5个`int`元素的数组。

Java的数组有几个特点：

- 数组所有元素初始化为默认值，整型都是`0`，浮点型是`0.0`，布尔型是`false`；
- 数组一旦创建后，大小就不可改变。

要访问数组中的某一个元素，需要使用索引。数组索引从`0`开始，例如，5个元素的数组，索引范围是`0`~`4`。

可以修改数组中的某一个元素，使用赋值语句，例如，`ns[1] = 79;`。

**可以用`数组变量.length`获取数组大小：**

~~~java
public class Main{
    public static void main(String[] args){
        int[] ns = new int [5];
        ns[0]=1;
        ns[1]=2;
        ns[2]=3;
        ns[3]=4;
        ns[4]=5;
        System.out.println(ns[0]);//1
        System.out.println(ns.length);//5
    }
}
~~~

也可以在定义数组时直接指定初始化的元素，这样就不必写出数组大小，而是由编译器自动推算数组大小。例如：

~~~java
public class Main{
    public static void main(String[] args){
        int[] ns = new int[] {1,2,3,4,5};
        //还可以简化为： int[] ns = {1,2,3,4,5};
    }
}
~~~

**注意数组是引用类型，并且数组大小不可变**。我们观察下面的代码：

~~~java
public class Main {
    public static void main(String[] args) {
        // 5位同学的成绩:
        int[] ns;
        ns = new int[] { 68, 79, 91, 85, 62 };
        System.out.println(ns.length); // 5
        ns = new int[] { 1, 2, 3 };
        System.out.println(ns.length); // 3
    }
}

~~~

数组大小变了吗？看上去好像是变了，但其实根本没变。

对于数组`ns`来说，执行`ns = new int[] { 68, 79, 91, 85, 62 };`时，它指向一个5个元素的数组：

```ascii
     ns
      │
      ▼
┌───┬───┬───┬───┬───┬───┬───┐
│   │68 │79 │91 │85 │62 │   │
└───┴───┴───┴───┴───┴───┴───┘
```

执行`ns = new int[] { 1, 2, 3 };`时，它指向一个*新的*3个元素的数组：

```ascii
     ns ──────────────────────┐
                              │
                              ▼
┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┐
│   │68 │79 │91 │85 │62 │   │ 1 │ 2 │ 3 │   │
└───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘
```

但是，原有的5个元素的数组并没有改变，只是无法通过变量`ns`引用到它们而已。

### 字符串数组

如果数组元素不是基本类型，而是一个引用类型，那么，修改数组元素会有哪些不同？

<u>字符串是引用类型</u>，因此我们先定义一个字符串数组：

~~~java
String[] names = {"ABC", "XYZ","zoo"};
~~~

对于`String[]`类型的数组变量`names`，它实际上包含3个元素，但每个元素都指向某个字符串对象：

```ascii
          ┌─────────────────────────┐
    names │   ┌─────────────────────┼───────────┐
      │   │   │                     │           │
      ▼   │   │                     ▼           ▼
┌───┬───┬─┴─┬─┴─┬───┬───────┬───┬───────┬───┬───────┬───┐
│   │░░░│░░░│░░░│   │ "ABC" │   │ "XYZ" │   │ "zoo" │   │
└───┴─┬─┴───┴───┴───┴───────┴───┴───────┴───┴───────┴───┘
      │                 ▲
      └─────────────────┘
```

对`names[1]`进行赋值，例如`names[1] = "cat";`，效果如下：

```ascii
          ┌─────────────────────────────────────────────────┐
    names │   ┌─────────────────────────────────┐           │
      │   │   │                                 │           │
      ▼   │   │                                 ▼           ▼
┌───┬───┬─┴─┬─┴─┬───┬───────┬───┬───────┬───┬───────┬───┬───────┬───┐
│   │░░░│░░░│░░░│   │ "ABC" │   │ "XYZ" │   │ "zoo" │   │ "cat" │   │
└───┴─┬─┴───┴───┴───┴───────┴───┴───────┴───┴───────┴───┴───────┴───┘
      │                 ▲
      └─────────────────┘
```

这里注意到原来`names[1]`指向的字符串`"XYZ"`并没有改变，仅仅是将`names[1]`的引用从指向`"XYZ"`改成了指向`"cat"`，其结果是字符串`"XYZ"`再也无法通过`names[1]`访问到了。

---

# 流程控制

## 输入和输出

### 输出

在前面的代码中，我们总是使用`System.out.println()`来向屏幕输出一些内容。

**`println`是print line的缩写，表示输出并换行**。因此，如果输出后不想换行，可以用`print()`：

~~~java
public class Main{
    public static void main(String[] args){
        System.out.print("A,");
        System.out.print("B,");
    }
}
>>A,B,
~~~

### 格式化输出

如果要把数据显示成我们期望的格式，就需要使用格式化输出的功能。格式化输出使用`System.out.printf()`，通过使用占位符`%`，`printf()`可以把后面的参数格式化成指定格式：

~~~java
public class Main{
    public static void main(String[] args){
        double d= 3.1415926;
        System.out.printf("%.2f\n",d);
    }
}
~~~

| 占位符 | 说明                             |
| ------ | -------------------------------- |
| %d     | 格式化输出整数                   |
| %x     | 格式化输出十六进制整数           |
| %f     | 格式化输出浮点数                 |
| %e     | 格式化输出科学计数法表示的浮点数 |
| %s     | 格式化字符串                     |

注意，由于%表示占位符，因此，连续两个%%表示一个%字符本身。

占位符本身还可以有更详细的格式化参数。下面的例子把一个整数格式化成十六进制，并用0补足8位：

~~~java
public class Main {
    public static void main(String[] args) {
        int n = 12345000;
        System.out.printf("n=%d, hex=%08x", n, n); // 注意，两个%占位符必须传入两个数
    }
}
>>n=12345000, hex=00bc5ea8
~~~

### 输入

~~~java
import java.util.Scanner;

public class hello {
    public static void main(String[] args) {
        Scanner scanner = new Scanner((System.in));//创建Scanner对象
        System.out.printf("input your name:");//打印提示
        String name = scanner.nextLine();//读取一行输入并获取字符串
        System.out.printf("%s", name);//格式化输出
    }
}
~~~

首先，我们通过`import`语句导入`java.util.Scanner`，`import`是导入某个类的语句，必须放到Java源代码的开头，后面我们在Java的`package`中会详细讲解如何使用`import`。

然后，**创建`Scanner`对象并传入`System.in`**。`System.out`代表标准输出流，而`System.in`代表标准输入流。直接使用`System.in`读取用户输入虽然是可以的，但需要更复杂的代码，而通过`Scanner`就可以简化后续的代码。

**有了`Scanner`对象后，要读取用户输入的字符串，使用`scanner.nextLine()`，要读取用户输入的整数，使用`scanner.nextInt()`**。`Scanner`会自动转换数据类型，因此不必手动转换。

练习：

请帮小明同学设计一个程序，输入上次考试成绩（int）和本次考试成绩（int），然后输出成绩提高的百分比，保留两位小数位（例如，21.75%）。

~~~java
import java.util.Scanner;

public class hello {
    public static void main(String[] args) {
        Scanner scanner = new Scanner((System.in));
        System.out.print("input your last score:");
        int last_score = scanner.nextInt();
        System.out.print("input your now score:");
        int now_score = scanner.nextInt();
        float per=((float)now_score-(float)last_score)/(float)last_score;
        System.out.printf("%.2f%%",  per*100);
    }
}
~~~

## if判断

前面讲过了浮点数在计算机中常常无法精确表示，并且计算可能出现误差，因此，判断浮点数相等用`==`判断不靠谱,正确的方法是利用差值小于某个临界值来判断：

~~~java
public class Main{
    public static void main(String[] args){
        double x=1-9.0/10;
        if (Math.abs(x-0.1)<0.00001){
            System.out.println("x is 0.1");
        }
        else{
            System.out.println("x is not 0.1");
        }
    }
}
~~~

### 判断引用类型相等

要判断引用类型的变量内容是否相等，必须使用`equals()`方法：

~~~java
public class Main{
    public static void main(String[] args){
        String s1="hello";
        String s2="HELLO".toLowerCase();
        if (s1.equals(s2)){
            System.out.println("s1 equals s2");
        }
        else{
            System.out.println("s1 not equals s2");
        }
    }
}
~~~

注意：执行语句`s1.equals(s2)`时，如果变量`s1`为`null`，会报`NullPointerException`，要避免`NullPointerException`错误，可以利用短路运算：

~~~java
public class Main{
    public static void main(String[] args){
        String s1="hello";
        String s2="HELLO".toLowerCase();
        if (s1!=null && s1.equals(s2)){
            System.out.println("s1 equals s2");
        }
        else{
            System.out.println("s1 not equals s2");
        }
    }
}
~~~

练习：

请用`if ... else`编写一个程序，用于计算体质指数BMI，并打印结果。

BMI = 体重(kg)除以身高(m)的平方

BMI结果：

- 过轻：低于18.5
- 正常：18.5-25
- 过重：25-28
- 肥胖：28-32
- 非常肥胖：高于32

~~~java
import java.util.Scanner;

public class hello {
    public static void main(String[] args) {
        Scanner scanner = new Scanner((System.in));
        System.out.print("input your weight(kg)：");
        float w = scanner.nextFloat();
        System.out.print("input your height(m):");
        float h = scanner.nextFloat();
        float BMI = w / (h * h);
        if (BMI < 18.5) {
            System.out.printf("过轻,%.3f", BMI);
        } else if (18.5 <= BMI && BMI < 25) {
            System.out.printf("正常,%.3f", BMI);
        } else if (25 <= BMI && BMI < 28) {
            System.out.printf("过重,%.3f", BMI);
        } else if (28 <= BMI && BMI < 32) {
            System.out.printf("肥胖,%.3f", BMI);
        } else {
            System.out.printf("非常肥胖,%.3f", BMI);
        }
    }
}
~~~



JAVA中&&和&、||和|（短路与和逻辑与、短路或和逻辑或）的区别？


 首先名称是不同的

＆＆逻辑与　　｜｜逻辑或　　它们都是逻辑运算符

＆　按位与　　｜　按位或　　它们都是位运算符

ｉｆ（ａ＝＝１＆＆ｂ＝＝２）　这是说既要满足ａ＝１也要满足ｂ＝２

ｉｆ（ａ＝＝１｜｜ｂ＝＝２）　这是说或者满足ａ＝１或者要满足ｂ＝２

而ａ＆ｂ或者ａ｜ｂ则是二进制的与或运算

＆同为１时为１，否则为０

｜同为０时为０，否则为１

＆＆逻辑与　也叫做短路与　因为只要当前项为假，它就不往后判断了，直接认为表达式为假

｜｜逻辑或　也叫做短路或　因为只要当前项为真，它也不往后判断了，直接认为表达式为真

## switch多重选择

~~~java
import java.util.Scanner;

public class hello{
    public static void main(String[] args){
//        int option=1;
        Scanner scanner =new Scanner((System.in));
        System.out.print("input option:");
        int option=scanner.nextInt();
        switch (option){
            case 1:
                System.out.println("it is 1");
                break;

            case 2:
                System.out.println("it is 2");
                break;

            default:
                System.out.println("None");
                break;
        }
    }
}
~~~

使用`switch`时，如果遗漏了`break`，就会造成严重的逻辑错误，而且不易在源代码中发现错误。从Java 12开始，`switch`语句升级为更简洁的表达式语法，使用类似模式匹配（Pattern Matching）的方法，保证只有一种路径会被执行，并且不需要`break`语句：

~~~java
public class Main {
    public static void main(String[] args) {
        String fruit = "apple";
        switch (fruit) {
        case "apple" -> System.out.println("Selected apple");
        case "pear" -> System.out.println("Selected pear");
        case "mango" -> {
            System.out.println("Selected mango");
            System.out.println("Good choice!");
        }
        default -> System.out.println("No fruit selected");
        }
    }
}

~~~

注意新语法使用`->`，如果有多条语句，需要用`{}`括起来。不要写`break`语句，因为新语法只会执行匹配的语句，没有穿透效应。

### yield

大多数时候，在`switch`表达式内部，我们会返回简单的值。

但是，如果需要复杂的语句，我们也可以写很多语句，放到`{...}`里，然后，用`yield`返回一个值作为`switch`语句的返回值：

~~~java
public class Main{
    public static void main(String[] args){
        String fruit="orange";
        int opt=switch(fruit){
                case "apple"->1;
                case "bear"->2;
                default ->{
                    int code=fruit.hashCode();
                    yield code;// switch语句返回值
                }
        };
        System.out.println("opt="+opt);
    }
}
~~~



练习：

使用`switch`实现一个简单的石头、剪子、布游戏。

~~~java
import java.util.Scanner;

public class hello {
    public static void main(String[] args) {
        Scanner scanner = new Scanner((System.in));
        System.out.print("please input:Scissors or rock or cloth:");

//        System.out.print("please input:");
        String geature = scanner.nextLine();
//        String fruit="orange";
//        a=Math.random();
        int opt = switch (geature) {
            case "Scissors" -> 1;
            case "rock" -> 2;
            case "cloth" -> 3;
            default -> 0;

        };
        if (opt == 0) {
            System.out.println("error input!");
        } else {
            System.out.println("your choice is:" + geature);
            int random_num = (int) (1 + Math.random() * 3);//Math.random()返回大于等于0小于1的随机数，double型
            switch (random_num) {
                case 1 -> System.out.println("random_geature is Scissors");
                case 2 -> System.out.println("random_geature is rock");
                case 3 -> System.out.println("random_geature is cloth");
                default -> System.out.println("error random_geature");
            }

            int score = opt - random_num;
            System.out.printf("score is :%d\n", score);
            switch (score) {
                case 0 -> System.out.println("Tie");
                case -1, 2 -> System.out.println("loss");//当有多个条件满足时
                case 1, -2 -> System.out.println("win");
                default -> System.out.println("error");
            }
        }
    }
}

~~~

## while循环

练习：

使用`while`计算从`m`到`n`的和：

~~~java
public class Main {
	public static void main(String[] args) {
		int sum = 0;
		int m = 20;
		int n = 100;
		// 使用while计算M+...+N:
		while (m<=n) {
			sum=sum+m;
			m++;
		}
		System.out.println(sum);
	}
}
~~~

## do while 循环

在Java中，`while`循环是先判断循环条件，再执行循环。而另一种`do while`循环则是先执行循环，再判断条件，条件满足时继续循环，条件**不满足时**退出。它的用法是：

~~~java
do {
    执行循环语句
} while (条件表达式);
~~~

可见，**`do while`循环会至少循环一次**。

练习：

使用`do while`循环计算从`m`到`n`的和。

~~~java
public class Main{
    public static void main(String[] args){
        int sum=0;
        int m=20;
        int n=100;
        do{
            sum=sum+m;
            m++;
        }while(m<=n);
        System.out.println(sum);
    }
}
~~~

## for 循环

`for`循环的功能非常强大，它使用计数器实现循环。`for`循环会先初始化计数器，然后，在每次循环前检测循环条件，在每次循环后更新计数器。计数器变量通常命名为`i`。

~~~java
for (初始条件; 循环检测条件; 循环后更新计数器) {
    // 执行语句
}
~~~

### for each循环

`for`循环经常用来遍历数组，因为通过计数器可以根据索引来访问数组的每个元素：

~~~java
int [] ns={1,2,3,4,5};
for(int i=0;i<ns.length;i++){
    System.out.println(ns[i]);
}
~~~

但是，很多时候，我们实际上真正想要访问的是数组每个元素的值。Java还提供了另一种`for each`循环，它可以更简单地遍历数组：

~~~java
public class Main{
    public static void main(String[] args){
        int [] ns={1,2,3,4,5};
        for(int n:ns){
            System.out.println(n);
        }
    }
}
~~~

和`for`循环相比，`for each`循环的变量n不再是计数器，而是直接对应到数组的每个元素。`for each`循环的写法也更简洁。但是，`for each`循环无法指定遍历顺序，也无法获取数组的索引。

除了数组外，`for each`循环能够遍历所有“可迭代”的数据类型，包括后面会介绍的`List`、`Map`等。

练习1：

给定一个数组，请用`for`循环倒序输出每一个元素：

~~~java
public class Main{
    public static void main(String[] args){
        int [] ns={1,2,3,4,5};
        int length=ns.length-1;
        //System.out.println(length);
        for (int i=length;i>=0;i--){
            System.out.println(ns[i]);
        }
    }
}
~~~

练习2：

利用`for each`循环对数组每个元素求和：

~~~java
public class Main{
    public static void main(String[] args){
        int[] ns={1,2,3,4,5};
        int sum=0;
        for(int n:ns){
            sum=sum+n;
        }
        System.out.println(sum);
    }
}
~~~

练习3：

圆周率π可以使用公式计算：

$\frac{\pi}{4}=1-\frac{1}{3}+\frac{1}{5}-\frac{1}{7}+\frac{1}{9}-...$

请利用`for`循环计算π：

~~~java
public class hello {
    public static void main(String[] args) {
        double pi=0;
        int n=1;
        double q=0;
        double w=0;
        for(int i=0;i<1000000;i++){
            q=Math.pow(-1,i);
//            System.out.println(q);
            w=(double)1/n;
//            System.out.println(w);
            pi=pi+q*w;
            n+=2;
        }
        System.out.println(4*pi);
    }
}

~~~

## break 和continue

### break

在循环过程中，可以使用`break`语句跳出当前循环。

`break`语句通常都是配合`if`语句使用。要特别注意，`break`语句总是跳出自己所在的那一层循环。例如：

~~~java
public class Main {
    public static void main(String[] args) {
        for (int i=1; i<=10; i++) {
            System.out.println("i = " + i);
            for (int j=1; j<=10; j++) {
                System.out.println("j = " + j);
                if (j >= i) {
                    break;
                }
            }
            // break跳到这里
            System.out.println("breaked");
        }
~~~

### continue

`break`会跳出当前循环，也就是整个循环都不会执行了。而`continue`则是提前结束本次循环，直接继续执行下次循环。

~~~java
public class Main{
    public static void main(String[] args){
        int sum=0;
        for (int i=1;i<=10;i++){
            System.out.println("begin i="+i);
            if(i%2==0){
                continue;
            }
            System.out.println("end i="+i);
            
        }
    }
}
~~~

# 数组操作

## 遍历数组

通过`for`循环就可以遍历数组。因为数组的每个元素都可以通过索引来访问，因此，使用标准的`for`循环可以完成一个数组的遍历：

~~~java
public class Main{
    public static void main(String[] args){
        int[] ns={1,2,3,4,5};
        for(int i=0;i<ns.length;i++){
            int n=ns[i];
            System.out.println(n);
        }
    }
}
~~~

第二种方式是使用`for each`循环，直接迭代数组的每个元素：

~~~java
public class Main{
    public static void main(String[] args){
        int[] ns={1,2,3,4,5};
        for(int n:ns){
            System.out.println(n);
        }
    }
}
~~~

注意：在`for (int n : ns)`循环中，变量`n`直接拿到`ns`数组的元素，而不是索引。

### 打印数组内容

使用`for each`循环打印也很麻烦。幸好**Java标准库提供了`Arrays.toString()`，可以快速打印数组内容：**

~~~java
import java.util.Arrays;
public class Main{
    public static void main(String[] args){
        int[] ns={1,2,3,4,5};
        System.out.println(Arrays.toString(ns));
    }
}
~~~

练习：

请按倒序遍历数组并打印每个元素：

~~~java
public class Main{
    public static void main(String[] args){
        int [] ns={1,2,3,4,5};
        int length=ns.length-1;
        //System.out.println(length);
        for (int i=length;i>=0;i--){
            System.out.println(ns[i]);
        }
    }
}
~~~

## 数组排序

对数组进行排序是程序中非常基本的需求。常用的排序算法有冒泡排序、插入排序和快速排序等。

我们来看一下如何使用冒泡排序算法对一个整型数组从小到大进行排序：

~~~java
import java.util.Arrays;

public class hello {
    public static void main(String[] args) {
        int[] ns = {28, 12, 89, 73, 65, 18, 96, 50, 8, 36};//有10个数字，要进行10-1=9趟排序，每i趟要进行10-i次排序
        // 排序前:
        System.out.println(Arrays.toString(ns));
        System.out.println(ns.length);
        for (int i = 0; i < ns.length - 1; i++) {//控制共9趟排序(0~8)，i表示第i趟
            System.out.println(i);
            for (int j = 0; j < ns.length - 1 - i; j++) {//控制每i趟进行10-i次比较，
                /*
                * i=0表示第1趟，要进行9次排序，j=0~8;
                * i=1表示第2趟，要进行8次排序，j=0~7;
                * ...
                * i=8表示第9趟，要进行1次排序，j=0;
                * */
                if (ns[j] > ns[j + 1]) {
                    // 交换ns[j]和ns[j+1]:
                    int tmp = ns[j];
                    ns[j] = ns[j + 1];
                    ns[j + 1] = tmp;
                }
            }
        }
        // 排序后:
        System.out.println(Arrays.toString(ns));
    }
}

~~~

### 冒泡算法：

[参考](https://www.cnblogs.com/bigdata-stone/p/10464243.html)

0.如果遇到相等的值不进行交换，那这种排序方式是稳定的排序方式。

1.原理：比较两个相邻的元素，将值大的元素交换到右边

2.思路：依次比较相邻的两个数，将比较小的数放在前面，比较大的数放在后面。

　　　　(1)第一次比较：首先比较第一和第二个数，将小数放在前面，将大数放在后面。

　　　　(2)比较第2和第3个数，将小数 放在前面，大数放在后面。

　　　　......

　　　　(3)如此继续，知道比较到最后的两个数，将小数放在前面，大数放在后面，重复步骤，直至全部排序完成

　　　　(4)在上面一趟比较完成后，最后一个数一定是数组中最大的一个数，所以在比较第二趟的时候，最后一个数是不参加比较的。

　　　　(5)在第二趟比较完成后，倒数第二个数也一定是数组中倒数第二大数，所以在第三趟的比较中，最后两个数是不参与比较的。

　　　　(6)依次类推，每一趟比较次数减少依次

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200827145348.png)

4.算法分析：

　　　　(1)由此可见：**N个数字要排序完成，总共进行N-1趟排序，每i趟的排序次数为(N-i)次，所以可以用双重循环语句，外层控制循环多少趟，内层控制每一趟的循环次数**

　　　　(2)冒泡排序的优点：每进行一趟排序，就会少比较一次，因为每进行一趟排序都会找出一个较大值。如上例：第一趟比较之后，排在最后的一个数一定是最大的一个数，第二趟排序的时候，只需要比较除了最后一个数以外的其他的数，同样也能找出一个最大的数排在参与第二趟比较的数后面，第三趟比较的时候，只需要比较除了最后两个数以外的其他的数，以此类推……也就是说，没进行一趟比较，每一趟少比较一次，一定程度上减少了算法的量。

---

实际上，Java的标准库已经内置了排序功能，我们只需要调用JDK提供的`Arrays.sort()`就可以排序：

~~~java
import java.util.Arrays;
public class Main{
    public static void main(String[] args){
        int[] ns={1,2,,3,4,5};
        Arrays.sort(ns);
        System.out.println(Arrays.toString(ns));//toString():打印数组内容
    }
}
~~~

练习：

请思考如何实现对数组进行降序排序：

~~~java
import java.util.Arrays;

public class Main{
    public static void main(String[] args){
        int[] ns={1,2,3,4,5,6,7,8};
        System.out.print(Array.toString(ns));
        for(int i=0;i<ns.length-1;i++){
            for (int j=0;j<ns.length-1-i;j++){
                if(ns[j]<ns[j+1]){
                    int tmp=int ns[j];
                    ns[j+1]=ns[j];
                    ns[j]=ns[j+1];
                    ns[j+1]=tmp;
                }
            }
        }
        System.out.println(Arrays.toString(ns));
    }
}
~~~

## 多维数组

### 二维数组

二维数组就是数组的数组。定义一个二维数组如下：

~~~java
public class Main{
    public static void main(String[] args){
        int[][] ns={
            {1,2,3,4};
            {5,6,7,8};
            {9,10,11,12};
        }
        System.out.println(ns.length);//3
    }
}
~~~

因为`ns`包含3个数组，因此，`ns.length`为`3`。实际上`ns`在内存中的结构如下：

```ascii
                    ┌───┬───┬───┬───┐
         ┌───┐  ┌──>│ 1 │ 2 │ 3 │ 4 │
ns ─────>│░░░│──┘   └───┴───┴───┴───┘
         ├───┤      ┌───┬───┬───┬───┐
         │░░░│─────>│ 5 │ 6 │ 7 │ 8 │
         ├───┤      └───┴───┴───┴───┘
         │░░░│──┐   ┌───┬───┬───┬───┐
         └───┘  └──>│ 9 │10 │11 │12 │
                    └───┴───┴───┴───┘
```

访问二维数组的某个元素需要使用`array[row][col]`，例如：

~~~java
System.out.println(ns[1][2]);//7
~~~

要打印一个二维数组，可以使用两层嵌套的for循环,或者使用Java标准库的`Arrays.deepToString()`：

~~~java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        int[][] ns = {
            { 1, 2, 3, 4 },
            { 5, 6, 7, 8 },
            { 9, 10, 11, 12 }
        };
        System.out.println(Arrays.deepToString(ns));
    }
}

~~~

### 三维数组

~~~java
int[][][]={
    {
        {1,2,3},
        {4,5,6},
        {7,8,9}
    },
    {
        {10,11},
        {12,13}
    },
    {
        {14,15,16},
        {17,18}
    }
}
~~~

它在内存中的结构如下：

```ascii
                              ┌───┬───┬───┐
                   ┌───┐  ┌──>│ 1 │ 2 │ 3 │
               ┌──>│░░░│──┘   └───┴───┴───┘
               │   ├───┤      ┌───┬───┬───┐
               │   │░░░│─────>│ 4 │ 5 │ 6 │
               │   ├───┤      └───┴───┴───┘
               │   │░░░│──┐   ┌───┬───┬───┐
        ┌───┐  │   └───┘  └──>│ 7 │ 8 │ 9 │
ns ────>│░░░│──┘              └───┴───┴───┘
        ├───┤      ┌───┐      ┌───┬───┐
        │░░░│─────>│░░░│─────>│10 │11 │
        ├───┤      ├───┤      └───┴───┘
        │░░░│──┐   │░░░│──┐   ┌───┬───┐
        └───┘  │   └───┘  └──>│12 │13 │
               │              └───┴───┘
               │   ┌───┐      ┌───┬───┬───┐
               └──>│░░░│─────>│14 │15 │16 │
                   ├───┤      └───┴───┴───┘
                   │░░░│──┐   ┌───┬───┐
                   └───┘  └──>│17 │18 │
                              └───┴───┘
```

如果我们要访问三维数组的某个元素，例如，`ns[2][0][1]`，只需要顺着定位找到对应的最终元素`15`即可。

练习：

使用二维数组可以表示一组学生的各科成绩，请计算所有学生的平均分：

~~~java
public class hello{
    public static void main(String[] args){
        int[][] scores = {
                { 82, 90, 91 },
                { 68, 72, 64 },
                { 95, 91, 89 },
                { 67, 52, 60 },
                { 79, 81, 85 },
        };
        double average=0;
//        System.out.println(scores[0].length);
        int num= scores[0].length;
        for(int i=0;i<scores.length;i++){
            for(int j=0;j<num;j++){
                average+=scores[i][j];
            }
        }
        average=average/15;
        System.out.println(average);
        if (Math.abs(average-77.733333)<0.000001){
            System.out.println("测试成功");
        }
        else{
            System.out.println("测试失败");
        }
    }
}
~~~

## 命令行参数

Java程序的入口是`main`方法，而`main`方法可以接受一个命令行参数，它是一个`String[]`数组。

这个命令行参数由JVM接收用户输入并传给`main`方法：

~~~java
public class Main {
    public static void main(String[] args) {
        for (String arg : args) {
            System.out.println(arg);
        }
    }
}
~~~



---

# 面向对象编程

面向对象的基本概念，包括：

- 类
- 实例
- 方法

面向对象的实现方式，包括：

- 继承
- 多态

Java语言本身提供的机制，包括：

- package
- classpath
- jar

以及Java标准库提供的核心类，包括：

- 字符串
- 包装类型
- JavaBean
- 枚举
- 常用工具类

## 面向对象基础

#### class和instance

所以，只要理解了class和instance的概念，基本上就明白了什么是面向对象编程。

class是一种对象模版，它定义了如何创建实例，因此，**class本身就是一种数据类型**：

![class](https://www.liaoxuefeng.com/files/attachments/1260571618658976/l)

而instance是对象实例，instance是根据class创建的实例，可以创建多个instance，每个instance类型相同，但各自属性可能不相同：

![instances](https://www.liaoxuefeng.com/files/attachments/1260571718581056/l)



#### 定义class

在Java中，创建一个类，例如，给这个类命名为`Person`，就是定义一个`class`：

```java
class Person {
    public String name;
    public int age;
}
```

一个`class`可以包含多个字段（`field`），字段用来描述一个类的特征。上面的`Person`类，我们定义了两个字段，一个是`String`类型的字段，命名为`name`，一个是`int`类型的字段，命名为`age`。因此，通过`class`，把一组数据汇集到一个对象上，实现了数据封装。

`public`是用来修饰字段的，它表示这个字段可以被外部访问。

#### 创建实例

- 构造对象
- 指定初始状态
- 对对象应用方法
- 使用new构造一个对象

定义了class，只是定义了对象模版，而要**根据对象模版创建出真正的对象实例，必须用new操作符**。

new操作符可以创建一个实例，然后，我们需要定义一个引用类型的变量来指向这个实例：

```java
Person ming = new Person();
```

上述代码创建了一个Person类型的实例，并通过变量`ming`指向它。

注意区分`Person ming`是定义`Person`类型的**变量**`ming`，而`new Person()`是创建`Person`**实例**

- ming 表示对象变量，类型是Person，注意对象变量并没有实际包含一个对象，而仅仅引用一个对象
- new Person() 构造了一个Person()类型的对象，并且它的值是对新创建对象的引用，这个引用储存在ming中。
- 可以将java对象变量理解为C++中的对象指针。

有了指向这个实例的变量，我们就可以通过这个变量来操作实例。访问实例变量可以用`变量.字段`，例如：

```
ming.name = "Xiao Ming"; // 对字段name赋值
ming.age = 12; // 对字段age赋值
System.out.println(ming.name); // 访问字段name

Person hong = new Person();
hong.name = "Xiao Hong";
hong.age = 15;
```

上述两个变量分别指向两个不同的实例，它们在内存中的结构如下：

```ascii
            ┌──────────────────┐
ming ──────>│Person instance   │
            ├──────────────────┤
            │name = "Xiao Ming"│
            │age = 12          │
            └──────────────────┘
            ┌──────────────────┐
hong ──────>│Person instance   │
            ├──────────────────┤
            │name = "Xiao Hong"│
            │age = 15          │
            └──────────────────┘
```

**两个`instance`拥有`class`定义的`name`和`age`字段，且各自都有一份独立的数据，互不干扰。**

练习：

请定义一个City类，该class具有如下字段:

- name: 名称，String类型
- latitude: 纬度，double类型
- longitude: 经度，double类型

~~~java
class City{
    public String name;
    public double latitude;
    public double longitude;
}

public class Main{
    public static void main(String[] args){
        City beijing=new City();
        beijing.name="beijing";
        beijing.latitude=39.903;
        beijing.longitude=116.401;
        
    }
}
~~~

### 方法

一个`class`可以包含多个`field`，例如，我们给`Person`类就定义了两个`field`：

```java
class Person {
    public String name;
    public int age;
}
```

但是，直接把`field`用`public`暴露给外部可能会破坏封装性。比如，代码可以这样写：

```java
Person ming = new Person();
ming.name = "Xiao Ming";
ming.age = -99; // age设置为负数 
```

显然，直接操作`field`，容易造成逻辑混乱。为了避免外部代码直接去访问`field`，我们可以用`private`修饰`field`，拒绝外部访问：

```java
class Person {
    private String name;
    private int age;
}
```

把`field`从`public`改成`private`，外部代码不能访问这些`field`，那我们定义这些`field`有什么用？怎么才能给它赋值？怎么才能读取它的值？

**所以我们需要使用方法（`method`）来让外部代码可以间接修改`field`**：

~~~java
public class Main{
    public static void main(String[] args){
        Person ming=new Person;
        ming.setname("xiaoming");
        ming.setAge(12);
        
    }
}

class Person{
    private String name;
    private int age;
    
    public getname(){//读方法getter
        return this.name;
    }
    public void setname(String name){//写方法setter
        this name=name;
    }
    
    public getage(){
        return this.age;
    }
    
    public void setAge(int age){
        this age=age;
    }
    
}
~~~



#### 定义方法

```
修饰符 方法返回类型 方法名(方法参数列表) {
    若干方法语句;
    return 方法返回值;
}
```

方法返回值通过`return`语句实现，如果没有返回值，返回类型设置为`void`，可以省略`return`。

#### private方法

有`public`方法，自然就有`private`方法。和`private`字段一样，`private`方法不允许外部调用，那我们定义`private`方法有什么用？

**定义`private`方法的理由是内部方法是可以调用`private`方法**的。例如：

~~~java
public class Main{
    public static void main(String[] args){
        Person ming=new Penson();
        ming.setBirth(2008);
        System.out.println(ming.getAge());
    }
}

class Person{
    private String name;
    private int birth;
    
    public void setBirth(int birth){
        this.birth=birth;
    }
    
    public int getAge(){
        return calcAge(2019);//调用private方法
    }
    
    //private方法
    private int calcAge(int currentYear){
        return currentYear-this.birth;
    }
}
~~~

观察上述代码，`calcAge()`是一个`private`方法，外部代码无法调用，但是，内部方法`getAge()`可以调用它。

此外，我们还注意到，**这个`Person`类只定义了`birth`字段，没有定义`age`字段，获取`age`时，通过方法`getAge()`返回的是一个实时计算的值**，并非存储在某个字段的值。这说明方法可以封装一个类的对外接口，调用方不需要知道也不关心`Person`实例在内部到底有没有`age`字段。

#### this变量

this的两个用途：

- 引用隐式参数
- 调用该类的其他构造方法

- this 表示当前实例filed

在方法内部，可以使用一个隐含的变量`this`，**它始终指向当前实例**。因此，通过`this.field`就可以访问当前实例的field。

如果没有命名冲突，可以省略`this`。例如：

```java
class Person {
    private String name;

    public String getName() {
        return name; // 相当于this.name
    }
}
```

但是，如果有局部变量和字段重名，那么局部变量优先级更高，就必须加上`this`：

```java
class Person {
    private String name;

    public void setName(String name) {
        this.name = name; // 前面的this不可少，少了就变成局部变量name了
    }
}
```



#### 方法参数

方法可以包含0个或任意个参数。**方法参数用于接收传递给方法的变量值**。调用方法时，必须严格按照参数的定义一一传递。例如：

```java
class Person {
    ...
    public void setNameAndAge(String name, int age) {
        ...
    }
}
```

调用这个`setNameAndAge()`方法时，必须有两个参数，且第一个参数必须为`String`，第二个参数必须为`int`：

```java
Person ming = new Person();
ming.setNameAndAge("Xiao Ming"); // 编译错误：参数个数不对
ming.setNameAndAge(12, "Xiao Ming"); // 编译错误：参数类型不对
ming.setNameAndAge("Xiao Ming",12);//正确
```

#### 可变参数

可变参数用`类型...`定义，可变参数相当于数组类型：

```java
class Group {
    private String[] names;

    public void setNames(String... names) {
        this.names = names;
    }
}
```

上面的`setNames()`就定义了一个可变参数。调用时，可以这么写：

```java
Group g = new Group();
g.setNames("Xiao Ming", "Xiao Hong", "Xiao Jun"); // 传入3个String
g.setNames("Xiao Ming", "Xiao Hong"); // 传入2个String
g.setNames("Xiao Ming"); // 传入1个String
g.setNames(); // 传入0个String
```

#### 参数绑定

调用方把参数传递给实例方法时，调用时传递的值会按参数位置一一绑定。

那什么是参数绑定？

我们先观察一个基本类型参数的传递：

~~~java
public class Main {
    public static void main(String[] args) {
        Person p = new Person();
        int n = 15; // n的值为15
        p.setAge(n); // 传入n的值
        System.out.println(p.getAge()); // 15
        n = 20; // n的值改为20
        System.out.println(p.getAge()); // 15
    }
}

class Person {
    private int age;

    public int getAge() {
        return this.age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
~~~

修改外部的局部变量`n`，不影响实例`p`的`age`字段，原因是`setAge()`方法获得的参数，复制了`n`的值，因此，**`p.age`和局部变量`n`互不影响**。

结论：**基本类型参数的传递，是调用方值的复制。双方各自的后续修改，互不影响。**（注意字符是基本类型，字符串是引用类型）

~~~java
public class Main {
    public static void main(String[] args) {
        Person p = new Person();
        String[] fullname ={ "Homer", "Simpson" };
        p.setName(fullname); // 传入fullname数组//传入的是数组的指向
        System.out.println(p.getName()); // "Homer Simpson"
        fullname[0] = "Bart"; // fullname数组的第一个元素修改为"Bart"
        System.out.println(p.getName()); // "Bart Simpson"
    }
}

class Person {
    private String[] name;

    public String getName() {
        return this.name[0] + " " + this.name[1];
    }

    public void setName(String[] name) {
        this.name = name;
    }
}

~~~

注意到`setName()`的参数现在是一个数组。一开始，把`fullname`数组传进去，然后，修改`fullname`数组的内容，结果发现，实例`p`的字段`p.name`也被修改了！

结论：**引用类型参数的传递，调用方的变量，和接收方的参数变量，指向的是同一个对象。双方任意一方对这个对象的修改，都会影响对方（因为指向同一个对象嘛）**。

注意：

~~~java
public class Main {
    public static void main(String[] args) {
        Person p = new Person();
        String bob = "Bob";
        p.setName(bob); // 传入bob变量
        System.out.println(p.getName()); // "Bob"
        bob = "Alice"; // bob改名为Alice
        System.out.println(p.getName()); // 依然是"Bob"
    }
}

class Person {
    private String name;

    public String getName() {
        return this.name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

~~~



```
1、整数、浮点数、字符是基本类型。
2、字符串、数组是引用类型（内存数据的索引）

3、基本类型参数的传递，是调用方值的复制。双方各自的后续修改，互不影响。
4、引用类型参数的传递，调用方的变量和接收方的参数变量，指向的是同一个对象。双方任意一方对这个对象的修改，都会影响对方。

那么3个例子中，
1、整数的参数传递理解了，复制出来的，分家了，自己管理自己的，类读出数据不变。
2、字符串数组的参数传递也理解了，都是指向同一个地方，数组的一个元素改了，类读出数据也就变了（类一直指向这里）。
3、字符串也是引用参数，为什么类读出数据不变？因为重写了整个字符串（新开内存和指向，参看字符串更改章节），类依然指向之前内存块，类读出数据不变，同结论1。如果只是修改字符串内存中某一个字符的值，则同结论2。

简单总结：类对基本类型是复制数据本身，新开内存。对引用类型是复制指向地址，内存数据本身变化了，类读出数据跟着变化。但字符串修改，是新开内存新指向，已经不能影响类数据。
```



练习：

~~~java
public class Main {
    public static void main(String[] args) {
        Person ming = new Person();
        ming.setName("小明");
        ming.setAge(12);
        System.out.println(ming.getAge());
    }
}

class Person {
    private String name;
private int age;

public int getAge(){
return this.age;
}

public void setAge(int age){
this.age=age;
}

    public String getName() {
        return this.name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

~~~

### 构造方法

- 构造方法与类同名
- 每个类可以有一个以上的构造方法
- 构造方法可以有0个，1个或多个参数
- 构造器没有返回值
- 构造器总是伴随new操作一起调用



能否<u>在创建对象实例时就把内部字段全部初始化为合适的值</u>？

完全可以。

这时，我们就需要**构造方法**。

创建实例的时候，实际上是**通过构造方法来初始化实例**的。我们先来定义一个构造方法，能在创建`Person`实例的时候，一次性传入`name`和`age`，完成初始化：

~~~java
public class hello{
    public static void main(String[] args){
        Person p=new Person("xiaoming",16);//在创建实例时，直接完成初始化
        System.out.println(p.getName());
        System.out.println(p.getAge());
    }
}

class Person{
    private String name;
    private int age;

    public Person(String name,int age){//构造方法
        this.name=name;
        this.age=age;
    }

    public String getName(){
        return this.name;
    }

    public int getAge(){
        return this.age;
    }
}
~~~

由于构造方法是如此特殊，所以构造方法的名称就是类名。构造方法的参数没有限制，在方法内部，也可以编写任意语句。但是，和普通方法相比，构造方法没有返回值（也没有`void`），调用构造方法，必须用`new`操作符。

#### 默认构造方法

是不是任何`class`都有构造方法？是的。

那前面我们并没有为`Person`类编写构造方法，为什么可以调用`new Person()`？

原因是<u>如果一个类没有定义构造方法，编译器会自动为我们生成一个默认构造方法</u>，它没有参数，也没有执行语句，类似这样：

```java
class Person {
    public Person() {
    }
}
```

要特别注意的是，如果我们自定义了一个构造方法，那么，编译器就*不再*自动创建默认构造方法.

在Java中，创建对象实例的时候，按照如下顺序进行初始化：

1. 先初始化字段，例如，`int age = 10;`表示字段初始化为`10`，`double salary;`表示字段默认初始化为`0`，`String name;`表示引用类型字段默认初始化为`null`；
2. 执行构造方法的代码进行初始化。

#### 多构造方法

可以定义多个构造方法，在通过`new`操作符调用的时候，编译器通过构造方法的参数数量、位置和类型自动区分：

~~~java
class Person{
    private String name;
    private int age;
    
    public Person(String name,int age){
        this.name=name;
        this.age=age;
    }
    
    public Person(String name){
        this.name=name;
    }
    
    public Person(){
        
    }
}
~~~

如果调用`new Person("Xiao Ming", 20);`，会自动匹配到构造方法`public Person(String, int)`。

如果调用`new Person("Xiao Ming");`，会自动匹配到构造方法`public Person(String)`。

如果调用`new Person();`，会自动匹配到构造方法`public Person()`。

**一个构造方法可以调用其他构造方法**，这样做的目的是便于代码复用。调用其他构造方法的语法是`this(…)`：

~~~java
class Person{
    private String name;
    private int age;
    
    public Person(String name;int age){
        this.name=name;
        this.age=age;
    }
    
    public Person(String name){
        this(name,16);//调用另一个构造方法Person(String,int)
    }
    
    public Person(){
        this("unnamed");//调用另一个构造方法Person(String)
    }
}
~~~

练习：

请给`Person`类增加`(String, int)`的构造方法：

~~~java
public class Main{
    public static void main(String[] args){
    	Person p=new Person("xiaoming",16);
        System.out.println(p.getName());
        System.out.println(p.getAge());
        
        
    }
}

class Person{
    private String name;
    private int age;
    
    public Person(String name,int age){
        this.name=name;
        this.age=age;
    }
    
    public getName(){
        return this.name;
    }
    
    public getAge(){
        return this.age;
    }
}
~~~

### 方法重载

在一个类中，我们可以定义多个方法。如果有一系列方法，它们的功能都是类似的，只有参数有所不同，那么，可以把这一组方法名做成*同名*方法。例如，在`Hello`类中，定义多个`hello()`方法：

```java
class Hello {
    public void hello() {
        System.out.println("Hello, world!");
    }

    public void hello(String name) {
        System.out.println("Hello, " + name + "!");
    }

    public void hello(String name, int age) {
        if (age < 18) {
            System.out.println("Hi, " + name + "!");
        } else {
            System.out.println("Hello, " + name + "!");
        }
    }
}
```

**这种方法名相同，但各自的参数不同，称为方法重载**（`Overload`）。

练习：

~~~java
public class hello{
    public static void main(String[] args){
        Person ming=new Person();
        Person hong=new Person();
        ming.setName("ming");
        hong.setname("xiao","hong");
        System.out.println(ming.getName());
        System.out.println(hong.getName());

    }
}

class Person{
    private String name;
    private String name1;

    public String getName() {
        return this.name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void setname(String name,String name1){//方法重载
        this.name=name+" "+name1;
    }
}
~~~

### 继承

- 一个子类只能够继承唯一一个父类

继承是面向对象编程中非常强大的一种机制，它首先可以复用代码。当我们让`Student`从`Person`继承时，`Student`就获得了`Person`的所有功能，我们只需要为`Student`编写新增的功能。

Java使用`extends`关键字来实现继承：

~~~java
class Person {
    private String name;
    private int age;

    public String getName() {...}
    public void setName(String name) {...}
    public int getAge() {...}
    public void setAge(int age) {...}
}

class Student extends Person {
    // 不要重复name和age字段/方法,
    // 只需要定义新增score字段/方法:
    private int score;

    public int getScore() { … }
    public void setScore(int score) { … }
}
~~~

可见，通过继承，`Student`只需要编写额外的功能，不再需要重复代码。

 **注意：子类自动获得了父类的所有字段，严禁定义与父类重名的字段！**

#### 继承树

注意到我们在定义`Person`的时候，没有写`extends`。在Java中，没有明确写`extends`的类，编译器会自动加上`extends Object`。所以，任何类，除了`Object`，都会继承自某个类。下图是`Person`、`Student`的继承树：

```ascii
┌───────────┐
│  Object   │
└───────────┘
      ▲
      │
┌───────────┐
│  Person   │
└───────────┘
      ▲
      │
┌───────────┐
│  Student  │
└───────────┘
```

**Java只允许一个class继承自一个类，因此，一个类有且仅有一个父类**。只有`Object`特殊，它没有父类。

类似的，如果我们定义一个继承自`Person`的`Teacher`，它们的继承树关系如下：

```ascii
       ┌───────────┐
       │  Object   │
       └───────────┘
             ▲
             │
       ┌───────────┐
       │  Person   │
       └───────────┘
          ▲     ▲
          │     │
          │     │
┌───────────┐ ┌───────────┐
│  Student  │ │  Teacher  │
└───────────┘ └───────────┘
```

#### protected

继承有个特点，就是子类无法访问父类的`private`字段或者`private`方法。例如，`Student`类就无法访问`Person`类的`name`和`age`字段：

```java
class Person {
    private String name;
    private int age;
}

class Student extends Person {
    public String hello() {
        return "Hello, " + name; // 编译错误：无法访问name字段
    }
}
```

这使得继承的作用被削弱了。为了让子类可以访问父类的字段，我们需要把`private`改为`protected`。**用`protected`修饰的字段可以被子类访问**：

```java
class Person {
    protected String name;
    protected int age;
}

class Student extends Person {
    public String hello() {
        return "Hello, " + name; // OK!
    }
}
```

因此，**`protected`关键字可以把字段和方法的访问权限控制在继承树内部，一个`protected`字段和方法可以被其子类，以及子类的子类所访问**。

#### super

super的两个用途：

- 调用父类的方法
- 调用父类的构造方法（必须放在子类构造方法的第一条语句）

`super`关键字表示父类（超类）。**子类引用父类的字段时，可以用`super.fieldName`**。例如：

```java
class Student extends Person {
    public String hello() {
        return "Hello, " + super.name;
    }
}
```

实际上，这里使用`super.name`，或者`this.name`，或者`name`，效果都是一样的。编译器会自动定位到父类的`name`字段。

但是，在某些时候，就必须使用`super`。我们来看一个例子：

~~~java
public class Main {
    public static void main(String[] args) {
        Student s = new Student("Xiao Ming", 12, 89);
    }
}

class Person {
    protected String name;
    protected int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

class Student extends Person {
    protected int score;

    public Student(String name, int age, int score) {//构造方法
        this.score = score;
    }
}
~~~

运行上面的代码，会得到一个编译错误，大意是在`Student`的构造方法中，无法调用`Person`的构造方法。

这是因为在Java中，**任何`class`的构造方法，第一行语句必须是调用父类的构造方法。如果没有明确地调用父类的构造方法，编译器会帮我们自动加一句`super();`，所以，`Student`类的构造方法实际上是这样：**

```java
class Student extends Person {
    protected int score;

    public Student(String name, int age, int score) {
        super(); // 自动调用父类的构造方法
        this.score = score;
    }
}
```

但是，**`Person`类并没有无参数的构造方法**，因此，编译失败。

解决方法是调用`Person`类存在的某个构造方法。例如：

```java
class Student extends Person {
    protected int score;

    public Student(String name, int age, int score) {
        super(name, age); // 调用父类的构造方法Person(String, int)
        this.score = score;
    }
}
```

这样就可以正常编译了！

因此我们得出结论：**如果父类没有默认的构造方法，子类就必须显式调用`super()`并给出参数以便让编译器定位到父类的一个合适的构造方法**。

这里还顺带引出了另一个问题：即**子类*不会继承*任何父类的构造方法。子类默认的构造方法是编译器自动生成的，不是继承的。**

#### 向上转型

- 父类变量可以引用父量对象，也可以引用任意一个子类对象

**子类类型转换为父类类型**

因为子类的功能比父类多，所以向上转型是可以的。

如果一个引用变量的类型是`Student`，那么它可以指向一个`Student`类型的实例：

```
Student s = new Student();
```

如果一个引用类型的变量是`Person`，那么它可以指向一个`Person`类型的实例：

```
Person p = new Person();
```

现在问题来了：如果`Student`是从`Person`继承下来的，那么，一个引用类型为`Person`的变量，能否指向`Student`类型的实例？

```
Person p = new Student(); // ???
```

测试一下就可以发现，**这种指向是允许的！**

<u>这是因为`Student`继承自`Person`，因此，它拥有`Person`的全部功能。`Person`类型的变量，如果指向`Student`类型的实例，对它进行操作，是没有问题的！</u>

这种把一个子类类型安全地变为父类类型的赋值，被称为向上转型（upcasting）。

向上转型实际上是把一个子类型安全地变为更加抽象的父类型：

```
Student s = new Student();
Person p = s; // upcasting, ok
Object o1 = p; // upcasting, ok
Object o2 = s; // upcasting, ok
```

注意到继承树是`Student > Person > Object`，所以，可以把`Student`类型转型为`Person`，或者更高层次的`Object`。

#### 向下转型

**父类类型转换为子类类型**

```
Person p1 = new Student(); // upcasting, ok
Person p2 = new Person();
Student s1 = (Student) p1; // ok
Student s2 = (Student) p2; // runtime error! ClassCastException!
```

如果测试上面的代码，可以发现：

`Person`类型`p1`实际指向`Student`实例，`Person`类型变量`p2`实际指向`Person`实例。在向下转型的时候，把`p1`转型为`Student`会成功，因为`p1`确实指向`Student`实例，把`p2`转型为`Student`会失败，因为`p2`的实际类型是`Person`，**不能把父类变为子类，因为子类功能比父类多，多的功能无法凭空变出来**。

因此，**向下转型很可能会失败。失败的时候，Java虚拟机会报`ClassCastException`。**

为了避免向下转型出错，Java提供了`instanceof`操作符，可以先判断一个实例究竟是不是某种类型：

```java
Person p = new Person();
System.out.println(p instanceof Person); // true
System.out.println(p instanceof Student); // false

Student s = new Student();
System.out.println(s instanceof Person); // true
System.out.println(s instanceof Student); // true

Student n = null;
System.out.println(n instanceof Student); // false
```

**`instanceof`实际上判断一个变量所指向的实例是否是指定类型**，或者这个类型的子类。如果一个引用变量为`null`，那么对任何`instanceof`的判断都为`false`。

利用`instanceof`，在向下转型前可以先判断：

```java
Person p = new Student();
if (p instanceof Student) {
    // 只有判断成功才会向下转型:
    Student s = (Student) p; // 一定会成功
}
```

从Java 14开始，判断`instanceof`后，可以直接转型为指定变量，避免再次强制转型。例如，对于以下代码：

```java
Object obj = "hello";
if (obj instanceof String) {
    String s = (String) obj;
    System.out.println(s.toUpperCase());
}
```

#### 区别继承和组合

在使用继承时，我们要注意逻辑一致性。

考察下面的`Book`类：

```java
class Book {
    protected String name;
    public String getName() {...}
    public void setName(String name) {...}
}
```

这个`Book`类也有`name`字段，那么，我们能不能让`Student`继承自`Book`呢？

```java
class Student extends Book {
    protected int score;
}
```

显然，从逻辑上讲，这是不合理的，`Student`不应该从`Book`继承，而应该从`Person`继承。

究其原因，是因为`Student`是`Person`的一种，它们是is关系，而`Student`并不是`Book`。实际上`Student`和`Book`的关系是has关系。

具有has关系不应该使用继承，而是使用组合，即`Student`可以持有一个`Book`实例：

```java
class Student extends Person {
    protected Book book;//组合关系，通过引用BooK类，可以调用BooK中属性和方法
    protected int score;
}
```

因此，继承是is关系，组合是has关系。

练习：

定义`PrimaryStudent`，从`Student`继承，并新增一个`grade`字段：

~~~java
package demo;

import java.util.Arrays;
public class hello{
    public static void main(String[] args){
        Person p=new Person("xiaoming",13);
        Student s=new Student("xiaohong",12,88);
        PrimaryStudebt t=new PrimaryStudebt("xiaohua",16,99,1);
        System.out.println(t.getGrade());
//        t.getGrade();
    }
}

class Person{
    protected String name;
    protected int age;

    public Person(String name,int age){
        this.name=name;
        this.age=age;
    }

    public String getName(){
        return name;
    }

//    public void setName(String name) {
//        this.name = name;
//    }

    public int getAge() {
        return age;
    }

//    public void setAge(int age) {
//        this.age = age;
//    }
}

class Student extends Person{
    protected int score;

    public Student(String name, int age,int score) {//第一行语句必须是调用父类的构造方法
        super(name, age);
        this.score=score;
    }

    public int getScore() {
        return score;
    }
}

class PrimaryStudebt extends Student{
    protected int grade;

    public PrimaryStudebt(String name, int age, int score,int grade) {
        super(name, age, score);
        this.grade=grade;
        }

    public int getGrade() {
        return grade;
    }
//
//    public void getGrade(String name,int age,int score,int grade){
//        System.out.printf("name:%s,age:%d,score:%d,grade:%d",name,age,score,grade);
//    }
}



~~~

### 多态

多态实现的三种充要条件：

- 继承
- 重写（覆写）父类方法
- 父类引用指向子类对象

在继承关系中，子类如果定义了一个与父类方法签名完全相同的方法，被称为覆写（Override）。

例如，在`Person`类中，我们定义了`run()`方法：

```java
class Person {
    public void run() {
        System.out.println("Person.run");
    }
}
```

在子类`Student`中，覆写这个`run()`方法：

```java
class Student extends Person {
    @Override
    public void run() {
        System.out.println("Student.run");
    }
}
```

如果方法签名相同，并且返回值也相同，就是`Override`。

<u>加上`@Override`可以让编译器帮助检查是否进行了正确的覆写。希望进行覆写，但是不小心写错了方法签名，编译器会报错。</u>

现在，我们考虑一种情况，如果子类覆写了父类的方法：

```java
// override 
public class Main {
    public static void main(String[] args) {
        Person p = new Student();
        p.run(); // 应该打印Student.run
    }
}

class Person {
    public void run() {
        System.out.println("Person.run");
    }
}

class Student extends Person {
    @Override
    public void run() {
        System.out.println("Student.run");
    }
}
```

Student.run 

那么，一个实际类型为`Student`，引用类型为`Person`的变量，调用其`run()`方法，调用的是`Person`还是`Student`的`run()`方法？

运行一下上面的代码就可以知道，实际上调用的方法是`Student`的`run()`方法。因此可得出结论：

**Java的实例方法调用是基于运行时的实际类型的动态调用，而非变量的声明类型。**

这个非常重要的特性在面向对象编程中称之为多态。它的英文拼写非常复杂：Polymorphic。

---



**多态是指，针对某个类型的方法调用，其真正执行的方法取决于运行时期实际类型的方法**。例如：

```java
Person p = new Student();
p.run(); // 调用Student run()方法
```



<u>多态的特性就是，运行期才能动态决定调用的子类方法</u>。对某个类型调用某个方法，执行的实际方法可能是某个子类的覆写方法。这种不确定性的方法调用，究竟有什么作用？

我们还是来举栗子。

假设我们定义一种收入，需要给它报税，那么先定义一个`Income`类：

~~~java
class Income{
    protected double income;
    public double getTax(){
        return income*0.1;
    }
}
~~~

对于工资收入，可以减去一个基数，那么我们可以从`Income`派生出`SalaryIncome`，并覆写`getTax()`：

~~~java
class Salay extends Income{
    @override
    public double getTax(){
        if (income<=5000){
            return 0;
        }
        return (income-5000)*0.2;
    }
}
~~~

如果你享受国务院特殊津贴，那么按照规定，可以全部免税：

```java
class StateCouncilSpecialAllowance extends Income {
    @Override
    public double getTax() {
        return 0;
    }
}
```

现在，我们要编写一个报税的财务软件，对于一个人的所有收入进行报税，可以这么写：

```java
public double totalTax(Income... incomes) {
    double total = 0;
    for (Income income: incomes) {
        total = total + income.getTax();
    }
    return total;
}
```

来试一下：

~~~java
public class Main {
    public static void main(String[] args) {
        // 给一个有普通收入、工资收入和享受国务院特殊津贴的小伙伴算税:
        Income[] incomes = new Income[] {
            new Income(3000),
            new Salary(7500),
            new StateCouncilSpecialAllowance(15000)
        };
        System.out.println(totalTax(incomes));
    }

    public static double totalTax(Income... incomes) {//该方法传入的参数是可变参数
        double total = 0;
        for (Income income: incomes) {
            total = total + income.getTax();
        }
        return total;
    }
}

class Income {
    protected double income;

    public Income(double income) {
        this.income = income;
    }

    public double getTax() {
        return income * 0.1; // 税率10%
    }
}

class Salary extends Income {
    public Salary(double income) {
        super(income);
    }

    @Override
    public double getTax() {
        if (income <= 5000) {
            return 0;
        }
        return (income - 5000) * 0.2;
    }
}

class StateCouncilSpecialAllowance extends Income {
    public StateCouncilSpecialAllowance(double income) {
        super(income);
    }

    @Override
    public double getTax() {
        return 0;
    }
}

~~~

观察`totalTax()`方法：利用多态，`totalTax()`方法只需要和`Income`打交道，它完全不需要知道`Salary`和`StateCouncilSpecialAllowance`的存在，就可以正确计算出总的税。如果我们要新增一种稿费收入，只需要从`Income`派生，然后正确覆写`getTax()`方法就可以。把新的类型传入`totalTax()`，不需要修改任何代码。

可见，多态具有一个非常强大的功能，就是允许添加更多类型的子类实现功能扩展，却不需要修改基于父类的代码。

#### 覆写Object方法

因为所有的`class`最终都继承自`Object`，而`Object`定义了几个重要的方法：

- `toString()`：把instance输出为`String`；
- `equals()`：判断两个instance是否逻辑相等；
- `hashCode()`：计算一个instance的哈希值。

在必要的情况下，我们可以覆写`Object`的这几个方法。例如：

```java
class Person {
    ...
    // 显示更有意义的字符串:
    @Override
    public String toString() {
        return "Person:name=" + name;
    }

    // 比较是否相等:
    @Override
    public boolean equals(Object o) {
        // 当且仅当o为Person类型:
        if (o instanceof Person) {
            Person p = (Person) o;
            // 并且name字段相同时，返回true:
            return this.name.equals(p.name);
        }
        return false;
    }

    // 计算hash:
    @Override
    public int hashCode() {
        return this.name.hashCode();
    }
}
```

#### 调用super

super的两个用途：

- 调用父类的方法
- 调用父类的构造方法（必须放在子类构造方法的第一条语句）

在子类的覆写方法中，如果要调用父类的被覆写的方法，可以通过`super`来调用。例如：

```java
class Person {
    protected String name;
    public String hello() {
        return "Hello, " + name;
    }
}

Student extends Person {
    @Override
    public String hello() {
        // 调用父类的hello()方法:
        return super.hello() + "!";
    }
}
```

#### final

- **用`final`修饰的字段在初始化后不能被修改**
- **用`final`修饰的方法在初始化后不能被覆写**
- **用`final`修饰的类在初始化后不能被继承**

继承可以允许子类覆写父类的方法。**如果一个父类不允许子类对它的某个方法进行覆写，可以把该方法标记为`final`**。用`final`修饰的方法不能被`Override`：

```java
class Person {
    protected String name;
    public final String hello() {
        return "Hello, " + name;
    }
}

Student extends Person {
    // compile error: 不允许覆写
    @Override
    public String hello() {
    }
}
```

如果一个类不希望任何其他类继承自它，那么可以把这个类本身标记为`final`。用`final`修饰的类不能被继承：

```java
final class Person {
    protected String name;
}

// compile error: 不允许继承自Person
Student extends Person {
}
```

对于一个类的实例字段，同样可以用`final`修饰。**用`final`修饰的字段在初始化后不能被修改**。例如：

```
class Person {
    public final String name = "Unamed";
}
```

对`final`字段重新赋值会报错：

```
Person p = new Person();
p.name = "New Name"; // compile error!
```

可以在构造方法中初始化final字段：

```java
class Person {
    public final String name;
    public Person(String name) {
        this.name = name;
    }
}
```

这种方法更为常用，因为可以保证实例一旦创建，其`final`字段就不可修改。

练习：

给一个有工资收入和稿费收入的小伙伴算税。

~~~java
package demo;

public class hello{
    public static void main(String[] args){
        Income[] incomes=new Income[]{
                new Income(3000),
                new Salay(7500)
        };

        System.out.println(totalTex(incomes));


    }

    public static double totalTex(Income...incomes){
        double total=0;
        for (Income income:incomes){
            total=total+ income.getTax();
        }
        return total;
    }

}

class Income{
    protected double income;

    public Income(double income){
        this.income=income;
    }

    public double getTax(){
        return income*0.1;
    }
}

class Salay extends Income{
    public Salay(double income){
        super(income);
    }

    @Override
    public double getTax() {
        if (income<5000){
            return 0;
        }
        return (income-5000)*0.2;
    }
}

class State extends Income{
    public State(double income){
        super(income);
    }

    @Override
    public double getTax() {
        return 0;
    }
}


~~~

### 抽象类

如果父类的方法本身不需要实现任何功能，仅仅是为了定义方法签名，目的是让子类去覆写它，那么，可以把父类的方法声明为抽象方法：

什么是方法签名：

完整的描述一个方法需要指出**方法名以及参数类型，这叫做方法的签名**。返回类型不是方法签名的一部分。也就是说，不能有两个名字相同、参数类型也相同却返回类型不同的方法。

```java
class Person {
    public abstract void run();
}
```

把一个方法声明为`abstract`，表示它是一个抽象方法，本身没有实现任何方法语句。因为这个抽象方法本身是无法执行的，所以，`Person`类也无法被实例化。编译器会告诉我们，无法编译`Person`类，因为它包含抽象方法。

必须把`Person`类本身也声明为`abstract`，才能正确编译它：

```java
abstract class Person {
    public abstract void run();
}
```

---

如果一个`class`定义了方法，但没有具体执行代码，这个方法就是抽象方法，抽象方法用`abstract`修饰。

因为无法执行抽象方法，因此这个类也必须申明为抽象类（abstract class）。

使用`abstract`修饰的类就是抽象类。**我们无法实例化一个抽象类**：

```
Person p = new Person(); // 编译错误
```

无法实例化的抽象类有什么用？

**因为抽象类本身被设计成只能用于被继承，因此，<u>抽象类可以强迫子类实现其定义的抽象方法</u>，否则编译会报错。因此，抽象方法实际上相当于定义了“规范”。**

例如，`Person`类定义了抽象方法`run()`，那么，在实现子类`Student`的时候，就必须覆写`run()`方法：

~~~java
package demo;

public class hello{
    public static void main(String[] args){
        Person p=new Student();
        p.run();
    }
}

abstract class Person{//抽象类
    public abstract void run();//抽象方法
}

class Student extends Person{
    @Override
    public void run() {
        System.out.println("Student run");
    }
}
~~~

#### 面向抽象编程

当我们定义了抽象类`Person`，以及具体的`Student`、`Teacher`子类的时候，我们可以通过抽象类`Person`类型去引用具体的子类的实例：

```
Person s = new Student();
Person t = new Teacher();
```

这种引用抽象类的好处在于，我们对其进行方法调用，并不关心`Person`类型变量的具体子类型：

```
// 不关心Person变量的具体子类型:
s.run();
t.run();
```

同样的代码，如果引用的是一个新的子类，我们仍然不关心具体类型：

```
// 同样不关心新的子类是如何实现run()方法的：
Person e = new Employee();
e.run();
```

这种尽量引用高层类型，避免引用实际子类型的方式，称之为面向抽象编程。

面向抽象编程的本质就是：

- 上层代码只定义规范（例如：`abstract class Person`）；
- 不需要子类就可以实现业务逻辑（正常编译）；
- 具体的业务逻辑由不同的子类实现，调用者并不关心。

~~~java
package demo;

abstract class Person{
    private String name;
    private int age;

    public Person(String name,int age){
        this.name=name;
        this.age=age;
    }

    public abstract String getDescription();

}

class Student extends Person{

    public Student(String name,int age){
        super(name,age);
    }

    @Override
    public String getDescription() {
        return String.format("an student");
    }

}

public class Main{
    public static void main(String[] args){
        Person s=new Student("xiaoming",18);
        System.out.println(s.getDescription());
    }
}
~~~



练习：

用抽象类给一个有工资收入和稿费收入的小伙伴算税。

~~~java
package demo;

public class hello{
    public static void main(String[] args){
        Income[] incomes=new Income[]{
//                new Income(3000),
                new Salay(7500),
                new State(1000)
        };

        System.out.println(totalTex(incomes));


    }

    public static double totalTex(Income...incomes){
        double total=0;
        for (Income income:incomes){
            total=total+ income.getTax();
        }
        return total;
    }

}

abstract class Income{
    protected double income;

    public Income(double income){
        this.income=income;//初始化收入
    }

    public abstract double getTax();//子类必须实现的规范
}

class Salay extends Income{
    public Salay(double income){
        super(income);//必须显示调用父类的构造方法
    }

    @Override
    public double getTax() {
        if (income<5000){
            return 0;
        }
        return (income-5000)*0.2;
    }
}

class State extends Income{
    public State(double income){
        super(income);
    }

    @Override
    public double getTax() {
        return this.income*0.1;
    }
}

~~~

### 接口

- 一个类可以实现一个或多个接口
- 接口不是类，而是对类的一组需求描述，这些类要遵从接口描述的统一格式定义。

在抽象类中，抽象方法本质上是定义接口规范：即规定高层类的接口，从而保证所有子类都有相同的接口实现，这样，多态就能发挥出威力。

**如果一个抽象类没有字段，所有方法全部都是抽象方法**：

```java
abstract class Person {
    public abstract void run();
    public abstract String getName();
}
```

就可以把该抽象类改写为接口：`interface`。

在Java中，使用`interface`可以声明一个接口：

```java
interface Person {
    void run();
    String getName();
}
```

<u>所谓`interface`，就是比抽象类还要抽象的纯抽象接口，因为它连字段都不能有。因为接口定义的所有方法默认都是`public abstract`的</u>，<u>所以这两个修饰符不需要写出来</u>（写不写效果都一样）。

当一个具体的`class`去实现一个`interface`时，需要使用`implements`关键字。举个例子：

- 在接口声明中，所有方法都自动为public，不过在实现接口时，必须把方法声明为public。

~~~java
package demo;
interface Person{
    void run();
    String getName();
}

class Student implements Person{
    private String name;
    public Student(String name){
        this.name=name;
    }
    @Override
    public void run() {
        System.out.println("Student sun");
    }

    @Override
    public String getName() {
        return name;
    }
}

public class hello{
    public static void main(String[] args){
        Student s=new Student("XIAOHONG");
        s.run();
    }
}
~~~

**我们知道，在Java中，一个类只能继承自另一个类，不能从多个类继承。但是，一个类可以实现多个`interface`**，例如：

```java
class Student implements Person, Hello { // 实现了两个interface
    ...
}
```

#### 术语

注意区分术语：

Java的接口特指`interface`的定义，表示一个接口类型和一组方法签名，而编程接口泛指接口规范，如方法签名，数据格式，网络协议等。

抽象类和接口的对比如下：

|            | abstract class       | interface                   |
| ---------- | :------------------- | :-------------------------- |
| 继承       | 只能extends一个class | 可以implements多个interface |
| 字段       | 可以定义实例字段     | **不能定义实例字段**        |
| 抽象方法   | 可以定义抽象方法     | 可以定义抽象方法            |
| 非抽象方法 | 可以定义非抽象方法   | 可以定义default方法         |

### 

#### 接口继承

一个`interface`可以继承自另一个`interface`。`interface`继承自`interface`使用`extends`，它相当于扩展了接口的方法。例如：

```java
interface Hello {
    void hello();
}

interface Person extends Hello {
    void run();
    String getName();
}
```

此时，`Person`接口继承自`Hello`接口，因此，`Person`接口现在实际上有3个抽象方法签名，其中一个来自继承的`Hello`接口。

#### 继承关系

合理设计`interface`和`abstract class`的继承关系，可以充分复用代码。一般来说，公共逻辑适合放在`abstract class`中，具体逻辑放到各个子类，而接口层次代表抽象程度。可以参考Java的集合类定义的一组接口、抽象类以及具体子类的继承关系：

```ascii
┌───────────────┐
│   Iterable    │
└───────────────┘
        ▲                ┌───────────────────┐
        │                │      Object       │
┌───────────────┐        └───────────────────┘
│  Collection   │                  ▲
└───────────────┘                  │
        ▲     ▲          ┌───────────────────┐
        │     └──────────│AbstractCollection │
┌───────────────┐        └───────────────────┘
│     List      │                  ▲
└───────────────┘                  │
              ▲          ┌───────────────────┐
              └──────────│   AbstractList    │
                         └───────────────────┘
                                ▲     ▲
                                │     │
                                │     │
                     ┌────────────┐ ┌────────────┐
                     │ ArrayList  │ │ LinkedList │
                     └────────────┘ └────────────┘
```

**在使用的时候，实例化的对象永远只能是某个具体的子类，但总是通过接口去引用它**，因为接口比抽象类更抽象：

```java
List list = new ArrayList(); // 用List接口引用具体子类的实例
Collection coll = list; // 向上转型为Collection接口
Iterable it = coll; // 向上转型为Iterable接口
```

#### default方法

在接口中，可以定义`default`方法。例如，把`Person`接口的`run()`方法改为`default`方法：

~~~java
public class Main {
    public static void main(String[] args) {
        Person p = new Student("Xiao Ming");
        p.run();
    }
}

interface Person {
    String getName();
    default void run() {
        System.out.println(getName() + " run");
    }
}

class Student implements Person {
    private String name;

    public Student(String name) {
        this.name = name;
    }

    public String getName() {
        return this.name;
    }
}
//Xiao Ming run
~~~

**实现类可以不必覆写`default`方法。**`default`方法的目的是，当我们需要给接口新增一个方法时，会涉及到修改全部子类。如果新增的是`default`方法，那么子类就不必全部修改，只需要在需要覆写的地方去覆写新增方法。

`default`方法和抽象类的普通方法是有所不同的。因为`interface`没有字段，`default`方法无法访问字段，而抽象类的普通方法可以访问实例字段。

~~~java
package demo;

public class hello{
    public static void main(String[] args){
        System.out.println(totalTex());
    }

    public static double totalTex(){
        Salay s =new Salay(7500);
        State s1=new State(10000);
        return s.getTex()+ s1.getTex();
    }

}


interface Income{//定义接口
    double getTex();//子类必须实现的规范
}


class Salay implements Income{
    private double income;

    public Salay(double income){
        this.income=income;
    }

    @Override
    public double getTex() {
        if (income<5000){
            return 0;
        }
        return (income-5000)*0.2;
    }
}

class State implements Income{
    private double income;
    public State(double income){
        this.income=income;
    }

    @Override
    public double getTex() {
        return this.income*0.1;
    }
}

~~~

### 静态字段和静态方法

~~~java
class Employee{
    private static int nextId=1;//静态域
    private int id;//实例域
}
~~~

每一个雇员对象都有一个自己的id域，但是这个类的所有实例将共享一个nextId域。换句话说，如果有1000个Employee类的对象，则有1000个实例域id。但是，只有一个静态域nextId。即使没有一个Employee类的对象，静态域nextId也存在，**它属于类，而不属于任何独立的对象。**



---



在一个`class`中定义的字段，我们称之为实例字段。实例字段的特点是，每个实例都有独立的字段，各个实例的同名字段互不影响。

还有一种字段，是用`static`修饰的字段，称为静态字段：`static field`。

<u>实例字段在每个实例中都有自己的一个独立“空间”，但是静态字段只有一个共享“空间”，所有实例都会共享该字段</u>。举个例子：

~~~java
class Person{
	public String name;
    public int age;
    public static int number;//定义静态字段
}
~~~

我们来看看下面的代码：

~~~java
public class Main {
    public static void main(String[] args) {
        Person ming = new Person("Xiao Ming", 12);
        Person hong = new Person("Xiao Hong", 15);
        ming.number = 88;
        System.out.println(hong.number);
        hong.number = 99;
        System.out.println(ming.number);
    }
}

class Person {
    public String name;
    public int age;

    public static int number;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
//88
//99 
~~~

**对于静态字段，无论修改哪个实例的静态字段，效果都是一样的：所有实例的静态字段都被修改了，原因是静态字段并不属于实例**：

```ascii
ming ──>│Person instance   │
        ├──────────────────┤
        │name = "Xiao Ming"│
        │age = 12          │
        │number ───────────┼──┐    ┌─────────────┐
        └──────────────────┘  │    │Person class │
                              │    ├─────────────┤
                              ├───>│number = 99  │
        ┌──────────────────┐  │    └─────────────┘
hong ──>│Person instance   │  │
        ├──────────────────┤  │
        │name = "Xiao Hong"│  │
        │age = 15          │  │
        │number ───────────┼──┘
        └──────────────────┘
```

虽然实例可以访问静态字段，但是它们指向的其实都是`Person class`的静态字段。所以，**所有实例共享一个静态字段**。

因此，不推荐用`实例变量.静态字段`去访问静态字段，因为在Java程序中，实例对象并没有静态字段。在代码中，实例对象能访问静态字段只是因为编译器可以根据实例类型自动转换为`类名.静态字段`来访问静态对象。

**推荐用类名来访问静态字段**。可以把静态字段理解为描述`class`本身的字段（非实例字段）。对于上面的代码，更好的写法是：

```
Person.number = 99;
System.out.println(Person.number);
```

#### 静态方法

- 静态方法通过类名调用，静态方法只能访问静态字段

有静态字段，就有静态方法。用`static`修饰的方法称为静态方法。

调用实例方法必须通过一个实例变量，而**调用静态方法则不需要实例变量，通过类名就可以调用**。静态方法类似其它编程语言的函数。例如：

~~~java
public class Main{
    public static void main(String[] args){
        Person.setNumber(99);//通过类名调用
        System.out.println(Person.number);//通过类名.静态字段来调用
    }
}

class Person{
    public static int number;
    
    public static void setNumber(int value){
        number=value;
    }
}
~~~

因为<u>静态方法属于`class`而不属于实例</u>，因此，静态方法内部，无法访问`this`变量，也无法访问实例字段，它**只能访问静态字段。**

通过实例变量也可以调用静态方法，但这只是编译器自动帮我们把实例改写成类名而已。

通常情况下，通过实例变量访问静态字段和静态方法，会得到一个编译警告。

静态方法经常用于工具类。例如：

- Arrays.sort()
- Math.random()

**静态方法也经常用于辅助方法。注意到Java程序的入口`main()`也是静态方法。**

#### 接口的静态字段

因为`interface`是一个纯抽象类，所以它不能定义实例字段。但是，`interface`是可以有静态字段的，并且静态字段必须为`final`类型：

```java
public interface Person {
    public static final int MALE = 1;
    public static final int FEMALE = 2;
}
```

实际上，因为`interface`的字段只能是`public static final`类型，所以我们可以把这些修饰符都去掉，上述代码可以简写为：

```java
public interface Person {
    // 编译器会自动加上public statc final:
    int MALE = 1;
    int FEMALE = 2;
}
```

编译器会自动把该字段变为`public static final`类型。

练习：

给Person类增加一个静态字段count和静态方法getCount，统计实例创建的个数。

~~~java
package demo;

public class hello{
    public static void main(String[] args){
        Person s1=new Person();
        Person s2=new Person();
        Person s3=new Person();
        System.out.println(Person.getCount());
    }
}

class Person{
    public static int count=0;

    public Person(){//每创建一个实例，count加1
        Person.count++;
    }

    public static int getCount(){
        return Person.count;
    }
}
~~~

### 包

在前面的代码中，我们把类和接口命名为`Person`、`Student`、`Hello`等简单名字。

在现实中，如果小明写了一个`Person`类，小红也写了一个`Person`类，现在，小白既想用小明的`Person`，也想用小红的`Person`，怎么办？

如果小军写了一个`Arrays`类，恰好JDK也自带了一个`Arrays`类，如何解决类名冲突？

**在Java中，我们使用`package`来解决名字冲突**。

Java定义了一种名字空间，称之为包：`package`。一个类总是属于某个包，类名（比如`Person`）只是一个简写，真正的完整类名是`包名.类名`。

例如：

小明的`Person`类存放在包`ming`下面，因此，完整类名是`ming.Person`；

小红的`Person`类存放在包`hong`下面，因此，完整类名是`hong.Person`；

小军的`Arrays`类存放在包`mr.jun`下面，因此，完整类名是`mr.jun.Arrays`；

JDK的`Arrays`类存放在包`java.util`下面，因此，完整类名是`java.util.Arrays`。

**在定义`class`的时候，我们需要在第一行声明这个`class`属于哪个包。**

小明的`Person.java`文件：

```java
package ming; // 申明包名ming

public class Person {
}
```

在Java虚拟机执行的时候，JVM只看完整类名，因此，只要包名不同，类就不同。

**包可以是多层结构，用`.`隔开。例如：`java.util`。**

没有定义包名的`class`，它使用的是默认包，非常容易引起名字冲突，因此，不推荐不写包名的做法。

我们还需要按照包结构把上面的Java文件组织起来。假设以`package_sample`作为根目录，`src`作为源码目录，那么所有文件结构就是：

```ascii
package_sample
└─ src
    ├─ hong //包名
    │  └─ Person.java //类名
    │  ming //包名
    │  └─ Person.java
    └─ mr //包名
       └─ jun
          └─ Arrays.java
```

即所有Java文件对应的目录层次要和包的层次一致。

编译后的`.class`文件也需要按照包结构存放。如果使用IDE，把编译后的`.class`文件放到`bin`目录下，那么，编译的文件结构就是：

```ascii
package_sample
└─ bin
   ├─ hong
   │  └─ Person.class
   │  ming
   │  └─ Person.class
   └─ mr
      └─ jun
         └─ Arrays.class
```

#### 包作用域

位于同一个包的类，可以访问包作用域的字段和方法。**不用`public`、`protected`、`private`修饰的字段和方法就是包作用域**。例如，`Person`类定义在`hello`包下面：

~~~java
package hello;

public class Person{
    //包作用域
    void hello(){//不用`public`、`protected`、`private`修饰
        System.out.println("Hello");
    }
}
~~~

#### import 

在一个`class`中，我们总会引用其他的`class`。例如，小明的`ming.Person`类，如果要引用小军的`mr.jun.Arrays`类，他有三种写法：

第一种，直接写出完整类名，例如：

```java
package ming;

public class Person{
    public void run(){
        mr.jun.Arrays=new mr.jun.Arrays();
    }
}
```

很显然，每次写完整类名比较痛苦。

因此，第二种写法是用`import`语句，导入小军的`Arrays`，然后写简单类名：

~~~java
package ming;

import mr.jun.Arrays;

public class Person{
    public void run(){
        Arrays arrays= new Arrays();
    }
}
~~~

在写`import`的时候，可以使用`*`，表示把这个包下面的所有`class`都导入进来（但不包括子包的`class`）：

```java
// Person.java
package ming;

// 导入mr.jun包的所有class:
import mr.jun.*;

public class Person {
    public void run() {
        Arrays arrays = new Arrays();
    }
}
```

我们一般不推荐这种写法，因为在导入了多个包后，很难看出`Arrays`类属于哪个包。

还有一种`import static`的语法，它可以导入可以导入一个类的静态字段和静态方法：

```java
package main;

// 导入System类的所有静态字段和静态方法:
import static java.lang.System.*;

public class Main {
    public static void main(String[] args) {
        // 相当于调用System.out.println(…)
        out.println("Hello, world!");
    }
}
```

`import static`很少使用。

ava编译器最终编译出的`.class`文件只使用*完整类名*，因此，在代码中，当编译器遇到一个`class`名称时：

- 如果是完整类名，就直接根据完整类名查找这个`class`；
- 如果是简单类名，按下面的顺序依次查找：
  - 查找当前`package`是否存在这个`class`；
  - 查找`import`的包是否包含这个`class`；
  - 查找`java.lang`包是否包含这个`class`。

如果按照上面的规则还无法确定类名，则编译报错。

### 作用域

在Java中，我们经常看到`public`、`protected`、`private`这些修饰符。在Java中，这些修饰符可以用来限定访问作用域。

#### public

定义为`public`的`class`、`interface`可以被其他任何类访问：

```
package abc;

public class Hello {
    public void hi() {
    }
}
```

上面的`Hello`是`public`，因此，可以被其他包的类访问：

```java
package xyz;

class Main {
    void foo() {
        // Main可以访问Hello
        Hello h = new Hello();
    }
}
```

定义为`public`的`field`、`method`可以被其他类访问，前提是首先有访问`class`的权限：

```java
package abc;

public class Hello {
    public void hi() {
    }
}
```

上面的`hi()`方法是`public`，可以被其他类调用，前提是首先要能访问`Hello`类：

```java
package xyz;

class Main {
    void foo() {
        Hello h = new Hello();
        h.hi();
    }
}
```

#### private

定义为`private`的`field`、`method`无法被其他类访问：

```java
package abc;

public class Hello {
    // 不能被其他类调用:
    private void hi() {
    }

    public void hello() {
        this.hi();
    }
}
```

实际上，确切地说，`private`访问权限被限定在`class`的内部，而且与方法声明顺序*无关*。推荐把`private`方法放到后面，因为`public`方法定义了类对外提供的功能，阅读代码的时候，应该先关注`public`方法：

```java
package abc;

public class Hello {
    public void hello() {
        this.hi();
    }

    private void hi() {
    }
}
```

**由于Java支持嵌套类，如果一个类内部还定义了嵌套类，那么，嵌套类拥有访问`private`的权限**：

~~~java
public class Main {
    public static void main(String[] args) {
        Inner i = new Inner();
        i.hi();
    }

    // private方法:
    private static void hello() {
        System.out.println("private hello!");
    }

    // 静态内部类:
    static class Inner {
        public void hi() {
            Main.hello();
        }
    }
}
~~~

定义在一个`class`内部的`class`称为嵌套类（`nested class`），Java支持好几种嵌套类。



#### protected

`protected`作用于继承关系。定义为`protected`的字段和方法可以被子类访问，以及子类的子类：

```java
package abc;

public class Hello {
    // protected方法:
    protected void hi() {
    }
}
```

上面的`protected`方法可以被继承的类访问：

```java
package xyz;

class Main extends Hello {
    void foo() {
        Hello h = new Hello();
        // 可以访问protected方法:
        h.hi();
    }
}
```

#### package

最后，包作用域是指一个类允许访问同一个`package`的没有`public`、`private`修饰的`class`，以及没有`public`、`protected`、`private`修饰的字段和方法。

```java
package abc;
// package权限的类:
class Hello {
    // package权限的方法:
    void hi() {
    }
}
```

只要在同一个包，就可以访问`package`权限的`class`、`field`和`method`：

```java
package abc;

class Main {
    void foo() {
        // 可以访问package权限的类:
        Hello h = new Hello();
        // 可以调用package权限的方法:
        h.hi();
    }
}
```

注意，包名必须完全一致，包没有父子关系，`com.apache`和`com.apache.abc`是不同的包。

#### 局部变量

在方法内部定义的变量称为局部变量，局部变量作用域从变量声明处开始到对应的块结束。方法参数也是局部变量。

```java
package abc;

public class Hello {
    void hi(String name) { // ①
        String s = name.toLowerCase(); // ②
        int len = s.length(); // ③
        if (len < 10) { // ④
            int p = 10 - len; // ⑤
            for (int i=0; i<10; i++) { // ⑥
                System.out.println(); // ⑦
            } // ⑧
        } // ⑨
    } // ⑩
}
```

我们观察上面的`hi()`方法代码：

- 方法参数name是局部变量，它的作用域是整个方法，即①～⑩；
- 变量s的作用域是定义处到方法结束，即②～⑩；
- 变量len的作用域是定义处到方法结束，即③～⑩；
- 变量p的作用域是定义处到if块结束，即⑤～⑨；
- 变量i的作用域是for循环，即⑥～⑧。

使用局部变量时，应该尽可能把局部变量的作用域缩小，尽可能延后声明局部变量。

#### final

Java还提供了一个`final`修饰符。`final`与访问权限不冲突，它有很多作用。

用`final` 修饰`class` 可以阻止被继承

~~~java
public final class Hello{
    ...
}
~~~

用`final`修饰`method`可以阻止被子类覆写：

~~~java
public class Hello{
    protected final void hello(){
        ...
    }
}
~~~

用`final`修饰`field`可以阻止被重新赋值：

~~~java
public class Hello{
    protected final int n=0;
}
~~~

用`final`修饰局部变量可以阻止被重新赋值：

~~~java
public class Hello{
    protected void hello(final int t){
        t=1;//error!
    }
}
~~~

#### 最佳实践

如果不确定是否需要`public`，就不声明为`public`，即尽可能少地暴露对外的字段和方法。

把方法定义为`package`权限有助于测试，因为测试类和被测试类只要位于同一个`package`，测试代码就可以访问被测试类的`package`权限方法。

一个`.java`文件只能包含一个`public`类，但可以包含多个非`public`类。如果有`public`类，文件名必须和`public`类的名字相同。

### classpath 和 jar

`classpath`是JVM用到的一个环境变量，它用来指示JVM如何搜索`class`。

因为Java是编译型语言，源码文件是`.java`，而编译后的`.class`文件才是真正可以被JVM执行的字节码。因此，**JVM需要知道，如果要加载一个`abc.xyz.Hello`的类，应该去哪搜索对应的`Hello.class`文件。**

所以，**`classpath`就是一组目录的集合**，它设置的搜索路径与操作系统相关。例如，在Windows系统上，用`;`分隔，带空格的目录用`""`括起来，可能长这样：

```
C:\work\project1\bin;C:\shared;"D:\My Documents\project1\bin"
```

在Linux系统上，用`:`分隔，可能长这样：

```
/usr/shared:/usr/local/bin:/home/liaoxuefeng/bin
```

...

不要设置`classpath`！默认的当前目录`.`对于绝大多数情况都够用了。

#### jar包

如果有很多`.class`文件，散落在各层目录中，肯定不便于管理。如果能把目录打一个包，变成一个文件，就方便多了。

jar包就是用来干这个事的，它可以把`package`组织的目录层级，以及各个目录下的所有文件（包括`.class`文件和其他文件）都打成一个jar文件，这样一来，无论是备份，还是发给客户，就简单多了。

**jar包实际上就是一个zip格式的压缩文件，而jar包相当于目录**。如果我们要执行一个jar包的`class`，就可以把jar包放到`classpath`中：

```
java -cp ./hello.jar abc.xyz.Hello
```

这样JVM会自动在`hello.jar`文件里去搜索某个类。

那么问题来了：如何创建jar包？

因为jar包就是zip包，所以，直接在资源管理器中，找到正确的目录，点击右键，在弹出的快捷菜单中选择“发送到”，“压缩(zipped)文件夹”，就制作了一个zip文件。然后，把后缀从`.zip`改为`.jar`，一个jar包就创建成功。

假设编译输出的目录结构是这样：

```ascii
package_sample
└─ bin
   ├─ hong
   │  └─ Person.class
   │  ming
   │  └─ Person.class
   └─ mr
      └─ jun
         └─ Arrays.class
```

这里需要特别注意的是，jar包里的第一层目录，不能是`bin`，而应该是`hong`、`ming`、`mr`。如果在Windows的资源管理器中看，应该长这样：

![hello.zip.ok](https://www.liaoxuefeng.com/files/attachments/1261393208671488/l)

如果长这样：

![hello.zip.invalid](https://www.liaoxuefeng.com/files/attachments/1261391527906784/l)

说明打包打得有问题，JVM仍然无法从jar包中查找正确的`class`，原因是`hong.Person`必须按`hong/Person.class`存放，而不是`bin/hong/Person.class`。

jar包还可以包含一个特殊的`/META-INF/MANIFEST.MF`文件，`MANIFEST.MF`是纯文本，可以指定`Main-Class`和其它信息。JVM会自动读取这个`MANIFEST.MF`文件，如果存在`Main-Class`，我们就不必在命令行指定启动的类名，而是用更方便的命令：

```
java -jar hello.jar
```

jar包还可以包含其它jar包，这个时候，就需要在`MANIFEST.MF`文件里配置`classpath`了。

在大型项目中，不可能手动编写`MANIFEST.MF`文件，再手动创建zip包。Java社区提供了大量的开源构建工具，例如[Maven](https://www.liaoxuefeng.com/wiki/1252599548343744/1255945359327200)，可以非常方便地创建jar包。

### 模块

从Java 9开始，JDK又引入了模块（Module）。

什么是模块？这要从Java 9之前的版本说起。

我们知道，`.class`文件是JVM看到的最小可执行文件，而一个大型程序需要编写很多Class，并生成一堆`.class`文件，很不便于管理，所以，`jar`文件就是`class`文件的容器。

在Java 9之前，一个大型Java程序会生成自己的jar文件，同时引用依赖的第三方jar文件，而JVM自带的Java标准库，实际上也是以jar文件形式存放的，这个文件叫`rt.jar`，一共有60多M。

如果是自己开发的程序，除了一个自己的`app.jar`以外，还需要一堆第三方的jar包，运行一个Java程序，一般来说，命令行写这样：

```bash
java -cp app.jar:a.jar:b.jar:c.jar com.liaoxuefeng.sample.Main
```

如果漏写了某个运行时需要用到的jar，那么在运行期极有可能抛出`ClassNotFoundException`。

所以，jar只是用于存放class的容器，它并不关心class之间的依赖。

从Java 9开始引入的模块，主要是为了解决“依赖”这个问题。如果`a.jar`必须依赖另一个`b.jar`才能运行，那我们应该给`a.jar`加点说明啥的，让程序在编译和运行的时候能自动定位到`b.jar`，**这种自带“依赖关系”的class容器就是模块**。

为了表明Java模块化的决心，从Java 9开始，原有的Java标准库已经由一个单一巨大的`rt.jar`分拆成了几十个模块，这些模块以`.jmod`扩展名标识，可以在`$JAVA_HOME/jmods`目录下找到它们：

- java.base.jmod
- java.compiler.jmod
- java.datatransfer.jmod
- java.desktop.jmod
- ...

这些`.jmod`文件每一个都是一个模块，模块名就是文件名。例如：模块`java.base`对应的文件就是`java.base.jmod`。模块之间的依赖关系已经被写入到模块内的`module-info.class`文件了。所有的模块都直接或间接地依赖`java.base`模块，只有`java.base`模块不依赖任何模块，它可以被看作是“根模块”，好比所有的类都是从`Object`直接或间接继承而来。

**把一堆class封装为jar仅仅是一个打包的过程，而把一堆class封装为模块则不但需要打包，还需要写入依赖关系，并且还可以包含二进制代码（通常是JNI扩展）**。此外，模块支持多版本，即在同一个模块中可以为不同的JVM提供不同的版本。

#### 编写模块

那么，我们应该如何编写模块呢？还是以具体的例子来说。首先，创建模块和原有的创建Java项目是完全一样的，以`oop-module`工程为例，它的目录结构如下：

```ascii
oop-module
├── bin
├── build.sh
└── src
    ├── com
    │   └── itranswarp
    │       └── sample
    │           ├── Greeting.java
    │           └── Main.java
    └── module-info.java
```

其中，`bin`目录存放编译后的class文件，`src`目录存放源码，按包名的目录结构存放，仅仅在`src`目录下多了一个`module-info.java`这个文件，这就是模块的描述文件。在这个模块中，它长这样：

```java
module hello.world {
	requires java.base; // 可不写，任何模块都会自动引入java.base
	requires java.xml;
}
```

其中，`module`是关键字，后面的`hello.world`是模块的名称，它的命名规范与包一致。花括号的`requires xxx;`表示这个模块需要引用的其他模块名。除了`java.base`可以被自动引入外，这里我们引入了一个`java.xml`的模块。

当我们使用模块声明了依赖关系后，才能使用引入的模块。例如，`Main.java`代码如下

```java
package com.itranswarp.sample;

// 必须引入java.xml模块后才能使用其中的类:
import javax.xml.XMLConstants;

public class Main {
	public static void main(String[] args) {
		Greeting g = new Greeting();
		System.out.println(g.hello(XMLConstants.XML_NS_PREFIX));
	}
}
```





## java核心类

- 字符串
- StringBuilder
- StringJoiner
- 包装类型
- JavaBean
- 枚举
- 常用工具类

### 字符串和编码

#### String

在Java中，`String`是一个引用类型，它本身也是一个`class`。但是，Java编译器对`String`有特殊处理，即可以直接用`"..."`来表示一个字符串：

```
String s1 = "Hello!";
```

实际上字符串在`String`内部是通过一个`char[]`数组表示的，因此，按下面的写法也是可以的：

```
String s2 = new String(new char[] {'H', 'e', 'l', 'l', 'o', '!'});
```

因为`String`太常用了，所以Java提供了`"..."`这种字符串字面量表示方法。

Java字符串的一个重要特点就是字符串*不可变*。这种不可变性是通过内部的`private final char[]`字段，以及没有任何修改`char[]`的方法实现的。

~~~java
public class Main{
    public static class main(String[] args){
        String s= "Hello";
        System.out.println(s);
        s=s.toUpperCase();
        System.out.println(s);
    }
}
~~~

#### 字符串比较

当我们想要比较两个字符串是否相同时，要特别注意，我们实际上是想比较字符串的内容是否相同。必须使用`equals()`方法而不能用`==`。

~~~java
public class Main{
    public static class main(String[] args){
        String s1="Helllo";
        String s2="Hello";
        System.out.println(s1==s2);
        System.out.println(s1.equals(s2));
    }
}
//true
//true 
~~~

从表面上看，两个字符串用`==`和`equals()`比较都为`true`，但实际上那只是Java编译器在编译期，会自动把所有相同的字符串当作一个对象放入常量池，自然`s1`和`s2`的引用就是相同的。

所以，这种`==`比较返回`true`纯属巧合。换一种写法，`==`比较就会失败：

~~~java
public class Main{
    public static class main(String[] args){
        String s1="Hello";
        String s2="Hello";
        System.out.println(s1==s2);
        System.out.println(s1.equals(s2));
    }
}
//false
//true
~~~

**结论：两个字符串比较，必须总是使用`equals()`方法。**

要忽略大小写比较，使用`equalsIgnoreCase()`方法。

---

`String`类还提供了多种方法来搜索子串、提取子串。常用的方法有：

~~~java
//是否包含字串
"Hello".contains("ll");
//true
~~~

注意到`contains()`方法的参数是`CharSequence`而不是`String`，因为`CharSequence`是`String`的父类。

搜索子串的更多的例子：

~~~java
"Hello".indexof("l");//2
"Hello".lastIndex("l");//3
"Hello".startWith("He");//true
"Hello".endWith("lo");//true
~~~

提取字串的例子;

~~~java
"Hello".substring(2);//"llo"
"Hello".substring(2,4);//"ll"->[2,4)->取第二第三个元素
~~~

#### 去除首尾空白字符

使用`trim()` 方法可以移除字符串首尾空白字符。空白字符包括空格，`\t`，`\r`，`\n`：

~~~java
"\tHello\r\n".trim();//"Hello"
~~~

注意：`trim()`并没有改变字符串的内容，而是返回了一个新字符串。

另一个`strip()`方法也可以移除字符串首尾空白字符。它和`trim()`不同的是，类似中文的空格字符`\u3000`也会被移除：

```java
"\u3000Hello\u3000".strip(); // "Hello"
" Hello ".stripLeading(); // "Hello "
" Hello ".stripTrailing(); // " Hello"
```

`String`还提供了`isEmpty()`和`isBlank()`来判断字符串是否为空和空白字符串：

```java
"".isEmpty(); // true，因为字符串长度为0
"  ".isEmpty(); // false，因为字符串长度不为0
"  \n".isBlank(); // true，因为只包含空白字符
" Hello ".isBlank(); // false，因为包含非空白字符
```

#### 替换字符

要在字符串中替换子串，有两种方法。一种是根据字符或字符串替换：

```java
String s="Hello";
s.replace('l','w');//"Hewwo"->所有字符'l'被替换为'w'
```

另一种是通过正则表达式替换：

~~~java
String s= "A,,B;C,D";
s.replaceAll("[\\,\\;\\s]+",",");//"A,B,C,D"
~~~

上面的代码通过正则表达式，把匹配的子串统一替换为`","`。关于正则表达式的用法我们会在后面详细讲解。

#### 分割字符串

要分割字符串，使用`split()`方法，并且传入的也是正则表达式：

~~~java
String s="A,B,C,D";
String[] ss=s.split("\\,");//{"A","B","C","D"}
~~~

#### 拼接字符串

拼接字符串使用静态方法`join()`，它用指定的字符串连接字符串数组：

~~~java
String[] arr={"A","B","C","D"};
String s=String.join("***",arr);//"A***B***C"
~~~

#### 格式化字符串

字符串提供了`formatted()`方法和`format()`静态方法，可以传入其他参数，替换占位符，然后生成新的字符串：

~~~java
public class Main{
    public static class main(String[] args){
        String s="Hi %s,your score is %d!";
        System.out.println(s.formatted("Alice",80));
        System.out.println(Srtring.format("Hi %s,your score is %.2f!","Bob",95.5));
    }
}
~~~

有几个占位符，后面就传入几个参数。参数类型要和占位符一致。我们经常用这个方法来格式化信息。常用的占位符有：

- `%s`：显示字符串；
- `%d`：显示整数；
- `%x`：显示十六进制整数；
- `%f`：显示浮点数。

占位符还可以带格式，例如`%.2f`表示显示两位小数。如果你不确定用啥占位符，那就始终用`%s`，因为`%s`可以显示任何数据类型。

#### 类型转换

要把任意基本类型或引用类型转换为字符串，可以使用静态方法`valueOf()`。这是一个重载方法，编译器会根据参数自动选择合适的方法

~~~java
String.valueof(123);//"123"
String.valueof(true);//"true"
~~~

要把字符串转换为其他类型，就需要根据情况。例如，把字符串转换为`int`类型：

~~~java
int n1=Integer.parseInt("123");//123
int n2 = Integer.parseInt("ff", 16); // 按十六进制转换，255
~~~

 字符串转换为`boolean`类型：

~~~java
boolean b1=Boolean.parseBoolean("true");//true
boolean b2=Boolean.parseBoolean("false");//false
~~~

#### 转换为char[]

`String`和`char[]`类型可以互相转换，方法是：

~~~java
char[] cs="Hello".toVharArray();//String->char[]
String s=new String(cs);//char[]->String
~~~

如果修改了`char[]`数组，`String`并不会改变：

~~~java
public class Main {
    public static void main(String[] args) {
        char[] cs = "Hello".toCharArray();
        String s = new String(cs);
        System.out.println(s);//Hello
        cs[0] = 'X';
        System.out.println(s);//Hello
    }
}
~~~

这是因为通过`new String(char[])`创建新的`String`实例时，它并不会直接引用传入的`char[]`数组，而是会**复制一份**，所以，修改外部的`char[]`数组不会影响`String`实例内部的`char[]`数组，因为这是两个不同的数组。

从`String`的不变性设计可以看出，如果传入的对象有可能改变，我们需要复制而不是直接引用。

~~~java
package demo;

import java.util.Arrays;


public class hello{
    public static void  main(String[] args) {
        int[] score={1,2,3,4};
//        int[] abc=score.clone();
//        System.out.println(Arrays.toString(score));

        Score s=new Score(score);
        score[2]=99;
        System.out.println(Arrays.toString(score));
        s.printScores();
    }
}

class Score{
    private  int[] scores;
    public Score(int[] scores){
//        int[] abc=scores.clone();
        this.scores=scores.clone();
    }

    public void printScores(){
        System.out.println(Arrays.toString(scores));
    }
}
~~~

### StringBuilder

Java编译器对`String`做了特殊处理，使得我们可以直接用`+`拼接字符串。

考察下面的循环代码：

```java
String s = "";
for (int i = 0; i < 1000; i++) {
    s = s + "," + i;
}
```

虽然可以直接拼接字符串，但是，在循环中，每次循环都会创建新的字符串对象，然后扔掉旧的字符串。这样，绝大部分字符串都是临时对象，不但浪费内存，还会影响GC效率

为了能高效拼接字符串，Java标准库提供了`StringBuilder`，它是一个可变对象，可以预分配缓冲区，这样，往`StringBuilder`中新增字符时，不会创建新的临时对象：

~~~java
StringBuilder sb=new StringBuilder(1024);
for (int i-0;i<1000;i++){
    sb.append(',');
    sb.append(i);
}
String s=sb.toString();
~~~

`StringBuilder`还可以进行链式操作：

~~~java
public class Main{
    public static void main(String[] args){
        var sb=new StringBuilder(1024);
        sb.append("Mr")
            .append("Bob")
            .append("!")
            .insert(0,"Hello, ");
       System.out.println(sb.toString());
    }
}
~~~

练习：

请使用`StringBuilder`构造一个`INSERT`语句：

~~~java
package demo;

import java.util.Arrays;


public class hello {
    public static void main(String[] args) {
        String[] fields = {"name", "position", "salary"};
        String table = "employee";
        String insert = buildInsertSql(table, fields);

        System.out.println(insert);

        String s = "INSERT INTO employee (name, position, salary) VALUES (?, ?, ?)";

        System.out.println(s.equals(insert) ? "测试成功" : "测试失败");


    }

    private static String buildInsertSql(String table, String[] fields) {
        var bb = new StringBuilder(1024);
//        bb.append("INSERT INTO employee (name, position, salary) VALUES (?, ?, ?)");
        bb.append("INSERT INTO ")
                .append(table)
                .append(" (")
//                .append(fields[0])
//                .append(", ")
//                .append(fields[1])
//                .append(", ")
//                .append(fields[2])
                .append(String.join(", ", fields))
                .append(") ")
                .append("VALUES (?, ?, ?)");

        String s = bb.toString();
        return s;
    }
}

~~~



### StringJoiner

用分隔符拼接数组的需求很常见，所以Java标准库还提供了一个`StringJoiner`来干这个事：

~~~java
package demo;

import java.util.StringJoiner;

public class hello{
    public static void main(String[] args){
        String[] names={"Bob","Alice","Grace"};
        var sj=new StringJoiner(", " , "Hello ","!");

        for (String name :names){
            sj.add(name);
        }

        System.out.println(sj.toString());
    }
}
>>Hello Bob, Alice, Grace! 
~~~

#### String.join()

`String`还提供了一个静态方法`join()`，这个方法在内部使用了`StringJoiner`来拼接字符串，在不需要指定“开头”和“结尾”的时候，用`String.join()`更方便：

~~~java
String[] names = {"Bob", "Alice", "Grace"};
var s = String.join(", ", names);
~~~

练习：

请使用`StringJoiner`构造一个`SELECT`语句：

~~~java
package demo;

import java.util.StringJoiner;

public class hello{
    public static void main(String[] args){
     String[] fileds={"name","position","salary"};
     String table="employee";
     String select=buildSelect(table,fileds);

     System.out.println(select);
        System.out.println("SELECT name, position, salary FROM employee".equals(select) ? "测试成功" : "测试失败");


    }

    private static String buildSelect(String table, String[] fileds) {
        var s=new StringJoiner(", ","SELECT ", " FROM "+table);
        for (String name:fileds){
            s.add(name);

        }


        return s.toString();
    }
}
~~~

### 包装类型

我们已经知道，Java的数据类型分两种：

- 基本类型：`byte`，`short`，`int`，`long`，`boolean`，`float`，`double`，`char`
- 引用类型：所有`class`和`interface`类型

引用类型可以赋值为`null`，表示空，但基本类型不能赋值为`null`：

~~~java
String s = null;
int n = null; // compile error!

~~~

比如，想要**把`int`基本类型变成一个引用类型**，我们可以定义一个`Integer`类，它只包含一个实例字段`int`，这样，`Integer`类就可以视为`int`的包装类（Wrapper Class）：

~~~java
public class Integer{
    private int value;
    
    public Integer(int value){
        this.value=value;
    }
    
    public int intValue(){
        return this.value;
    }
}
~~~

定义好了`Integer`类，我们就可以把`int`和`Integer`互相转换：

~~~java
Integer n=null;
Integer n2=new Integer(99);
int n3=n2.intvalue();
~~~

实际上，因为包装类型非常有用，Java核心库为每种基本类型都提供了对应的包装类型：

| 基本类型 | 对应的引用类型      |
| :------- | :------------------ |
| boolean  | java.lang.Boolean   |
| byte     | java.lang.Byte      |
| short    | java.lang.Short     |
| int      | java.lang.Integer   |
| long     | java.lang.Long      |
| float    | java.lang.Float     |
| double   | java.lang.Double    |
| char     | java.lang.Character |

我们可以直接使用，并不需要自己去定义：

~~~java
public class Main {
    public static void main(String[] args) {
        int i = 100;
        // 通过new操作符创建Integer实例(不推荐使用,会有编译警告):
        Integer n1 = new Integer(i);
        // 通过静态方法valueOf(int)创建Integer实例:
        Integer n2 = Integer.valueOf(i);
        // 通过静态方法valueOf(String)创建Integer实例:
        Integer n3 = Integer.valueOf("100");
        System.out.println(n3.intValue());
    }
}

~~~

#### Auto Boxing

因为`int`和`Integer`可以互相转换：

~~~java
int i=100;
Integer.valueof(int)
    int x=n;
~~~

这种直接把`int`变为`Integer`的赋值写法，称为自动装箱（Auto Boxing），反过来，把`Integer`变为`int`的赋值写法，称为自动拆箱（Auto Unboxing）。

注意：自动装箱和自动拆箱只发生在编译阶段，目的是为了少写代码。

装箱和拆箱会影响代码的执行效率，因为**编译后的`class`代码是严格区分基本类型和引用类型的**。并且，自动拆箱执行时可能会报`NullPointerException`：

#### 不变类

所有的包装类型都是不变类。我们查看`Integer`的源码可知，它的核心代码如下：

```
public final class Integer {
    private final int value;
}
```

因此，一旦创建了`Integer`对象，该对象就是不变的。

对两个`Integer`实例进行比较要特别注意：绝对不能用`==`比较，因为`Integer`是引用类型，必须使用`equals()`比较：

~~~java
public class Main{
    public static void main(String[] args){
        Integer x=127;
        Integer y=127;
        Integer m=99999;
        Integer n=99999;
        System.out.println("x==y:"+(x==y));//true
        System.out.println("m==n:"+(x==y));//false
        System.out.println("x==y:"+(x.equals(y)));//true
        System.out.println("m==n:"+(m.equals(n)));//true
        
    }
}
~~~

仔细观察结果的童鞋可以发现，`==`比较，较小的两个相同的`Integer`返回`true`，较大的两个相同的`Integer`返回`false`，这是因为`Integer`是不变类，编译器把`Integer x = 127;`自动变为`Integer x = Integer.valueOf(127);`，为了节省内存，`Integer.valueOf()`对于较小的数，始终返回相同的实例，因此，`==`比较“恰好”为`true`，但我们*绝不能*因为Java标准库的`Integer`内部有缓存优化就用`==`比较，必须用`equals()`方法比较两个`Integer`。

---

因为`Integer.valueOf()`可能始终返回同一个`Integer`实例，因此，在我们自己创建`Integer`的时候，以下两种方法：

- 方法1：`Integer n = new Integer(100);`
- 方法2：`Integer n = Integer.valueOf(100);`



方法2更好，因为方法1总是创建新的`Integer`实例，方法2把内部优化留给`Integer`的实现者去做，即使在当前版本没有优化，也有可能在下一个版本进行优化。

我们把能创建“新”对象的静态方法称为静态工厂方法。`Integer.valueOf()`就是静态工厂方法，它尽可能地返回缓存的实例以节省内存。

#### 进制转换

`Integer`类本身还提供了大量方法，例如，最常用的静态方法`parseInt()`可以把字符串解析成一个整数：

~~~java
int x1=Integer.parseInt("100");//100
int x2=Integer.parseInt("100",16);//256,按16进制解析
~~~

`Integer`还可以把整数格式化为指定进制的字符串：

~~~java
public class Main {
    public static void main(String[] args) {
        System.out.println(Integer.toString(100)); // "100",表示为10进制
        System.out.println(Integer.toString(100, 36)); // "2s",表示为36进制
        System.out.println(Integer.toHexString(100)); // "64",表示为16进制
        System.out.println(Integer.toOctalString(100)); // "144",表示为8进制
        System.out.println(Integer.toBinaryString(100)); // "1100100",表示为2进制
    }
}

~~~

注意：上述方法的输出都是`String`，**在计算机内存中，只用二进制表示，不存在十进制或十六进制的表示方法**。`int n = 100`在内存中总是以4字节的二进制表示：

```ascii
┌────────┬────────┬────────┬────────┐
│00000000│00000000│00000000│01100100│
└────────┴────────┴────────┴────────┘
```

我们经常使用的`System.out.println(n);`是依靠核心库自动把整数格式化为10进制输出并显示在屏幕上，使用`Integer.toHexString(n)`则通过核心库自动把整数格式化为16进制。

这里我们注意到程序设计的一个重要原则：**数据的存储和显示要分离。**

ava的包装类型还定义了一些有用的静态变量

```java
// boolean只有两个值true/false，其包装类型只需要引用Boolean提供的静态字段:
Boolean t = Boolean.TRUE;
Boolean f = Boolean.FALSE;
// int可表示的最大/最小值:
int max = Integer.MAX_VALUE; // 2147483647
int min = Integer.MIN_VALUE; // -2147483648
// long类型占用的bit和byte数量:
int sizeOfLong = Long.SIZE; // 64 (bits)
int bytesOfLong = Long.BYTES; // 8 (bytes)
```

最后，所有的整数和浮点数的包装类型都继承自`Number`，因此，可以非常方便地直接通过包装类型获取各种基本类型：

```java
// 向上转型为Number:
Number num = new Integer(999);
// 获取byte, int, long, float, double:
byte b = num.byteValue();
int n = num.intValue();
long ln = num.longValue();
float f = num.floatValue();
double d = num.doubleValue();
```

#### 处理无符号整型

在Java中，并没有无符号整型（Unsigned）的基本数据类型。`byte`、`short`、`int`和`long`都是带符号整型，最高位是符号位。而C语言则提供了CPU支持的全部数据类型，包括无符号整型。**无符号整型和有符号整型的转换在Java中就需要借助包装类型的静态方法完成。**

例如，byte是有符号整型，范围是`-128`~`+127`，但如果把`byte`看作无符号整型，它的范围就是`0`~`255`。我们把一个负的`byte`按无符号整型转换为`int`：

~~~java
public class Main {
    public static void main(String[] args) {
        byte x = -1;
        byte y = 127;
        System.out.println(Byte.toUnsignedInt(x)); // 255
        System.out.println(Byte.toUnsignedInt(y)); // 127
    }
}

~~~

因为`byte`的`-1`的二进制表示是`11111111`，以无符号整型转换后的`int`就是`255`。

类似的，可以把一个`short`按unsigned转换为`int`，把一个`int`按unsigned转换为`long`。

### JavaBean

在Java中，有很多`class`的定义都符合这样的规范：

- 若干`private`实例字段；
- 通过`public`方法来读写实例字段。

例如：

~~~java
public class Person{
    private String name;
    private int age;
    
    public String getname(){//读
        return this.name;
    }
    
    public int getAge(){//读
        return this.age;
    }
    
    public void setAge(int age){//写
        this.age=age;
    }
}
~~~

如果读写方法符合以下这种命名规范：

```java
// 读方法:
public Type getXyz()//无参数
// 写方法:
public void setXyz(Type value)//有参数
```

那么这种`class`被称为`JavaBean`.

上面的字段是`xyz`，那么**读写方法名分别以`get`和`set`开头**，并且后接大写字母开头的字段名`Xyz`，因此两个读写方法名分别是`getXyz()`和`setXyz()`。

`boolean`字段比较特殊，它的读方法一般命名为`isXyz()`：

```java
// 读方法:
public boolean isChild()
// 写方法:
public void setChild(boolean value)
```



我们通常把一组对应的读方法（`getter`）和写方法（`setter`）称为属性（`property`）。例如，`name`属性：

- 对应的读方法是`String getName()`
- 对应的写方法是`setName(String)`



只有`getter`的属性称为只读属性（read-only），例如，定义一个age只读属性：

- 对应的读方法是`int getAge()`
- 无对应的写方法`setAge(int)`

类似的，只有`setter`的属性称为只写属性（write-only）。

很明显，只读属性很常见，只写属性不常见。

---

属性只需要定义`getter`和`setter`方法，不一定需要对应的字段。例如，`child`只读属性定义如下：

~~~java
public class Person{
    private String name;
    private int age;
    
    public String getname(){//读
        return this.name;
    }
    
    public int getAge(){//读
        return this.age;
    }
    
    public void setAge(int age){//写
        this.age=age;
    }
    
    public boolean isChild(){
        return age<=6;
    }
}
~~~

可以看出，`getter`和`setter`也是一种数据封装的方法。

#### JavaBean的作用

JavaBean主要用来传递数据，即把一组数据组合成一个JavaBean便于传输。此外，JavaBean可以方便地被IDE工具分析，生成读写属性的代码，主要用在图形界面的可视化设计中。

通过IDE，可以快速生成`getter`和`setter`。例如，在Eclipse中，先输入以下代码：

```
public class Person {
    private String name;
    private int age;
}
```

然后，点击右键，在弹出的菜单中选择“Source”，“Generate Getters and Setters”，在弹出的对话框中选中需要生成`getter`和`setter`方法的字段，点击确定即可由IDE自动完成所有方法代码。

#### 枚举JavaBean属性

要枚举一个JavaBean的所有属性，可以直接使用Java核心库提供的`Introspector`：

~~~java
public class Main {
    public static void main(String[] args) throws Exception {
        BeanInfo info = Introspector.getBeanInfo(Person.class);
        for (PropertyDescriptor pd : info.getPropertyDescriptors()) {
            System.out.println(pd.getName());
            System.out.println("  " + pd.getReadMethod());
            System.out.println("  " + pd.getWriteMethod());
        }
    }
}

class Person {
    private String name;
    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}

~~~

### 枚举类

在Java中，我们可以**通过`static final`来定义常量**。例如，我们希望定义周一到周日这7个常量，可以用7个不同的`int`表示：

```java
public class Weekday {
    public static final int SUN = 0;
    public static final int MON = 1;
    public static final int TUE = 2;
    public static final int WED = 3;
    public static final int THU = 4;
    public static final int FRI = 5;
    public static final int SAT = 6;
}
```

使用常量的时候，可以这么引用：

```java
if (day == Weekday.SAT || day == Weekday.SUN) {
    // TODO: work at home
}
```

也可以把常量定义为字符串类型，例如，定义3种颜色的常量：

```java
public class Color {
    public static final String RED = "r";
    public static final String GREEN = "g";
    public static final String BLUE = "b";
}
```

使用常量的时候，可以这么引用：

```java
String color = ...
if (Color.RED.equals(color)) {
    // TODO:
}
```

无论是`int`常量还是`String`常量，使用这些常量来表示一组枚举值的时候，有一个严重的问题就是，编译器无法检查每个值的合理性。例如：

```
if (weekday == 6 || weekday == 7) {
    if (tasks == Weekday.MON) {
        // TODO:
    }
}
```

上述代码编译和运行均不会报错，但存在两个问题：

- 注意到`Weekday`定义的常量范围是`0`~`6`，并不包含`7`，编译器无法检查不在枚举中的`int`值；
- 定义的常量仍可与其他变量比较，但其用途并非是枚举星期值。

#### enum

为了让编译器能自动检查某个值在枚举的集合内，并且，不同用途的枚举需要不同的类型来标记，不能混用，我们可以使用`enum`来定义枚举类：

```java
package demo;


import java.beans.*;

public class hello {
    public static void main(String[] args) {

        Weekday day = Weekday.SUN;
        if (day == Weekday.SAT || day == Weekday.SUN) {
            System.out.println("work at home");

        } else {
            System.out.println("work at office");
        }
    }
}


enum Weekday {
    SUN, MON, TUE, WED, THU, FRI, SAT;
}
```

注意到定义枚举类是通过关键字`enum`实现的，我们只需依次列出枚举的常量名。

和`int`定义的常量相比，使用`enum`定义枚举有如下好处：

**首先，`enum`常量本身带有类型信息，即`Weekday.SUN`类型是`Weekday`，编译器会自动检查出类型错误**。例如，下面的语句不可能编译通过：

```java
int day = 1;
if (day == Weekday.SUN) { // Compile error: bad operand types for binary operator '=='
}
```

其次，不可能引用到非枚举的值，因为无法通过编译。

最后，不同类型的枚举不能互相比较或者赋值，因为类型不符。例如，不能给一个`Weekday`枚举类型的变量赋值为`Color`枚举类型的值：

```java
Weekday x = Weekday.SUN; // ok!
Weekday y = Color.RED; // Compile error: incompatible types
```

#### enum的比较

使用`enum`定义的枚举类是一种引用类型。前面我们讲到，引用类型比较，要使用`equals()`方法，如果使用`==`比较，它比较的是两个引用类型的变量是否是同一个对象。因此，引用类型比较，要始终使用`equals()`方法，但`enum`类型可以例外。

这是因为`enum`类型的每个常量在JVM中只有一个唯一实例，所以可以直接用`==`比较：

```java
if (day == Weekday.FRI) { // ok!
}
if (day.equals(Weekday.SUN)) { // ok, but more code!
}
```

#### enum类型

通过`enum`定义的枚举类，和其他的`class`有什么区别？

答案是没有任何区别。`enum`定义的类型就是`class`，只不过它有以下几个特点：

- 定义的`enum`类型总是继承自`java.lang.Enum`，且无法被继承；
- 只能定义出`enum`的实例，而无法通过`new`操作符创建`enum`的实例；
- 定义的每个实例都是引用类型的唯一实例；
- 可以将`enum`类型用于`switch`语句。

例如，我们定义的`Color`枚举类：

```
public enum Color {
    RED, GREEN, BLUE;
}
```

编译器编译出的`class`大概就像这样：

```
public final class Color extends Enum { // 继承自Enum，标记为final class
    // 每个实例均为全局唯一:
    public static final Color RED = new Color();
    public static final Color GREEN = new Color();
    public static final Color BLUE = new Color();
    // private构造方法，确保外部无法调用new操作符:
    private Color() {}
}
```

所以，编译后的`enum`类和普通`class`并没有任何区别。但是我们自己无法按定义普通`class`那样来定义`enum`，必须使用`enum`关键字，这是Java语法规定的。

因为`enum`是一个`class`，每个枚举的值都是`class`实例，因此，这些实例有一些方法：

##### name()

返回常量名，例如：

```java
String s = Weekday.SUN.name(); // "SUN"
```

##### ordinal()

返回定义的常量的顺序，从0开始计数，例如：

```
int n = Weekday.MON.ordinal(); // 1
```

改变枚举常量定义的顺序就会导致`ordinal()`返回值发生变化。例如：

```
public enum Weekday {
    SUN, MON, TUE, WED, THU, FRI, SAT;
}
```

和

```
public enum Weekday {
    MON, TUE, WED, THU, FRI, SAT, SUN;
}
```

的`ordinal`就是不同的。如果在代码中编写了类似`if(x.ordinal()==1)`这样的语句，就要保证`enum`的枚举顺序不能变。新增的常量必须放在最后。

##### switch

最后，枚举类可以应用在`switch`语句中。因为枚举类天生具有类型信息和有限个枚举常量，所以比`int`、`String`类型更适合用在`switch`语句中：

~~~java
public class Main {
    public static void main(String[] args) {
        Weekday day = Weekday.SUN;
        switch(day) {
        case MON:
        case TUE:
        case WED:
        case THU:
        case FRI:
            System.out.println("Today is " + day + ". Work at office!");
            break;
        case SAT:
        case SUN:
            System.out.println("Today is " + day + ". Work at home!");
            break;
        default:
            throw new RuntimeException("cannot process " + day);
        }
    }
}

enum Weekday {
    MON, TUE, WED, THU, FRI, SAT, SUN;
}

~~~

### 记录类

使用`String`、`Integer`等类型的时候，这些类型都是不变类，一个不变类具有以下特点：

1. 定义class时使用`final`，无法派生子类；
2. 每个字段使用`final`，保证创建实例后无法修改任何字段。

假设我们希望定义一个`Point`类，有`x`、`y`两个变量，同时它是一个不变类，可以这么写：

```java
public final class Point {
    private final int x;
    private final int y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int x() {
        return this.x;
    }

    public int y() {
        return this.y;
    }
}
```

为了保证不变类的比较，还需要正确覆写`equals()`和`hashCode()`方法，这样才能在集合类中正常使用。后续我们会详细讲解正确覆写`equals()`和`hashCode()`，这里演示`Point`不变类的写法目的是，这些代码写起来都非常简单，但是很繁琐。

#### record

从Java 14开始，引入了新的`Record`类。我们定义`Record`类时，使用关键字`record`。把上述`Point`类改写为`Record`类，代码如下：

~~~java
public class Main {
    public static void main(String[] args) {
        Point p = new Point(123, 456);
        System.out.println(p.x());
        System.out.println(p.y());
        System.out.println(p);
    }
}

public record Point(int x, int y) {}

~~~

### BigInteger

在Java中，由CPU原生提供的整型最大范围是64位`long`型整数。使用`long`型整数可以直接通过CPU指令进行计算，速度非常快。

如果我们使用的整数范围超过了`long`型怎么办？这个时候，就只能用软件来模拟一个大整数。`java.math.BigInteger`**就是用来表示任意大小的整数**。`BigInteger`内部用一个`int[]`数组来模拟一个非常大的整数：

```
BigInteger bi = new BigInteger("1234567890");
System.out.println(bi.pow(5)); // 2867971860299718107233761438093672048294900000
```

对`BigInteger`做运算的时候，只能使用实例方法，例如，加法运算：

```
BigInteger i1 = new BigInteger("1234567890");
BigInteger i2 = new BigInteger("12345678901234567890");
BigInteger sum = i1.add(i2); // 12345678902469135780
```

和`long`型整数运算比，`BigInteger`不会有范围限制，但缺点是速度比较慢。

也可以把`BigInteger`转换成`long`型：

```java
BigInteger i = new BigInteger("123456789000");
System.out.println(i.longValue()); // 123456789000
System.out.println(i.multiply(i).longValueExact()); // java.lang.ArithmeticException: BigInteger out of long range
```

使用`longValueExact()`方法时，如果超出了`long`型的范围，会抛出`ArithmeticException`。

`BigInteger`和`Integer`、`Long`一样，也是不可变类，并且也继承自`Number`类。因为`Number`定义了转换为基本类型的几个方法：

- 转换为`byte`：`byteValue()`
- 转换为`short`：`shortValue()`
- 转换为`int`：`intValue()`
- 转换为`long`：`longValue()`
- 转换为`float`：`floatValue()`
- 转换为`double`：`doubleValue()`

因此，通过上述方法，可以把`BigInteger`转换成基本类型。如果`BigInteger`表示的范围超过了基本类型的范围，转换时将丢失高位信息，即结果不一定是准确的。如果需要准确地转换成基本类型，可以使用`intValueExact()`、`longValueExact()`等方法，在转换时如果超出范围，将直接抛出`ArithmeticException`异常。

#### 小结

- ` BigInteger`用于表示任意大小的整数；

- `BigInteger`是不变类，并且继承自`Number`；

- 将`BigInteger`转换成基本类型时可使用`longValueExact()`等方法保证结果准确。

### BigDecimal

和`BigInteger`类似，`BigDecimal`可以表示一个任意大小且精度完全准确的浮点数。

```java
BigDecimal bd = new BigDecimal("123.4567");
System.out.println(bd.multiply(bd)); // 15241.55677489
```

`BigDecimal`用`scale()`表示小数位数，例如：

```java
BigDecimal d1 = new BigDecimal("123.45");
BigDecimal d2 = new BigDecimal("123.4500");
BigDecimal d3 = new BigDecimal("1234500");
System.out.println(d1.scale()); // 2,两位小数
System.out.println(d2.scale()); // 4
System.out.println(d3.scale()); // 0
```

通过`BigDecimal`的`stripTrailingZeros()`方法，可以将一个`BigDecimal`格式化为一个相等的，但去掉了末尾0的`BigDecimal`：

```java
BigDecimal d1 = new BigDecimal("123.4500");
BigDecimal d2 = d1.stripTrailingZeros();
System.out.println(d1.scale()); // 4
System.out.println(d2.scale()); // 2,因为去掉了00

BigDecimal d3 = new BigDecimal("1234500");
BigDecimal d4 = d3.stripTrailingZeros();
System.out.println(d3.scale()); // 0
System.out.println(d4.scale()); // -2
```

如果一个`BigDecimal`的`scale()`返回负数，例如，`-2`，表示这个数是个整数，并且末尾有2个0。

---

可以对一个`BigDecimal`设置它的`scale`，如果精度比原始值低，那么按照指定的方法进行四舍五入或者直接截断：

~~~java
package demo;

import java.math.BigDecimal;
import java.math.RoundingMode;

public class hello{
    public static void main(String[] args){
        BigDecimal d1=new BigDecimal("123.454556");
        BigDecimal d2=d1.setScale(4, RoundingMode.HALF_UP);//四舍五入，123.4546
        BigDecimal d3=d1.setScale(4,RoundingMode.DOWN);//直接截断，123.4545
        System.out.println(d2);
        System.out.println(d3);
    }
}


~~~

对`BigDecimal`做加、减、乘时，精度不会丢失，但是做除法时，存在无法除尽的情况，这时，就必须指定精度以及如何进行截断：

```java
BigDecimal d1 = new BigDecimal("123.456");
BigDecimal d2 = new BigDecimal("23.456789");
BigDecimal d3 = d1.divide(d2, 10, RoundingMode.HALF_UP); // 保留10位小数并四舍五入
BigDecimal d4 = d1.divide(d2); // 报错：ArithmeticException，因为除不尽
```

还可以对`BigDecimal`做除法的同时求余数：

~~~java
package demo;

import java.math.BigDecimal;
import java.math.RoundingMode;

public class hello{
    public static void main(String[] args){
        BigDecimal d1=new BigDecimal("12.3");
        BigDecimal d2=new BigDecimal("0.12");
        BigDecimal[] dr =d1.divideAndRemainder(d2);

        System.out.println(dr[0]);
        System.out.println(dr[1]);
    }
}

//public record point(int x,int y){}
~~~

调用`divideAndRemainder()`方法时，返回的数组包含两个`BigDecimal`，分别是商和余数，其中商总是整数，余数不会大于除数。我们可以利用这个方法判断两个`BigDecimal`是否是整数倍数：

```java
BigDecimal n = new BigDecimal("12.75");
BigDecimal m = new BigDecimal("0.15");
BigDecimal[] dr = n.divideAndRemainder(m);
if (dr[1].signum() == 0) {
    // n是m的整数倍
}
```

#### 比较BigDecimal

在比较两个`BigDecimal`的值是否相等时，要特别注意，使用`equals()`方法不但要求两个`BigDecimal`的值相等，还要求它们的`scale()`相等：

```java
BigDecimal d1 = new BigDecimal("123.456");
BigDecimal d2 = new BigDecimal("123.45600");
System.out.println(d1.equals(d2)); // false,因为scale不同
System.out.println(d1.equals(d2.stripTrailingZeros())); // true,因为d2去除尾部0后scale变为2
System.out.println(d1.compareTo(d2)); // 0
```

必须使用`compareTo()`方法来比较，它根据两个值的大小分别返回负数、正数和`0`，分别表示小于、大于和等于。

### 常用工具类

#### Math

~~~java
Math.abs(-11);//11
Math.max(100,10);//100
Math.min(100,10);//10
Main.pow(2,2);//4
Main.sqrt(2);//1.414...
Main.exp(2);//7.389
Main.log(4);//以e为底的对数
Main.log10(100);//以10为底的对数
Math.sin(3.14); // 0.00159...
Math.cos(3.14); // -0.9999...
Math.tan(3.14); // -0.0015...
Math.asin(1.0); // 1.57079...
Math.acos(1.0); // 0.0


double pi = Math.PI; // 3.14159...
double e = Math.E; // 2.7182818...
Math.sin(Math.PI / 6); // sin(π/6) = 0.5

~~~

生成一个随机数x，x的范围是`0 <= x < 1`：

```
Math.random(); // 0.53907... 每次都不一样
```

如果我们要生成一个区间在`[MIN, MAX)`的随机数，可以借助`Math.random()`实现，计算如下：

~~~java
public class Main{
    public static void main(String[] args){
        double x=Math.random();//[0,1)
        double min=10;
        doublemax=50;
        double y=x*(max-min)+min;//[10,50)
        
        long n=(long)y;
        System.out.println(y);
        System.out.println(n);
    }
}
~~~

#### Random

`Random`用来创建伪随机数。所谓伪随机数，是指只要给定一个初始的种子，产生的随机数序列是完全一样的。

要生成一个随机数，可以使用`nextInt()`、`nextLong()`、`nextFloat()`、`nextDouble()`：

~~~java
Random r = new Random();
r.nextInt(); // 2071575453,每次都不一样
r.nextInt(10); // 5,生成一个[0,10)之间的int
r.nextLong(); // 8811649292570369305,每次都不一样
r.nextFloat(); // 0.54335...生成一个[0,1)之间的float
r.nextDouble(); // 0.3716...生成一个[0,1)之间的double
~~~

有童鞋问，每次运行程序，生成的随机数都是不同的，没看出*伪随机数*的特性来。

这是因为我们创建`Random`实例时，如果不给定种子，就使用系统当前时间戳作为种子，因此每次运行时，种子不同，得到的伪随机数序列就不同。

如果我们在创建`Random`实例时指定一个种子，就会得到完全确定的随机数序列：

```java
package demo;

import java.math.BigDecimal;
import java.math.RoundingMode;
import java.util.Random;

public class hello{
    public static void main(String[] args){
        Random r=new Random(123456);
        for (int i=0;i<10;i++){
            System.out.println(r.nextInt(100));
        }


    }
}


```

前面我们使用的`Math.random()`实际上内部调用了`Random`类，所以它也是伪随机数，只是我们无法指定种子。

#### SecureRandom

有伪随机数，就有真随机数。实际上真正的真随机数只能通过量子力学原理来获取，而我们想要的是一个不可预测的安全的随机数，`SecureRandom`就是用来创建安全的随机数的：

~~~java
SecureRandom sr=new SecureRandom();
System.out.println(sr.nextInt(100));
~~~

`SecureRandom`无法指定种子，它使用RNG（random number generator）算法。JDK的`SecureRandom`实际上有多种不同的底层实现，有的使用安全随机种子加上伪随机数算法来产生安全的随机数，有的使用真正的随机数生成器。实际使用的时候，可以优先获取高强度的安全随机数生成器，如果没有提供，再使用普通等级的安全随机数生成器

---



# 异常处理

- 异常处理的任务就是将控制权从错误产生的地方转移给能够处理这种情况的错误处理器。

## java的异常

在计算机程序运行的过程中，总是会出现各种各样的错误。

有一些错误是用户造成的，比如，希望用户输入一个`int`类型的年龄，但是用户的输入是`abc`：

```
// 假设用户输入了abc：
String s = "abc";
int n = Integer.parseInt(s); // NumberFormatException!
```

程序想要读写某个文件的内容，但是用户已经把它删除了：

```
// 用户删除了该文件：
String t = readFile("C:\\abc.txt"); // FileNotFoundException!
```

还有一些错误是随机出现，并且永远不可能避免的。比如：

- 网络突然断了，连接不到远程服务器；
- 内存耗尽，程序崩溃了；
- 用户点“打印”，但根本没有打印机；
- ……

所以，一个健壮的程序必须处理各种各样的错误。

所谓错误，就是程序调用某个函数的时候，如果失败了，就表示出错。

**调用方如何获知调用失败的信息？有两种方法：**

**方法一：约定返回错误码。**

例如，处理一个文件，如果返回`0`，表示成功，返回其他整数，表示约定的错误码：

```
int code = processFile("C:\\test.txt");
if (code == 0) {
    // ok:
} else {
    // error:
    switch (code) {
    case 1:
        // file not found:
    case 2:
        // no read permission:
    default:
        // unknown error:
    }
}
```

因为使用`int`类型的错误码，想要处理就非常麻烦。这种方式常见于底层C函数。

**方法二：在语言层面上提供一个异常处理机制。**

Java内置了一套异常处理机制，总是使用异常来表示错误。

异常是一种`class`，因此它本身带有类型信息。异常可以在任何地方抛出，但只需要在上层捕获，这样就和方法调用分离了：

```
try {
    String s = processFile(“C:\\test.txt”);
    // ok:
} catch (FileNotFoundException e) {
    // file not found:
} catch (SecurityException e) {
    // no read permission:
} catch (IOException e) {
    // io error:
} catch (Exception e) {
    // other error:
}
```

因为Java的异常是`class`，它的继承关系如下：

```ascii
                     ┌───────────┐
                     │  Object   │
                     └───────────┘
                           ▲
                           │
                     ┌───────────┐
                     │ Throwable │
                     └───────────┘
                           ▲
                 ┌─────────┴─────────┐
                 │                   │
           ┌───────────┐       ┌───────────┐
           │   Error   │       │ Exception │
           └───────────┘       └───────────┘
                 ▲                   ▲
         ┌───────┘              ┌────┴──────────┐
         │                      │               │
┌─────────────────┐    ┌─────────────────┐┌───────────┐
│OutOfMemoryError │... │RuntimeException ││IOException│...
└─────────────────┘    └─────────────────┘└───────────┘
                                ▲
                    ┌───────────┴─────────────┐
                    │                         │
         ┌─────────────────────┐ ┌─────────────────────────┐
         │NullPointerException │ │IllegalArgumentException │...
         └─────────────────────┘ └─────────────────────────┘
```

从继承关系可知：`Throwable`是异常体系的根，它继承自`Object`。`Throwable`有两个体系：`Error`和`Exception`，`Error`表示严重的错误，程序对此一般无能为力，例如：

- `OutOfMemoryError`：内存耗尽
- `NoClassDefFoundError`：无法加载某个Class
- `StackOverflowError`：栈溢出

而`Exception`则是运行时的错误，它可以被捕获并处理。

某些异常是应用程序逻辑处理的一部分，应该捕获并处理。例如：

- `NumberFormatException`：数值类型的格式错误
- `FileNotFoundException`：未找到文件
- `SocketException`：读取网络失败

`Exception`又分为两大类：

1. `RuntimeException`以及它的子类；
2. 非`RuntimeException`（包括`IOException`、`ReflectiveOperationException`等等）

Java规定：

- **必须捕获的异常，包括`Exception`及其子类，但不包括`RuntimeException`及其子类，这种类型的异常称为Checked Exception。**
- 不需要捕获的异常，包括`Error`及其子类，`RuntimeException`及其子类。

 注意：编译器对RuntimeException及其子类不做强制捕获要求，不是指应用程序本身不应该捕获并处理RuntimeException。是否需要捕获，具体问题具体分析。

- 派生于Error或RuntimeException类的所有异常称为非受查异常，所有其他的异常受查异常
- 编译器为受查异常提供了异常处理器

#### 捕获异常

捕获异常使用`try...catch`语句，把可能发生异常的代码放到`try {...}`中，然后使用`catch`捕获对应的`Exception`及其子类

~~~java
public class Main {
    public static void main(String[] args) {
        byte[] bs = toGBK("中文");
        System.out.println(Arrays.toString(bs));
    }

    static byte[] toGBK(String s) {
        try {
            // 用指定编码转换String为byte[]:
            return s.getBytes("GBK");
        } catch (UnsupportedEncodingException e) {
            // 如果系统不支持GBK编码，会捕获到UnsupportedEncodingException:
            System.out.println(e); // 打印异常信息
            return s.getBytes(); // 尝试使用用默认编码
        }
    }
}

~~~

如果我们不捕获`UnsupportedEncodingException`，会出现编译失败的问题：

~~~java
public class Main {
    public static void main(String[] args) {
        byte[] bs = toGBK("中文");
        System.out.println(Arrays.toString(bs));
    }

    static byte[] toGBK(String s) {
        return s.getBytes("GBK");
    }
}

~~~

编译器会报错，错误信息类似：unreported exception UnsupportedEncodingException; must be caught or declared to be thrown，并且准确地指出需要捕获的语句是`return s.getBytes("GBK");`。意思是说，**像`UnsupportedEncodingException`这样的Checked Exception，必须被捕获。**

这是因为`String.getBytes(String)`方法定义是：

~~~java
public byte[] getBytes(String charsetName) throws UnsupportedEncodingException {
    ...
}
~~~

<u>在方法定义的时候，使用`throws Xxx`表示该方法可能抛出的异常类型。调用方在调用的时候，必须强制捕获这些异常，否则编译器会报错。</u>

在`toGBK()`方法中，因为调用了`String.getBytes(String)`方法，就必须捕获`UnsupportedEncodingException`。我们也可以不捕获它，而是在方法定义处用throws表示`toGBK()`方法可能会抛出`UnsupportedEncodingException`，就可以让`toGBK()`方法通过编译器检查：

~~~java
public class Main {
    public static void main(String[] args) {
        byte[] bs = toGBK("中文");
        System.out.println(Arrays.toString(bs));
    }

    static byte[] toGBK(String s) throws UnsupportedEncodingException {
        return s.getBytes("GBK");
    }
}

~~~

上述代码仍然会得到编译错误，但这一次，编译器提示的不是调用`return s.getBytes("GBK");`的问题，而是`byte[] bs = toGBK("中文");`。因为在`main()`方法中，调用`toGBK()`，没有捕获它声明的可能抛出的`UnsupportedEncodingException`。

修复方法是在`main()`方法中捕获异常并处理：

~~~java
package demo;

import java.io.UnsupportedEncodingException;
import java.util.Arrays;


public class hello {
    public static void main(String[] args) throws UnsupportedEncodingException {
        byte[] bs = toGBK("中文");
        System.out.println(Arrays.toString(bs));

    }

    private static byte[] toGBK(String s) throws UnsupportedEncodingException{
        return s.getBytes("GBK");
    }
}

~~~

可见，<u>只要是方法声明的Checked Exception，不在调用层捕获，也必须在更高的调用层捕获。所有未捕获的异常，最终也必须在`main()`方法中捕获，不会出现漏写`try`的情况</u>。这是由编译器保证的。`main()`方法也是最后捕获`Exception`的机会。

---

如果是测试代码，上面的写法就略显麻烦。如果不想写任何`try`代码，可以直接把`main()`方法定义为`throws Exception`：

~~~java
public class Main {
    public static void main(String[] args) throws Exception {
        byte[] bs = toGBK("中文");
        System.out.println(Arrays.toString(bs));
    }

    static byte[] toGBK(String s) throws UnsupportedEncodingException {
        // 用指定编码转换String为byte[]:
        return s.getBytes("GBK");
    }
}

~~~

因为`main()`方法声明了可能抛出`Exception`，也就**声明了可能抛出所有的`Exception`**，因此在内部就无需捕获了。代价就是一旦发生异常，程序会立刻退出。

还有一些童鞋喜欢在`toGBK()`内部“消化”异常：

```
static byte[] toGBK(String s) {
    try {
        return s.getBytes("GBK");
    } catch (UnsupportedEncodingException e) {
        // 什么也不干
    }
    return null;
```

这种捕获后不处理的方式是非常不好的，即使真的什么也做不了，也要先把异常记录下来：

```
static byte[] toGBK(String s) {
    try {
        return s.getBytes("GBK");
    } catch (UnsupportedEncodingException e) {
        // 先记下来再说:
        e.printStackTrace();
    }
    return null;
```

**所有异常都可以调用`printStackTrace()`方法打印异常栈，这是一个简单有用的快速打印异常的方法。**

#### 小结

Java使用异常来表示错误，并通过`try ... catch`捕获异常；

Java的异常是`class`，并且从`Throwable`继承；

`Error`是无需捕获的严重错误，`Exception`是应该捕获的可处理的错误；

`RuntimeException`无需强制捕获，非`RuntimeException`（Checked Exception）需强制捕获，或者用`throws`声明；

不推荐捕获了异常但不进行任何处理。

### 声明异常

声明受查异常：

- 方法应该在首部声明(throws)所有可能抛出的异常。这样可以从首部反映出这个方法可能抛出哪类受查异常。（告诉编译器可能发生什么错误）

  ~~~java
  public FileInputStream(String name) throws FileNotFoundException
  ~~~

~~~java
class Person{
	...
    public Image loadImage(String s) throws IOException{
        
    }
}
~~~

- 总之，一个方法必须声明所有可能抛出的受查异常，而非受查异常要么不可控（Error）,要么应该避免发生。



---

### 捕获异常

在Java中，凡是可能抛出异常的语句，都可以用`try ... catch`捕获。**把可能发生异常的语句放在`try { ... }`中，然后使用`catch`捕获对应的`Exception`及其子类。**

### 多catch语句

可以使用多个`catch`语句，每个`catch`分别捕获对应的`Exception`及其子类。JVM在捕获到异常后，会从上到下匹配`catch`语句，匹配到某个`catch`后，执行`catch`代码块，然后*不再*继续匹配。

简单地说就是：多个`catch`语句只有一个能被执行。例如：

```
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
    } catch (IOException e) {
        System.out.println(e);
    } catch (NumberFormatException e) {
        System.out.println(e);
    }
}
```

存在多个`catch`的时候，`catch`的顺序非常重要：子类必须写在前面。例如：

```
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
    } catch (IOException e) {
        System.out.println("IO error");
    } catch (UnsupportedEncodingException e) { // 永远捕获不到
        System.out.println("Bad encoding");
    }
}
```

对于上面的代码，`UnsupportedEncodingException`异常是永远捕获不到的，因为它是`IOException`的子类。当抛出`UnsupportedEncodingException`异常时，会被`catch (IOException e) { ... }`捕获并执行。

因此，正确的写法是把子类放到前面：

```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
    } catch (UnsupportedEncodingException e) {
        System.out.println("Bad encoding");
    } catch (IOException e) {
        System.out.println("IO error");
    }
}
```

### finally语句

无论是否有异常发生，如果我们都希望执行一些语句，例如清理工作，怎么写？

可以把执行语句写若干遍：正常执行的放到`try`中，每个`catch`再写一遍。例如：

```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
        System.out.println("END");
    } catch (UnsupportedEncodingException e) {
        System.out.println("Bad encoding");
        System.out.println("END");
    } catch (IOException e) {
        System.out.println("IO error");
        System.out.println("END");
    }
}
```

上述代码无论是否发生异常，都会执行`System.out.println("END");`这条语句。

那么如何消除这些重复的代码？Java的`try ... catch`机制还提供了`finally`语句，**`finally`语句块保证有无错误都会执行**。上述代码可以改写如下：

```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
    } catch (UnsupportedEncodingException e) {
        System.out.println("Bad encoding");
    } catch (IOException e) {
        System.out.println("IO error");
    } finally {
        System.out.println("END");
    }
}
```

注意`finally`有几个特点：

1. `finally`语句不是必须的，可写可不写；
2. `finally`总是最后执行。

如果没有发生异常，就正常执行`try { ... }`语句块，然后执行`finally`。如果发生了异常，就中断执行`try { ... }`语句块，然后跳转执行匹配的`catch`语句块，最后执行`finally`。

**可见，`finally`是用来保证一些代码必须执行的**

---

某些情况下，可以没有`catch`，只使用`try ... finally`结构。例如：

```java
void process(String file) throws IOException {
    try {
        ...
    } finally {
        System.out.println("END");
    }
}
```

因为方法声明了可能抛出的异常，所以可以不写`catch`。

### 捕获多种异常

如果某些异常的处理逻辑相同，但是异常本身不存在继承关系，那么就得编写多条`catch`子句：

```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
    } catch (IOException e) {
        System.out.println("Bad input");
    } catch (NumberFormatException e) {
        System.out.println("Bad input");
    } catch (Exception e) {
        System.out.println("Unknown error");
    }
}
```

因为处理`IOException`和`NumberFormatException`的代码是相同的，所以我们可以把它两用`|`合并到一起：

```java
public static void main(String[] args) {
    try {
        process1();
        process2();
        process3();
    } catch (IOException | NumberFormatException e) { // IOException或NumberFormatException
        System.out.println("Bad input");
    } catch (Exception e) {
        System.out.println("Unknown error");
    }
}
```



练习

~~~java
package demo;


public class hello {
    public static void main(String[] args) {
        String a = "12";
        String b = "x9";
        try {
            int c = stringToInt(a);
            int d = stringToInt(b);
            System.out.println(c * d);

        } catch (NumberFormatException q) {
            System.out.println(q);
        }
        // TODO: 捕获异常并处理


    }

    static int stringToInt(String s) {
        return Integer.parseInt(s);
    }
    
}

~~~

## 抛出异常

### 异常的传播

当某个方法抛出了异常时，如果当前方法没有捕获异常，异常就会被抛到上层调用方法，直到遇到某个`try ... catch`被捕获为止：

~~~java
public class Main {
    public static void main(String[] args) {
        try {
            process1();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    static void process1() {
        process2();
    }

    static void process2() {
        Integer.parseInt(null); // 会抛出NumberFormatException
    }
}

~~~

通过`printStackTrace()`可以打印出方法的调用栈，类似：

```java
java.lang.NumberFormatException: null
    at java.base/java.lang.Integer.parseInt(Integer.java:614)
    at java.base/java.lang.Integer.parseInt(Integer.java:770)
    at Main.process2(Main.java:16)
    at Main.process1(Main.java:12)
    at Main.main(Main.java:5)
```

`printStackTrace()`对于调试错误非常有用，上述信息表示：`NumberFormatException`是在`java.lang.Integer.parseInt`方法中被抛出的，从下往上看，调用层次依次是：

1. `main()`调用`process1()`；
2. `process1()`调用`process2()`；
3. `process2()`调用`Integer.parseInt(String)`；
4. `Integer.parseInt(String)`调用`Integer.parseInt(String, int)`。

查看`Integer.java`源码可知，抛出异常的方法代码如下：

```
public static int parseInt(String s, int radix) throws NumberFormatException {
    if (s == null) {
        throw new NumberFormatException("null");
    }
    ...
}
```

并且，每层调用均给出了源代码的行号，可直接定位。

### 抛出异常

当发生错误时，例如，用户输入了非法的字符，我们就可以抛出异常。

如何抛出异常？参考`Integer.parseInt()`方法，抛出异常分两步：

1. 创建某个`Exception`的实例；
2. 用`throw`语句抛出。

下面是一个例子：

```java
void process2(String s) {
    if (s==null) {
        NullPointerException e = new NullPointerException();
        throw e;
    }
}
```

实际上，绝大部分抛出异常的代码都会合并写成一行：

```java
void process2(String s) {
    if (s==null) {
        throw new NullPointerException();
    }
}
```

如果一个方法捕获了某个异常后，又在`catch`子句中抛出新的异常，就相当于把抛出的异常类型“转换”了：

```java
void process1(String s) {
    try {
        process2();
    } catch (NullPointerException e) {
        throw new IllegalArgumentException();
    }
}

void process2(String s) {
    if (s==null) {
        throw new NullPointerException();
    }
}
```

当`process2()`抛出`NullPointerException`后，被`process1()`捕获，然后抛出`IllegalArgumentException()`。

---

如果一个方法捕获了某个异常后，又在`catch`子句中抛出新的异常，就相当于把抛出的异常类型“转换”了：

```
void process1(String s) {
    try {
        process2();
    } catch (NullPointerException e) {
        throw new IllegalArgumentException();
    }
}

void process2(String s) {
    if (s==null) {
        throw new NullPointerException();
    }
}
```

当`process2()`抛出`NullPointerException`后，被`process1()`捕获，然后抛出`IllegalArgumentException()`。

如果在`main()`中捕获`IllegalArgumentException`，我们看看打印的异常栈：

~~~java
如果一个方法捕获了某个异常后，又在catch子句中抛出新的异常，就相当于把抛出的异常类型“转换”了：

void process1(String s) {
    try {
        process2();
    } catch (NullPointerException e) {
        throw new IllegalArgumentException();
    }
}

void process2(String s) {
    if (s==null) {
        throw new NullPointerException();
    }
}

当process2()抛出NullPointerException后，被process1()捕获，然后抛出IllegalArgumentException()。

如果在main()中捕获IllegalArgumentException，我们看看打印的异常栈：
~~~



打印出的异常栈类似：

```
java.lang.IllegalArgumentException
    at Main.process1(Main.java:15)
    at Main.main(Main.java:5)
```

这说明新的异常丢失了原始异常信息，我们已经看不到原始异常`NullPointerException`的信息了。

为了能追踪到完整的异常栈，在构造异常的时候，把原始的`Exception`实例传进去，新的`Exception`就可以持有原始`Exception`信息。对上述代码改进如下：

~~~java
public class Main {
    public static void main(String[] args) {
        try {
            process1();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    static void process1() {
        try {
            process2();
        } catch (NullPointerException e) {
            throw new IllegalArgumentException(e);
        }
    }

    static void process2() {
        throw new NullPointerException();
    }
}

~~~

运行上述代码，打印出的异常栈类似：

```
java.lang.IllegalArgumentException: java.lang.NullPointerException
    at Main.process1(Main.java:15)
    at Main.main(Main.java:5)
Caused by: java.lang.NullPointerException
    at Main.process2(Main.java:20)
    at Main.process1(Main.java:13)
```

注意到`Caused by: Xxx`，说明捕获的`IllegalArgumentException`并不是造成问题的根源，根源在于`NullPointerException`，是在`Main.process2()`方法抛出的。

在代码中获取原始异常可以使用`Throwable.getCause()`方法。如果返回`null`，说明已经是“根异常”了。

有了完整的异常栈的信息，我们才能快速定位并修复代码的问题。

 捕获到异常并再次抛出时，一定要留住原始异常，否则很难定位第一案发现场！

如果我们在`try`或者`catch`语句块中抛出异常，`finally`语句是否会执行？例如：

~~~java
public class Main {
    public static void main(String[] args) {
        try {
            Integer.parseInt("abc");
        } catch (Exception e) {
            System.out.println("catched");
            throw new RuntimeException(e);
        } finally {
            System.out.println("finally");
        }
    }
}

~~~

上述代码执行结果如下：

```
catched
finally
Exception in thread "main" java.lang.RuntimeException: java.lang.NumberFormatException: For input string: "abc"
    at Main.main(Main.java:8)
Caused by: java.lang.NumberFormatException: For input string: "abc"
    at ...
```

第一行打印了`catched`，说明进入了`catch`语句块。第二行打印了`finally`，说明执行了`finally`语句块。

**因此，在`catch`中抛出异常，不会影响`finally`的执行。JVM会先执行`finally`，然后抛出异常。**

### 异常屏蔽

如果在执行`finally`语句时抛出异常，那么，`catch`语句的异常还能否继续抛出？例如：

~~~java
public class Main {
    public static void main(String[] args) {
        try {
            Integer.parseInt("abc");
        } catch (Exception e) {
            System.out.println("catched");
            throw new RuntimeException(e);
        } finally {
            System.out.println("finally");
            throw new IllegalArgumentException();
        }
    }
}

~~~

执行上述代码，发现异常信息如下：

```
catched
finally
Exception in thread "main" java.lang.IllegalArgumentException
    at Main.main(Main.java:11)
```

这说明`finally`抛出异常后，原来在`catch`中准备抛出的异常就“消失”了，因为只能抛出一个异常。没有被抛出的异常称为“被屏蔽”的异常（Suppressed Exception）。

在极少数的情况下，我们需要获知所有的异常。如何保存所有的异常信息？方法是先用`origin`变量保存原始异常，然后调用`Throwable.addSuppressed()`，把原始异常添加进来，最后在`finally`抛出：

~~~java
public class Main {
    public static void main(String[] args) throws Exception {
        Exception origin = null;
        try {
            System.out.println(Integer.parseInt("abc"));
        } catch (Exception e) {
            origin = e;
            throw e;
        } finally {
            Exception e = new IllegalArgumentException();
            if (origin != null) {
                e.addSuppressed(origin);
            }
            throw e;
        }
    }
}

~~~

当`catch`和`finally`都抛出了异常时，虽然`catch`的异常被屏蔽了，但是，`finally`抛出的异常仍然包含了它：

```
Exception in thread "main" java.lang.IllegalArgumentException
    at Main.main(Main.java:11)
Suppressed: java.lang.NumberFormatException: For input string: "abc"
    at java.base/java.lang.NumberFormatException.forInputString(NumberFormatException.java:65)
    at java.base/java.lang.Integer.parseInt(Integer.java:652)
    at java.base/java.lang.Integer.parseInt(Integer.java:770)
    at Main.main(Main.java:6)
```

通过`Throwable.getSuppressed()`可以获取所有的`Suppressed Exception`。

绝大多数情况下，在`finally`中不要抛出异常。因此，我们通常不需要关心`Suppressed Exception`。

练习

~~~java
package demo;


public class hello {
    public static void main(String[] args) {
        try {
            System.out.println(tax(2000, 0.1));//可能发生异常的语句
//            System.out.println(tax(-200, 0.1));
//            System.out.println(tax(2000, -0.1));
        } catch (Exception e) {//捕获异常
            System.out.println("error");
        }
    }

    static double tax(int salary, double rate) {
        // TODO: 如果传入的参数为负，则抛出IllegalArgumentException
        if (salary < 0 || rate < 0) {
            throw new IllegalArgumentException();//抛出异常
        } else {
            return salary * rate;
        }


    }

}
~~~

## 自定义异常

Java标准库定义的常用异常包括：

```ascii
Exception
│
├─ RuntimeException
│  │
│  ├─ NullPointerException
│  │
│  ├─ IndexOutOfBoundsException
│  │
│  ├─ SecurityException
│  │
│  └─ IllegalArgumentException
│     │
│     └─ NumberFormatException
│
├─ IOException
│  │
│  ├─ UnsupportedCharsetException
│  │
│  ├─ FileNotFoundException
│  │
│  └─ SocketException
│
├─ ParseException
│
├─ GeneralSecurityException
│
├─ SQLException
│
└─ TimeoutException
```

当我们在代码中需要抛出异常时，尽量使用JDK已定义的异常类型。例如，参数检查不合法，应该抛出`IllegalArgumentException`：

```java
static void process1(int age) {
    if (age <= 0) {
        throw new IllegalArgumentException();
    }
}
```

在一个大型项目中，可以自定义新的异常类型，但是，保持一个合理的异常继承体系是非常重要的。

一个常见的做法是自定义一个`BaseException`作为“根异常”，然后，派生出各种业务类型的异常。

`BaseException`需要从一个适合的`Exception`派生，通常建议从`RuntimeException`派生：

```
public class BaseException extends RuntimeException {
}
```

其他业务类型的异常就可以从`BaseException`派生：

```
public class UserNotFoundException extends BaseException {
}

public class LoginFailedException extends BaseException {
}

...
```

自定义的`BaseException`应该提供多个构造方法：

```
public class BaseException extends RuntimeException {
    public BaseException() {
        super();
    }

    public BaseException(String message, Throwable cause) {
        super(message, cause);
    }

    public BaseException(String message) {
        super(message);
    }

    public BaseException(Throwable cause) {
        super(cause);
    }
}
```

上述构造方法实际上都是原样照抄`RuntimeException`。这样，抛出异常的时候，就可以选择合适的构造方法。通过IDE可以根据父类快速生成子类的构造方法。

## NullPointException

在所有的`RuntimeException`异常中，Java程序员最熟悉的恐怕就是`NullPointerException`了。

`NullPointerException`即空指针异常，俗称NPE。如果一个对象为`null`，调用其方法或访问其字段就会产生`NullPointerException`，这个异常通常是由JVM抛出的，例如：

~~~java
public class Main{
    public static void main(String[] args){
        String s= null;
        System.out.println(s.toLowerCase())
    }
}

/*
Exception in thread "main" java.lang.NullPointerException: Cannot invoke "String.toLowerCase()" because "<local1>" is null
at Main.main(Main.java:5) 
*/
~~~

指针这个概念实际上源自C语言，Java语言中并无指针。我们定义的变量实际上是引用，Null Pointer更确切地说是Null Reference，不过两者区别不大。

#### 处理NullPointerException

如果遇到`NullPointerException`，我们应该如何处理？首先，必须明确，`NullPointerException`是一种代码逻辑错误，遇到`NullPointerException`，遵循原则是早暴露，早修复，严禁使用`catch`来隐藏这种编码错误：

~~~java
// 错误示例: 捕获NullPointerException
try {
    transferMoney(from, to, amount);
} catch (NullPointerException e) {
}
~~~

好的编码习惯可以极大地降低`NullPointerException`的产生，例如：

成员变量在定义时初始化：

~~~java
public class Person{
    private String name="";
}
~~~

使用空字符串`""`而不是默认的`null`可避免很多`NullPointerException`，编写业务逻辑时，用空字符串`""`表示未填写比`null`安全得多。

**返回空字符串`""`、空数组而不是`null`：**

~~~java
public String[] readLineFromFile(String file){
    if (getFileSize(file)==0){
        //返回空数组而不是null
        return new String[0];
    }
}
~~~

这样可以使得调用方无需检查结果是否为`null`。

如果调用方一定要根据`null`判断，比如返回`null`表示文件不存在，那么考虑返回`Optional<T>`：

~~~java
public Optional<String> readFromFile(String file){
    if(!fileExit(file)){
        return Optional.empty();
    }
    ...
}
~~~

这样调用方必须通过`Optional.isPresent()`判断是否有结果.

#### 定位NullPointerException

如果产生了`NullPointerException`，例如，调用`a.b.c.x()`时产生了`NullPointerException`，原因可能是：

- `a`是`null`；
- `a.b`是`null`；
- `a.b.c`是`null`；

确定到底是哪个对象是`null`以前只能打印这样的日志：

```
System.out.println(a);
System.out.println(a.b);
System.out.println(a.b.c);
```

从Java 14开始，如果产生了`NullPointerException`，JVM可以给出详细的信息告诉我们`null`对象到底是谁。我们来看例子：

~~~java
public class Main{
    public static void main(String[] args){
        Person p =new Person();
        System.out.println(p.address.city.toLowerCase());
    }
}

class Person{
    String[] name =new String[2];
    Address address= new Address();
}

class Address{
    String city;
    String street;
    String zip;
}

/*
Exception in thread "main" java.lang.NullPointerException: Cannot invoke "String.toLowerCase()" because "<local1>.address.city" is null
at Main.main(Main.java:5) 
*/
~~~

可以在`NullPointerException`的详细信息中看到类似`... because "<local1>.address.city" is null`，意思是`city`字段为`null`，这样我们就能快速定位问题所在。

## 使用断言

断言（Assertion）是一种调试程序的方式。在Java中，使用`assert`关键字来实现断言。

我们先看一个例子：

~~~java
public static void main(String[] args){
    double x=Math.abs(-123.45);
    assert x>=0;
    System.out.println(x);
}
~~~

**语句`assert x >= 0;`即为断言，断言条件`x >= 0`预期为`true`。如果计算结果为`false`，则断言失败，抛出`AssertionError`。**

使用`assert`语句时，还可以添加一个可选的断言消息：

```java
assert x >= 0 : "x must >= 0";
```

这样，断言失败的时候，`AssertionError`会带上消息`x must >= 0`，更加便于调试。

Java断言的特点是：断言失败时会抛出`AssertionError`，导致程序结束退出。因此，**断言不能用于可恢复的程序错误，只应该用于开发和测试阶段**。

对于可恢复的程序错误，不应该使用断言。例如：

```
void sort(int[] arr) {
    assert arr != null;
}
```

应该抛出异常并在上层捕获：

```java
void sort(int[] arr) {
    if (x == null) {
        throw new IllegalArgumentException("array cannot be null");
    }
}
```

~~~java
public class Main {
    public static void main(String[] args) {
        int x = -1;
        assert x > 0;
        System.out.println(x);
    }
}
//-1
~~~

断言`x`必须大于`0`，实际上`x`为`-1`，断言肯定失败。执行上述代码，发现程序并未抛出`AssertionError`，而是正常打印了`x`的值。

这是怎么肥四？为什么`assert`语句不起作用？

这是因为JVM默认关闭断言指令，即遇到`assert`语句就自动忽略了，不执行。

要执行`assert`语句，必须给Java虚拟机传递`-enableassertions`（可简写为`-ea`）参数启用断言。所以，上述程序必须在命令行下运行才有效果：

```
$ java -ea Main.java
Exception in thread "main" java.lang.AssertionError
	at Main.main(Main.java:5)
```

还可以有选择地对特定地类启用断言，命令行参数是：`-ea:com.itranswarp.sample.Main`，表示只对`com.itranswarp.sample.Main`这个类启用断言。

或者对特定地包启用断言，命令行参数是：`-ea:com.itranswarp.sample...`（注意结尾有3个`.`），表示对`com.itranswarp.sample`这个包启动断言。

实际开发中，很少使用断言。更好的方法是编写单元测试，后续我们会讲解`JUnit`的使用。



## 使用JDK Logging

在编写程序的过程中，发现程序运行结果与预期不符，怎么办？当然是用`System.out.println()`打印出执行过程中的某些变量，观察每一步的结果与代码逻辑是否符合，然后有针对性地修改代码。

代码改好了怎么办？当然是删除没有用的`System.out.println()`语句了。

如果改代码又改出问题怎么办？再加上`System.out.println()`。

反复这么搞几次，很快大家就发现使用`System.out.println()`非常麻烦。

怎么办？

解决方法是使用日志。

那什么是日志？**日志就是Logging，它的目的是为了取代`System.out.println()`。**

输出日志，而不是用`System.out.println()`，有以下几个好处：

1. 可以设置输出样式，避免自己每次都写`"ERROR: " + var`；
2. 可以设置输出级别，禁止某些级别输出。例如，只输出错误日志；
3. 可以被重定向到文件，这样可以在程序运行结束后查看日志；
4. 可以按包名控制日志级别，只输出某些包打的日志；
5. 可以……

总之就是好处很多啦。

那如何使用日志？

因为Java标准库内置了日志包`java.util.logging`，我们可以直接用。先看一个简单的例子：

~~~java
package demo;

import java.util.logging.Logger;

public class Main {
    public static void main(String[] args) {
        Logger logger =Logger.getGlobal();
        logger.info("start process...");
        logger.warning("memory is running out..");
        logger.fine("ignored.");
        logger.severe("process will be terminated...");
    }
}
/*
9月 13, 2020 3:47:33 下午 demo.Main main
信息: start process...
9月 13, 2020 3:47:34 下午 demo.Main main
警告: memory is running out..
9月 13, 2020 3:47:34 下午 demo.Main main
严重: process will be terminated...
*/
~~~

对比可见，使用日志最大的好处是，它**自动打印了时间、调用类、调用方法等很多有用的信息**。

再仔细观察发现，4条日志，只打印了3条，`logger.fine()`没有打印。这是因为，日志的输出可以设定级别。JDK的Logging定义了7个日志级别，从严重到普通：

- SEVERE
- WARNING
- INFO
- CONFIG
- FINE
- FINER
- FINEST

因为默认级别是INFO，因此，**INFO级别以下的日志，不会被打印出来**。使用日志级别的好处在于，调整级别，就可以屏蔽掉很多调试相关的日志输出。

使用Java标准库内置的Logging有以下局限：

Logging系统在JVM启动时读取配置文件并完成初始化，一旦开始运行`main()`方法，就无法修改配置；

配置不太方便，需要在JVM启动时传递参数`-Djava.util.logging.config.file=<config-file-name>`。

因此，Java标准库内置的Logging使用并不是非常广泛。更方便的日志系统我们稍后介绍。

练习：

使用logger.severe()打印异常：

~~~java
package demo;

import java.io.UnsupportedEncodingException;
import java.util.logging.Logger;

public class Main {
    public static void main(String[] args) {
        Logger logger =Logger.getLogger(Main.class.getName());
        logger.info("start process...");
        try{
            "".getBytes("invalidCharsetName");
        }catch (UnsupportedEncodingException e){
            logger.severe("process will be terminated...");
            e.printStackTrace();
        }
        logger.info("process end.");
    }
}

~~~

## 使用Commons Logging

和Java标准库提供的日志不同，Commons Logging是一个第三方日志库，它是由Apache创建的日志模块。

Commons Logging的特色是，它可以挂接不同的日志系统，并通过配置文件指定挂接的日志系统。默认情况下，Commons Loggin自动搜索并使用Log4j（Log4j是另一个流行的日志系统），如果没有找到Log4j，再使用JDK Logging。

使用Commons Logging只需要和两个类打交道，并且只有两步：

**第一步，通过`LogFactory`获取`Log`类的实例； 第二步，使用`Log`实例的方法打日志。**

~~~java

~~~

## 使用Log4j

前面介绍了Commons Logging，可以作为“日志接口”来使用。而真正的“日志实现”可以使用Log4j。

Log4j是一种非常流行的日志框架，最新版本是2.x。

## 使用SLF4J和Logback

前面介绍了Commons Logging和Log4j这一对好基友，它们一个负责充当日志API，一个负责实现日志底层，搭配使用非常便于开发。

有的童鞋可能还听说过SLF4J和Logback。这两个东东看上去也像日志，它们又是啥？

其实SLF4J类似于Commons Logging，也是一个日志接口，而Logback类似于Log4j，是一个日志的实现。

为什么有了Commons  Logging和Log4j，又会蹦出来SLF4J和Logback？这是因为Java有着非常悠久的开源历史，不但OpenJDK本身是开源的，而且我们用到的第三方库，几乎全部都是开源的。开源生态丰富的一个特定就是，同一个功能，可以找到若干种互相竞争的开源库。

# 反射

什么是反射？

能够分析类能力的程序称为反射。反射机制可以用来：

- 在运行是分析类的能力
- 在运行时查看对象
- 实现通用的数组操作代码
- 利用Method对象方法

反射就是Reflection，Java的反射是指程序在运行期可以拿到一个对象的所有信息。

正常情况下，如果我们要调用一个对象的方法，或者访问一个对象的字段，通常会传入对象实例：

```
// Main.java
import com.itranswarp.learnjava.Person;

public class Main {
    String getFullName(Person p) {
        return p.getFirstName() + " " + p.getLastName();
    }
}
```

但是，如果不能获得`Person`类，只有一个`Object`实例，比如这样：

```
String getFullName(Object obj) {
    return ???
}
```

怎么办？有童鞋会说：强制转型啊！

```
String getFullName(Object obj) {
    Person p = (Person) obj;
    return p.getFirstName() + " " + p.getLastName();
}
```

强制转型的时候，你会发现一个问题：编译上面的代码，仍然需要引用`Person`类。不然，去掉`import`语句，你看能不能编译通过？

**所以，反射是为了解决在运行期，对某个实例一无所知的情况下，如何调用其方法。**

Java反射机制主要提供了以下几种功能：

- 在运行时判断一个对象所属的类
- 在运行时构造一个类的对象
- 在运行时判断一个类所有成员变量和方法
- 在运行时调用对象的方法

## Class类

在java程序运行时，每一个对象都有一个类型标识，这个信息跟踪者这个对象所属的类，可以通过专门的java类访问这些信息。保存这些信息的类被称为Class。

- 一个Class对象将表示一个特定类的属性

~~~java
Employee e;
Class c1= e.getClass();

~~~



![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200928101733.png)









---



除了`int`等基本类型外，Java的其他类型全部都是`class`（包括`interface`）。例如：

- `String`
- `Object`
- `Runnable`
- `Exception`
- ...

仔细思考，我们可以得出结论：`class`（包括`interface`）的本质是数据类型（`Type`）。无继承关系的数据类型无法赋值：

~~~java
Number n = new Double(123.456); // OK
String s = new Double(123.456); // compile error!
~~~

而`class`是由JVM在执行过程中动态加载的。JVM在第一次读取到一种`class`类型时，将其加载进内存。

每加载一种`class`，JVM就为其创建一个`Class`类型的实例，并关联起来。注意：这里的`Class`类型是一个名叫`Class`的`class`。它长这样：

## 访问字段

...

## 调用方法

...

## 调用构造方法

...

## 获取继承关系

...

## 动态代理

...

# 注解

## 使用注解

注解是放在Java源码的类、方法、字段、参数前的一种特殊“注释”:

~~~java
// this is a component:
@Resource("hello")
public class Hello {
    @Inject
    int n;

    @PostConstruct
    public void hello(@Param String name) {
        System.out.println(name);
    }

    @Override
    public String toString() {
        return "Hello";
    }
}
~~~

注释会被编译器直接忽略，注解则可以被编译器打包进入class文件，因此，注解是一种用作标注的“元数据”。

### 注解的作用

从JVM的角度看，注解本身对代码逻辑没有任何影响，如何使用注解完全由工具决定。

Java的注解可以分为三类：

第一类是由编译器使用的注解，例如：

- `@Override`：让编译器检查该方法是否正确地实现了覆写；
- `@SuppressWarnings`：告诉编译器忽略此处代码产生的警告。

这类注解不会被编译进入`.class`文件，它们在编译后就被编译器扔掉了。

第二类是由工具处理`.class`文件使用的注解，比如有些工具会在加载class的时候，对class做动态修改，实现一些特殊的功能。这类注解会被编译进入`.class`文件，但加载结束后并不会存在于内存中。这类注解只被一些底层库使用，一般我们不必自己处理。

第三类是在程序运行期能够读取的注解，它们在加载后一直存在于JVM中，这也是最常用的注解。例如，一个配置了`@PostConstruct`的方法会在调用构造方法后自动被调用（这是Java代码读取该注解实现的功能，JVM并不会识别该注解）

定义一个注解时，还可以定义配置参数。配置参数可以包括：

- 所有基本类型；
- String；
- 枚举类型；
- 基本类型、String、Class以及枚举的数组。

因为配置参数必须是常量，所以，上述限制保证了注解在定义时就已经确定了每个参数的值。

注解的配置参数可以有默认值，缺少某个配置参数时将使用默认值。

此外，大部分注解会有一个名为`value`的配置参数，对此参数赋值，可以只写常量，相当于省略了value参数。

如果只写注解，相当于全部使用默认值。

```java
public class Hello {
    @Check(min=0, max=100, value=55)
    public int n;

    @Check(value=99)
    public int p;

    @Check(99) // @Check(value=99)
    public int x;

    @Check
    public int y;
}
```

`@Check`就是一个注解。第一个`@Check(min=0, max=100, value=55)`明确定义了三个参数，第二个`@Check(value=99)`只定义了一个`value`参数，它实际上和`@Check(99)`是完全一样的。最后一个`@Check`表示所有参数都使用默认值。

## 定义注解

Java语言使用`@interface`语法来定义注解（`Annotation`），它的格式如下：

```
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```

### 元注解

<u>有一些注解可以修饰其他注解，这些注解就称为元注解</u>（meta annotation）。Java标准库已经定义了一些元注解，我们只需要使用元注解，通常不需要自己去编写元注解。

@Target

最常用的元注解是`@Target`。使用`@Target`可以定义`Annotation`能够被应用于源码的哪些位置：

- 类或接口：`ElementType.TYPE`；
- 字段：`ElementType.FIELD`；
- 方法：`ElementType.METHOD`；
- 构造方法：`ElementType.CONSTRUCTOR`；
- 方法参数：`ElementType.PARAMETER`。

例如，定义注解`@Report`可用在方法上，我们必须添加一个`@Target(ElementType.METHOD)`：

```java
@Target(ElementType.METHOD)
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```

定义注解`@Report`可用在方法或字段上，可以把`@Target`注解参数变为数组`{ ElementType.METHOD, ElementType.FIELD }`：

```java
@Target({
    ElementType.METHOD,
    ElementType.FIELD
})
public @interface Report {
    ...
}
```

实际上`@Target`定义的`value`是`ElementType[]`数组，只有一个元素时，可以省略数组的写法。

#### @Retention

另一个重要的元注解`@Retention`定义了`Annotation`的生命周期：

- 仅编译期：`RetentionPolicy.SOURCE`；
- 仅class文件：`RetentionPolicy.CLASS`；
- 运行期：`RetentionPolicy.RUNTIME`。

如果`@Retention`不存在，则该`Annotation`默认为`CLASS`。因为通常我们自定义的`Annotation`都是`RUNTIME`，所以，务必要加上`@Retention(RetentionPolicy.RUNTIME)`这个元注解：

```
@Retention(RetentionPolicy.RUNTIME)
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```

#### @Repeatable

使用`@Repeatable`这个元注解可以定义`Annotation`是否可重复。这个注解应用不是特别广泛。

```
@Repeatable(Reports.class)
@Target(ElementType.TYPE)
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}

@Target(ElementType.TYPE)
public @interface Reports {
    Report[] value();
}
```

经过`@Repeatable`修饰后，在某个类型声明处，就可以添加多个`@Report`注解：

```
@Report(type=1, level="debug")
@Report(type=2, level="warning")
public class Hello {
}
```

#### @Inherited

使用`@Inherited`定义子类是否可继承父类定义的`Annotation`。`@Inherited`仅针对`@Target(ElementType.TYPE)`类型的`annotation`有效，并且仅针对`class`的继承，对`interface`的继承无效：

```
@Inherited
@Target(ElementType.TYPE)
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```

在使用的时候，如果一个类用到了`@Report`：

```
@Report(type=1)
public class Person {
}
```

则它的子类默认也定义了该注解：

```
public class Student extends Person {
}
```

#### 如何定义Annotation

我们总结一下定义`Annotation`的步骤：

第一步，用`@interface`定义注解：

```
public @interface Report {
}
```

第二步，添加参数、默认值：

```
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```

把最常用的参数定义为`value()`，推荐所有参数都尽量设置默认值。

第三步，用元注解配置注解：

```
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```

其中，必须设置`@Target`和`@Retention`，`@Retention`一般设置为`RUNTIME`，因为我们自定义的注解通常要求在运行期读取。一般情况下，不必写`@Inherited`和`@Repeatable`。

#### 小结

Java使用`@interface`定义注解：

可定义多个参数和默认值，核心参数使用`value`名称；

必须设置`@Target`来指定`Annotation`可以应用的范围；

应当设置`@Retention(RetentionPolicy.RUNTIME)`便于运行期读取该`Annotation`。



## 处理注解

Java的注解本身对代码逻辑没有任何影响。根据`@Retention`的配置：

- `SOURCE`类型的注解在编译期就被丢掉了；
- `CLASS`类型的注解仅保存在class文件中，它们不会被加载进JVM；
- `RUNTIME`类型的注解会被加载进JVM，并且在运行期可以被程序读取。

如何使用注解完全由工具决定。`SOURCE`类型的注解主要由编译器使用，因此我们一般只使用，不编写。`CLASS`类型的注解主要由底层工具库使用，涉及到class的加载，一般我们很少用到。**只有`RUNTIME`类型的注解不但要使用，还经常需要编写。**

因此，我们只讨论如何读取`RUNTIME`类型的注解。



因为注解定义后也是一种`class`，所有的注解都继承自`java.lang.annotation.Annotation`，因此，**读取注解，需要使用反射API**。

Java提供的使用反射API读取`Annotation`的方法包括：

判断某个注解是否存在于`Class`、`Field`、`Method`或`Constructor`：

- `Class.isAnnotationPresent(Class)`
- `Field.isAnnotationPresent(Class)`
- `Method.isAnnotationPresent(Class)`
- `Constructor.isAnnotationPresent(Class)`

例如：

```
// 判断@Report是否存在于Person类:
Person.class.isAnnotationPresent(Report.class);
```

使用反射API读取Annotation：

- `Class.getAnnotation(Class)`
- `Field.getAnnotation(Class)`
- `Method.getAnnotation(Class)`
- `Constructor.getAnnotation(Class)`

例如：

```java
// 获取Person定义的@Report注解:
Report report = Person.class.getAnnotation(Report.class);
int type = report.type();
String level = report.level();
```

第二种方法是直接读取`Annotation`，如果`Annotation`不存在，将返回`null`：

```java
Class cls = Person.class;
Report report = cls.getAnnotation(Report.class);
if (report != null) {
   ...
}
```

读取方法、字段和构造方法的`Annotation`和Class类似。但要读取方法参数的`Annotation`就比较麻烦一点，因为方法参数本身可以看成一个数组，而每个参数又可以定义多个注解，所以，一次获取方法参数的所有注解就必须用一个二维数组来表示。例如，对于以下方法定义的注解：

```
public void hello(@NotNull @Range(max=5) String name, @NotNull String prefix) {
}
```

要读取方法参数的注解，我们先用反射获取`Method`实例，然后读取方法参数的所有注解：

```java
// 获取Method实例:
Method m = ...
// 获取所有参数的Annotation:
Annotation[][] annos = m.getParameterAnnotations();
// 第一个参数（索引为0）的所有Annotation:
Annotation[] annosOfName = annos[0];
for (Annotation anno : annosOfName) {
    if (anno instanceof Range) { // @Range注解
        Range r = (Range) anno;
    }
    if (anno instanceof NotNull) { // @NotNull注解
        NotNull n = (NotNull) anno;
    }
}
```

### 使用注解

注解如何使用，完全由程序自己决定。例如，JUnit是一个测试框架，它会自动运行所有标记为`@Test`的方法。

我们来看一个`@Range`**注解，我们希望用它来定义一个`String`字段的规则：字段长度满足`@Range`的参数定义**：

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.FIELD)
public @interface Range {
    int min() default 0;
    int max() default 255;
}
```

在某个JavaBean中，我们可以使用该注解：

```java
public class Person {
    @Range(min=1, max=20)
    public String name;

    @Range(max=10)
    public String city;
}
```

但是，定义了注解，本身对程序逻辑没有任何影响。我们必须自己编写代码来使用注解。这里，我们编写一个`Person`实例的检查方法，它可以检查`Person`实例的`String`字段长度是否满足`@Range`的定义：

```java
void check(Person person) throws IllegalArgumentException, ReflectiveOperationException {
    // 遍历所有Field:
    for (Field field : person.getClass().getFields()) {
        // 获取Field定义的@Range:
        Range range = field.getAnnotation(Range.class);
        // 如果@Range存在:
        if (range != null) {
            // 获取Field的值:
            Object value = field.get(person);
            // 如果值是String:
            if (value instanceof String) {
                String s = (String) value;
                // 判断值是否满足@Range的min/max:
                if (s.length() < range.min() || s.length() > range.max()) {
                    throw new IllegalArgumentException("Invalid field: " + field.getName());
                }
            }
        }
    }
}
```

这样一来，我们通过`@Range`注解，配合`check()`方法，就可以完成`Person`实例的检查。注意检查逻辑完全是我们自己编写的，JVM不会自动给注解添加任何额外的逻辑。

# 泛型

泛型是一种“代码模板”，可以用一套代码套用各种类型。

## 什么是泛型

在讲解什么是泛型之前，我们先观察Java标准库提供的`ArrayList`，它可以看作“可变长度”的数组，因为用起来比数组更方便。

实际上`ArrayList`内部就是一个`Object[]`数组，配合存储一个当前分配的长度，就可以充当“可变数组”：

~~~java
public class ArrayList {
    private Object[] array;
    private int size;
    public void add(Object e) {...}
    public void remove(int index) {...}
    public Object get(int index) {...}
}
~~~

如果用上述`ArrayList`存储`String`类型，会有这么几个缺点：

- 需要强制转型；
- 不方便，易出错。

例如，代码必须这么写：

```java
ArrayList list = new ArrayList();
list.add("Hello");
// 获取到Object，必须强制转型为String:
String first = (String) list.get(0);
```

```java
list.add(new Integer(123));
// ERROR: ClassCastException:
String second = (String) list.get(1);
```

要解决上述问题，我们可以为`String`单独编写一种`ArrayList`：

~~~java
public class StringArrayList{
    private String[] array;
    public int size;
    public void add(String e){}
    public void remove(int index) {...}
    public String get(int index) {...}
}
~~~

这样一来，存入的必须是`String`，取出的也一定是`String`，不需要强制转型，因为编译器会强制检查放入的类型：

```
StringArrayList list = new StringArrayList();
list.add("Hello");
String first = list.get(0);
// 编译错误: 不允许放入非String类型:
list.add(new Integer(123));
```

问题暂时解决。

然而，新的问题是，如果要存储`Integer`，还需要为`Integer`单独编写一种`ArrayList`：

```
public class IntegerArrayList {
    private Integer[] array;
    private int size;
    public void add(Integer e) {...}
    public void remove(int index) {...}
    public Integer get(int index) {...}
}
```

实际上，还需要为其他所有class单独编写一种`ArrayList`：

- LongArrayList
- DoubleArrayList
- PersonArrayList
- ...

这是不可能的，JDK的class就有上千个，而且它还不知道其他人编写的class。

---

为了解决新的问题，我们必须把`ArrayList`变成一种**模板**：`ArrayList<T>`，代码如下：

~~~java
class ArrayList<T>{
    private T[] array;
    private void add(T e){
        ...
    }
    public void remove(int index){
        ...
    }
    public  T get(int index){
        ...
    }
}
~~~

**`T`可以是任何class。这样一来，我们就实现了：编写一次模版，可以创建任意类型的`ArrayList`**：

~~~java
// 创建可以存储String的ArrayList:
ArrayList<String> strList = new ArrayList<String>();
// 创建可以存储Float的ArrayList:
ArrayList<Float> floatList = new ArrayList<Float>();
// 创建可以存储Person的ArrayList:
ArrayList<Person> personList = new ArrayList<Person>();
~~~

因此，**泛型就是定义一种模板**，例如`ArrayList<T>`，然后在代码中为用到的类创建对应的`ArrayList<类型>`：

~~~java
ArrayList<String> strList = new ArrayList<String>();

~~~

```java
strList.add("hello"); // OK
String s = strList.get(0); // OK
strList.add(new Integer(123)); // compile error!
Integer n = strList.get(0); // compile error!
```

这样一来，既实现了编写一次，万能匹配，又通过编译器保证了类型安全：这就是泛型。

#### 向上转型

在Java标准库中的`ArrayList<T>`实现了`List<T>`接口，它可以向上转型为`List<T>`：

```java
public class ArrayList<T> implements List<T> {
    ...
}

List<String> list = new ArrayList<String>();
```

即类型`ArrayList<T>`可以向上转型为`List<T>`。

要*特别注意*：不能把`ArrayList<Integer>`向上转型为`ArrayList<Number>`或`List<Number>`。

这是为什么呢？假设`ArrayList<Integer>`可以向上转型为`ArrayList<Number>`，观察一下代码：

```java
// 创建ArrayList<Integer>类型：
ArrayList<Integer> integerList = new ArrayList<Integer>();
// 添加一个Integer：
integerList.add(new Integer(123));
// “向上转型”为ArrayList<Number>：
ArrayList<Number> numberList = integerList;
// 添加一个Float，因为Float也是Number：
numberList.add(new Float(12.34));
// 从ArrayList<Integer>获取索引为1的元素（即添加的Float）：
Integer n = integerList.get(1); // ClassCastException!
```

我们把一个`ArrayList<Integer>`转型为`ArrayList<Number>`类型后，这个`ArrayList<Number>`就可以接受`Float`类型，因为`Float`是`Number`的子类。但是，`ArrayList<Number>`实际上和`ArrayList<Integer>`是同一个对象，也就是`ArrayList<Integer>`类型，它不可能接受`Float`类型， 所以在获取`Integer`的时候将产生`ClassCastException`。

实际上，编译器为了避免这种错误，根本就不允许把`ArrayList<Integer>`转型为`ArrayList<Number>`。

~~~java
package demo;

import java.io.UnsupportedEncodingException;
import java.util.ArrayList;
import java.util.logging.Logger;

/*
* @author shilongshen
*
*@
*
*
* */

public class Main{
    public static void main(String[] args){
        ArrayList<String> strlist=new ArrayList<String>();
        ArrayList<Float> floatlist =new ArrayList<Float>();

        strlist.add("hello");
        String s =strlist.get(0);
        System.out.println(s);

        //报错
        //strlist.add(new Integer(123));
        
    }
}

~~~



## 使用泛型

使用`ArrayList`时，如果不定义泛型类型时，泛型类型实际上就是`Object`：

```java
// 编译器警告:
List list = new ArrayList();
list.add("Hello");
list.add("World");
String first = (String) list.get(0);
String second = (String) list.get(1);
```

此时，只能把`<T>`当作`Object`使用，没有发挥泛型的优势。

当我们定义泛型类型`<String>`后，`List<T>`的**泛型接口**变为强类型`List<String>`：

T ->String

```java
// 无编译器警告:
List<String> list = new ArrayList<String>();
list.add("Hello");
list.add("World");
// 无强制转型:
String first = list.get(0);
String second = list.get(1);
```

当我们定义泛型类型`<Number>`后，`List<T>`的泛型接口变为强类型`List<Number>`：

```java
List<Number> list = new ArrayList<Number>();
list.add(new Integer(123));
list.add(new Double(12.34));
Number first = list.get(0);
Number second = list.get(1);
```

编译器如果能自动推断出泛型类型，就可以省略后面的泛型类型。例如，对于下面的代码：

```java
List<Number> list = new ArrayList<Number>();
```

编译器看到泛型类型`List<Number>`就可以自动推断出后面的`ArrayList<T>`的泛型类型必须是`ArrayList<Number>`，因此，可以把代码简写为：

```java
// 可以省略后面的Number，编译器可以自动推断泛型类型：
List<Number> list = new ArrayList<>();
```

### 泛型接口

除了`ArrayList<T>`使用了泛型，还可以在接口中使用泛型。例如，`Arrays.sort(Object[])`可以对任意数组进行排序，但待排序的元素必须实现`Comparable<T>`这个泛型接口：

~~~java
public interface Comparable<T> {
    /**
     * 返回负数: 当前实例比参数o小
     * 返回0: 当前实例与参数o相等
     * 返回正数: 当前实例比参数o大
     */
    int compareTo(T o);
}

~~~

可以直接对`String`数组进行排序：

~~~java
package demo;

import java.io.UnsupportedEncodingException;
import java.lang.reflect.Array;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.logging.Logger;


/*
* @author shilongshen
*
*@
*
*
* */

public class Main{
    public static void main(String[] args){
       String[] ss=new String[] {
               "orange","Apple","pear"
       };

        Arrays.sort(ss);

        System.out.print(Arrays.toString(ss));

    }
}

~~~

这是因为**`String`本身已经实现了`Comparable<String>`接口**。如果换成我们自定义的`Person`类型试试：

~~~java
package demo;

import java.io.UnsupportedEncodingException;
import java.lang.reflect.Array;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.logging.Logger;


/*
* @author shilongshen
*
*@
*
*
* */

public class Main{
    public static void main(String[] args){
       Person[] ps =new Person[] {
               new Person("Bob",61),
               new Person("xiaohong",99),
               new Person("xiaoming",88)
       };

       Arrays.sort(ps);

       System.out.print(Arrays.toString(ps));

    }
}

class Person{
    String name;
    int score;
    Person(String name,int score){
        this.name=name;
        this.score=score;
    }

    public String toString(){
        return this.name+","+this.score;
    }
}

~~~

运行程序，我们会得到`ClassCastException`，即**无法将`Person`转型为`Comparable`**。我们修改代码，让`Person`实现`Comparable<T>`接口：

~~~java
package demo;

import java.io.UnsupportedEncodingException;
import java.lang.reflect.Array;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.logging.Logger;


/*
* @author shilongshen
*
*@
*
*
* */

public class Main{
    public static void main(String[] args){
       Person[] ps =new Person[] {
               new Person("Bob",61),
               new Person("xiaohong",99),
               new Person("xiaoming",88)
       };

       Arrays.sort(ps);

       System.out.print(Arrays.toString(ps));

    }
}

class Person implements Comparable<Person>{
    String name;
    int score;
    Person(String name,int score){
        this.name=name;
        this.score=score;
    }

    public int compareTo(Person other){
        return this.name.compareTo(other.name);
    }

    public String toString(){
        return this.name+","+this.score;
    }
}

~~~

### 小结

使用泛型时，把泛型参数`<T>`替换为需要的class类型，例如：`ArrayList<String>`，`ArrayList<Number>`等；

可以省略编译器能自动推断出的类型，例如：`List<String> list = new ArrayList<>();`；

不指定泛型参数类型时，编译器会给出警告，且只能将`<T>`视为`Object`类型；

可以在接口中定义泛型类型，实现此接口的类必须实现正确的泛型类型。

## 编写泛型

编写泛型类比普通类要复杂。通常来说，泛型类一般用在集合类中，例如`ArrayList<T>`，**我们很少需要编写泛型类。**

如果我们确实需要编写一个泛型类，那么，应该如何编写它？

可以按照以下步骤来编写一个泛型类。

首先，按照某种类型，例如：`String`，来编写类：

~~~java
public class Pair {
    private String first;
    private String last;
    public Pair(String first, String last) {
        this.first = first;
        this.last = last;
    }
    public String getFirst() {
        return first;
    }
    public String getLast() {
        return last;
    }
}
~~~

然后，标记所有的特定类型，这里是`String`：

```java
public class Pair {
    private String first;
    private String last;
    public Pair(String first, String last) {
        this.first = first;
        this.last = last;
    }
    public String getFirst() {
        return first;
    }
    public String getLast() {
        return last;
    }
}
```

最后，把特定类型`String`替换为`T`，并申明`<T>`：

~~~java
class pair<T>{
    private T first;
    private T last;
    public pair(T first,T last){
        this.first=first;
        this.last=last;
    }


    public T getFirst() {
        return first;
    }

    public void setFirst(T first) {
        this.first = first;
    }

    public T getLast() {
        return last;
    }

    public void setLast(T last) {
        this.last = last;
    }
}

~~~

熟练后即可直接从`T`开始编写。

### 静态方法

编写泛型类时，要特别注意，泛型类型`<T>`不能用于静态方法。例如：

```java
public class Pair<T> {
    private T first;
    private T last;
    public Pair(T first, T last) {
        this.first = first;
        this.last = last;
    }
    public T getFirst() { ... }
    public T getLast() { ... }

    // 对静态方法使用<T>:
    public static Pair<T> create(T first, T last) {
        return new Pair<T>(first, last);
    }
}
```

上述代码会导致编译错误，我们无法在静态方法`create()`的方法参数和返回类型上使用泛型类型`T`。

有些同学在网上搜索发现，可以在`static`修饰符后面加一个`<T>`，编译就能通过：

```java
public class Pair<T> {
    private T first;
    private T last;
    public Pair(T first, T last) {
        this.first = first;
        this.last = last;
    }
    public T getFirst() { ... }
    public T getLast() { ... }

    // 可以编译通过:
    public static <T> Pair<T> create(T first, T last) {
        return new Pair<T>(first, last);
    }
}
```

但实际上，这个`<T>`和`Pair<T>`类型的`<T>`已经没有任何关系了。

---

对于静态方法，我们可以单独改写为“泛型”方法，只需要使用另一个类型即可。对于上面的`create()`静态方法，我们应该把它**改为另一种泛型类型**，例如，`<K>`：

~~~java
public class Pair<T> {
    private T first;
    private T last;
    public Pair(T first, T last) {
        this.first = first;
        this.last = last;
    }
    public T getFirst() { ... }
    public T getLast() { ... }

    // 静态泛型方法应该使用其他类型区分:
    public static <K> Pair<K> create(K first, K last) {
        return new Pair<K>(first, last);
    }
}
~~~

这样才能清楚地将静态方法的泛型类型和实例类型的泛型类型区分开。

### 多个泛型类型

~~~java
public class pair<T,K>{
    private T first;
    private K last;
    public pair(T first,K last){
        this.first=first;
        this.last=last;
    }
    
    public T getFirst(){...}
    public K getLast(){...}
}
~~~

使用的时候，需要指出两种类型：

```
Pair<String, Integer> p = new Pair<>("test", 123);
```

Java标准库的`Map<K, V>`就是使用两种泛型类型的例子。它对Key使用一种类型，对Value使用另一种类型。

### 小结

编写泛型时，需要定义泛型类型`<T>`；

静态方法不能引用泛型类型`<T>`，必须定义其他类型（例如`<K>`）来实现静态泛型方法；

泛型可以同时定义多种类型，例如`Map<K, V>`。

## 擦拭法

泛型是一种类似”模板代码“的技术，不同语言的泛型实现方式不一定相同。

Java语言的泛型实现方式是擦拭法（Type Erasure）。

所谓擦拭法是指，**虚拟机对泛型其实一无所知，所有的工作都是编译器做的**。

例如，我们编写了一个泛型类`Pair<T>`，这是编译器看到的代码：

```
public class Pair<T> {
    private T first;
    private T last;
    public Pair(T first, T last) {
        this.first = first;
        this.last = last;
    }
    public T getFirst() {
        return first;
    }
    public T getLast() {
        return last;
    }
}
```

而虚拟机根本不知道泛型。这是虚拟机执行的代码：

```
public class Pair {
    private Object first;
    private Object last;
    public Pair(Object first, Object last) {
        this.first = first;
        this.last = last;
    }
    public Object getFirst() {
        return first;
    }
    public Object getLast() {
        return last;
    }
}
```

因此，Java使用擦拭法实现泛型，导致了：

- 编译器把类型`<T>`视为`Object`；
- 编译器根据`<T>`实现安全的强制转型。

使用泛型的时候，我们编写的代码也是编译器看到的代码：

```
Pair<String> p = new Pair<>("Hello", "world");
String first = p.getFirst();
String last = p.getLast();
```

而虚拟机执行的代码并没有泛型：

```
Pair p = new Pair("Hello", "world");
String first = (String) p.getFirst();
String last = (String) p.getLast();
```

所以，**Java的泛型是由编译器在编译时实行的，编译器内部永远把所有类型`T`视为`Object`处理，但是，在需要转型的时候，编译器会根据`T`的类型自动为我们实行安全地强制转型。**

---

了解了Java泛型的实现方式——擦拭法，我们就知道了Java泛型的局限：

局限一：`<T>`不能是基本类型，例如`int`，因为实际类型是`Object`，**`Object`类型无法持有基本类型：**

```java
Pair<int> p = new Pair<>(1, 2); // compile error!
```

局限二：无法取得带泛型的`Class`。观察以下代码：

~~~java
package demo;

import java.io.UnsupportedEncodingException;
import java.lang.reflect.Array;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.logging.Logger;


/*
* @author shilongshen
*
*@
*
*
* */

public class Main{
    public static void main(String[] args){
       pair<String> p1=new pair<>("hello","world");
       pair<Integer> p2 =new pair<>(123,456);

       Class c1=p1.getClass();
       Class c2=p2.getClass();

       System.out.println(c1==c2);//True
       System.out.println(c1==pair.class);//True

    }
}



class pair<T>{
    private T first;
    private T last;
    public pair(T first,T last){
        this.first=first;
        this.last=last;
    }


    public T getFirst() {
        return first;
    }

    public void setFirst(T first) {
        this.first = first;
    }

    public T getLast() {
        return last;
    }

    public void setLast(T last) {
        this.last = last;
    }
}



~~~

因为`T`是`Object`，我们对`Pair<String>`和`Pair<Integer>`类型获取`Class`时，获取到的是同一个`Class`，也就是`Pair`类的`Class`。

换句话说，所有泛型实例，**无论`T`的类型是什么，`getClass()`返回同一个`Class`实例**，因为编译后它们全部都是`Pair<Object>`。

局限三：无法判断带泛型的类型：

```
Pair<Integer> p = new Pair<>(123, 456);
// Compile error:
if (p instanceof Pair<String>) {
}
```

原因和前面一样，并不存在`Pair<String>.class`，而是**只有唯一的`Pair.class`。**

```java
public class Pair<T> {
    private T first;
    private T last;
    public Pair() {
        // Compile error:
        first = new T();
        last = new T();
    }
}
```

上述代码无法通过编译，因为构造方法的两行语句：

```java
first = new T();
last = new T();
```

擦拭后实际上变成了：

```java
first = new Object();
last = new Object();
```

这样一来，创建`new Pair<String>()`和创建`new Pair<Integer>()`就全部成了`Object`，显然编译器要阻止这种类型不对的代码。

要实例化`T`类型，我们必须借助额外的`Class<T>`参数：

```java
public class Pair<T> {
    private T first;
    private T last;
    public Pair(Class<T> clazz) {
        first = clazz.newInstance();
        last = clazz.newInstance();
    }
}
```

上述代码借助`Class<T>`参数并通过反射来实例化`T`类型，使用的时候，也必须传入`Class<T>`。例如：

```
Pair<String> pair = new Pair<>(String.class);
```

因为传入了`Class<String>`的实例，所以我们借助`String.class`就可以实例化`String`类型。

### 不恰当的覆写方法

有些时候，一个看似正确定义的方法会无法通过编译。例如：

```java
public class Pair<T> {
    public boolean equals(T t) {
        return this == t;
    }
}
```

这是因为，定义的`equals(T t)`方法实际上会被擦拭成`equals(Object t)`，而这个方法是继承自`Object`的，编译器会阻止一个实际上会变成覆写的泛型方法定义。

换个方法名，避开与`Object.equals(Object)`的冲突就可以成功编译：

```java
public class Pair<T> {
    public boolean same(T t) {
        return this == t;
    }
}
```

### 泛型继承

一个类可以继承自一个泛型类。例如：父类的类型是`Pair<Integer>`，子类的类型是`IntPair`，可以这么继承：

```
public class IntPair extends Pair<Integer> {
}
```

使用的时候，因为子类`IntPair`并没有泛型类型，所以，正常使用即可：

```
IntPair ip = new IntPair(1, 2);
```

前面讲了，我们无法获取`Pair<T>`的`T`类型，即给定一个变量`Pair<Integer> p`，无法从`p`中获取到`Integer`类型。

但是，在父类是泛型类型的情况下，编译器就必须把类型`T`（对`IntPair`来说，也就是`Integer`类型）保存到子类的class文件中，不然编译器就不知道`IntPair`只能存取`Integer`这种类型。

在继承了泛型类型的情况下，子类可以获取父类的泛型类型。例如：`IntPair`可以获取到父类的泛型类型`Integer`。获取父类的泛型类型代码比较复杂：



### 小结

Java的泛型是采用擦拭法实现的；

擦拭法决定了泛型`<T>`：

- 不能是基本类型，例如：`int`；
- 不能获取带泛型类型的`Class`，例如：`Pair<String>.class`；
- 不能判断带泛型类型的类型，例如：`x instanceof Pair<String>`；
- 不能实例化`T`类型，例如：`new T()`。

泛型方法要防止重复定义方法，例如：`public boolean equals(T obj)`；

子类可以获取父类的泛型类型`<T>`。

## extends通配符

...

## super通配符

...

## 泛型和反射

...

# 集合

- 集合是一个**容器**，可以一次容纳多个对象。
- 例：假设数据库中有10条记录，那么假设把这10条条记录查询出来，在java程序中会将10条数据封装成10个java对象，然后将10个java对象放在某一个集合中，将集合传到前端，然后遍历集合。
- 集合中不能直接存储基本数据类型，集合也不能直接存储java对象，集合中存储的是java对象的**内存地址**（或者说是引用 ）list.add(100)//自动装箱Integer
  - 集合也是一个对象
  - 集合在任何时候存储的都是内存地址
- 每一个不同的集合，底层会对应不同的数据结构。往不同的集合中存储元素等于将数据放到不同的数据结构中。
  - 常见的数据结构：数组、链表、哈希表
  - 使用不同的集合等同于使用不同的数据结构

~~~java
new ArrayList(); //创建一个集合，底层是数组
new LinkList();//创建一个集合，底层是链表
new Treeset(); //创建一个集合，底层是二叉树
~~~



在Java中，如果<u>一个Java对象可以在内部持有若干其他Java对象</u>，并对外提供访问接口，我们把这种Java对象称为集合。很显然，Java的数组可以看作是一种集合：

```java
String[] ss =new String[10];
ss[0]="Hello";
String first=ss[0];
```

既然Java提供了数组这种数据类型，可以充当集合，那么，我们为什么还需要其他集合类？这是因为数组有如下限制：

- 数组初始化后大小不可变；
- 数组只能按索引顺序存取。

因此，我们需要各种不同类型的集合类来处理不同的数据，例如：

- 可变大小的顺序链表；
- 保证无重复元素的集合；
- ...

Collection

**Java标准库自带的`java.util`包提供了集合类：`Collection`，它是除`Map`外所有其他集合类的根接口**。Java的`java.util`包主要提供了以下三种类型的集合：

- `List`：一种有序列表的集合，例如，按索引排列的`Student`的`List`；
- `Set`：一种保证没有重复元素的集合，例如，所有无重复名称的`Student`的`Set`；
- `Map`：一种通过键值（key-value）查找的映射表集合，例如，根据`Student`的`name`查找对应`Student`的`Map`。

**集合的两个基本接口：Collection和Map**

java.util.Collection : 单个方式存储:插入元素：add(element)

java.util.Map ：键值方式存储，put \ get

Java集合的设计有几个特点：一是实现了接口（相当于模板类）和实现类相分离，例如，有序表的接口是`List`，具体的实现类有`ArrayList`，`LinkedList`等，二是**支持泛型，我们可以限制在一个集合中只能放入同一种数据类型的元素**，例如：

```
List<String> list = new ArrayList<>(); // 只能放入String类型
```

最后，**Java访问集合总是通过统一的方式——迭代器（Iterator）来实现**，它最明显的好处在于无需知道集合内部元素是按什么方式存储的。

由于Java的集合设计非常久远，中间经历过大规模改进，我们要注意到有一小部分集合类是遗留类，不应该继续使用：

- `Hashtable`：一种线程安全的`Map`实现；
- `Vector`：一种线程安全的`List`实现；
- `Stack`：基于`Vector`实现的`LIFO`的栈。

还有一小部分接口是遗留接口，也不应该继续使用：

- `Enumeration<E>`：已被`Iterator<E>`取代。

小结

Java的集合类定义在`java.util`包中，支持泛型，主要提供了3种集合类，包括`List`，`Set`和`Map`。Java集合使用统一的`Iterator`遍历，尽量不要使用遗留接口。

## 使用list

在集合类中，`List`是最基础的一种集合：它是一种有序列表。

可以采用两种方式访问元素：

- 使用迭代器访问 - 随机访问，按任意顺序访问元素
- 使用整数索引访问 - 顺序访问元素

有两种有序集合：

- Arraylist- 可快速随机访问（使用整数索引）
- 链表-访问速度慢，最好使用迭代器来遍历

`List`的行为和数组几乎完全相同：`List`内部按照放入元素的先后顺序存放，每个元素都可以通过索引确定自己的位置，`List`的索引和数组一样，从`0`开始。

数组和`List`类似，也是有序结构，如果我们使用数组，在添加和删除元素的时候，会非常不方便。例如，从一个已有的数组`{'A', 'B', 'C', 'D', 'E'}`中删除索引为`2`的元素：

```ascii
┌───┬───┬───┬───┬───┬───┐
│ A │ B │ C │ D │ E │   │
└───┴───┴───┴───┴───┴───┘
              │   │
          ┌───┘   │
          │   ┌───┘
          │   │
          ▼   ▼
┌───┬───┬───┬───┬───┬───┐
│ A │ B │ D │ E │   │   │
└───┴───┴───┴───┴───┴───┘
```

这个“删除”操作实际上是把`'C'`后面的元素依次往前挪一个位置，而“添加”操作实际上是把指定位置以后的元素都依次向后挪一个位置，腾出来的位置给新加的元素。这两种操作，用数组实现非常麻烦。

因此，**在实际应用中，需要增删元素的有序列表，我们使用最多的是`ArrayList`**。实际上，`ArrayList`在内部使用了数组来存储所有元素。例如，一个`ArrayList`拥有5个元素，实际数组大小为`6`（即有一个空位）：

- ArrayList的扩容机制 ：[参考](https://www.cnblogs.com/baichunyu/p/12965241.html)

```ascii
size=5
┌───┬───┬───┬───┬───┬───┐
│ A │ B │ C │ D │ E │   │
└───┴───┴───┴───┴───┴───┘
```

当添加一个元素并指定索引到`ArrayList`时，`ArrayList`自动移动需要移动的元素：

```ascii
size=5
┌───┬───┬───┬───┬───┬───┐
│ A │ B │   │ C │ D │ E │
└───┴───┴───┴───┴───┴───┘
```

然后，往内部指定索引的数组位置添加一个元素，然后把`size`加`1`：

```ascii
size=6
┌───┬───┬───┬───┬───┬───┐
│ A │ B │ F │ C │ D │ E │
└───┴───┴───┴───┴───┴───┘
```

继续添加元素，但是数组已满，没有空闲位置的时候，`ArrayList`先**创建一个更大的新数组**（进行扩容），然后把旧数组的所有元素复制到新数组，紧接着用新数组取代旧数组：

```ascii
size=6
┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┐
│ A │ B │ F │ C │ D │ E │   │   │   │   │   │   │
└───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘
```

现在，新数组就有了空位，可以继续添加一个元素到数组末尾，同时`size`加`1`：

```ascii
size=7
┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┐
│ A │ B │ F │ C │ D │ E │ G │   │   │   │   │   │
└───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘
```

可见，`**ArrayList`把添加和删除的操作封装起来，让我们操作`List`类似于操作数组，却不用关心内部元素如何移动。**

我们考察`List<E>`接口，可以看到几个主要的接口方法：

- 在末尾添加一个元素：`boolean add(E e)`
- 在指定索引添加一个元素：`boolean add(int index, E e)`
- 删除指定索引的元素：`int remove(int index)`
- 删除某个元素：`int remove(Object e)`
- 获取指定索引的元素：`E get(int index)`
- 获取链表大小（包含元素的个数）：`int size()`

但是，实现`List`接口并非只能通过数组（即`ArrayList`的实现方式）来实现，另一种`LinkedList`通过“链表”也实现了List接口。在`LinkedList`中，它的内部每个元素都指向下一个元素：

```ascii
        ┌───┬───┐   ┌───┬───┐   ┌───┬───┐   ┌───┬───┐
HEAD ──>│ A │ ●─┼──>│ B │ ●─┼──>│ C │ ●─┼──>│ D │   │
        └───┴───┘   └───┴───┘   └───┴───┘   └───┴───┘
```



我们来比较一下`ArrayList`和`LinkedList`：

|                     | ArrayList    | LinkedList           |
| ------------------- | ------------ | -------------------- |
| 获取指定元素        | 速度很快     | 需要从头开始查找元素 |
| 添加元素到末尾      | 速度很快     | 速度很快             |
| 在指定位置添加/删除 | 需要移动元素 | 不需要移动元素       |
| 内存占用            | 少           | 较大                 |

通常情况下，我们总是优先使用`ArrayList`。





### List的特点

- 有序的
- 元素可以重复

使用`List`时，我们要关注`List`接口的规范。`List`接口允许我们添加重复的元素，即**`List`内部的元素可以重复：**

~~~java
public class Main {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("apple"); // size=1
        list.add("pear"); // size=2
        list.add("apple"); // 允许重复添加元素，size=3
        System.out.println(list.size());
    }
}

~~~

`List`还允许添加`null`：

~~~java
public class Main {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("apple"); // size=1
        list.add(null); // size=2
        list.add("pear"); // size=3
        String second = list.get(1); // null
        System.out.println(second);
    }
}
~~~

### 创建List

除了使用`ArrayList`和`LinkedList`，我们还可以通过`List`接口提供的`of()`方法，根据给定元素快速创建`List`：

```
List<Integer> list = List.of(1, 2, 5);
```

但是`List.of()`方法不接受`null`值，如果传入`null`，会抛出`NullPointerException`异常。

### 遍历List

和数组类型，我们要遍历一个`List`，完全可以用`for`循环根据索引配合`get(int)`方法遍历：

~~~java
public class Main{
    public static void main(String[] args){
        List<String> list=List.of("apple","orange");
        for (int i=0;i<list.size();i++){
            String s =list.get(i);
            System.out.println(s);
        }
    }
}
~~~



但这种方式并不推荐，一是代码复杂，二是因为`get(int)`方法只有`ArrayList`的实现是高效的，换成`LinkedList`后，索引越大，访问速度越慢。

所以我们要**始终坚持使用迭代器`Iterator`来访问`List`**。<u>`Iterator`本身也是一个对象，但它是由`List`的实例调用`iterator()`方法的时候创建的</u>。`Iterator`对象知道如何遍历一个`List`，并且不同的`List`类型，返回的`Iterator`对象实现也是不同的，但总是具有最高的访问效率。

**`Iterator`对象有两个方法：`boolean hasNext()`判断是否有下一个元素，`E next()`返回下一个元素**。因此，使用`Iterator`遍历`List`代码如下:

~~~java
public class Main{
    public static void main(String[] args){
        List<String> list=List.of("apple","orange");
        for (Iterator<String> it = list.iterator(); it.hasNext();){
            String s =it.next();
            System.out.println(s);
        }

    }
}
~~~

有童鞋可能觉得使用`Iterator`访问`List`的代码比使用索引更复杂。但是，要记住，通过`Iterator`遍历`List`永远是最高效的方式。并且，由于`Iterator`遍历是如此常用，所以，Java的**`for each`循环本身就可以帮我们使用`Iterator`遍历**。把上面的代码再改写如下：

~~~java
public class Main{
    public static void main(String[] args){
        List<String> list=List.of("apple","orange");
        for (String s:list){
            System.out.println(s);
        }
    }
}
~~~





- 当集合的结构发生改变时，迭代器必须重新获取
- 在迭代集合的元素时，不能够调用`remove` 方法删除集合的元素。

~~~java
package  demo;


import java.util.*;

public class Main{
    public static void main(String[] args) {
        Collection c=new ArrayList();
        c.add(123);
        c.add(456);
        c.add(789);
        Iterator it=c.iterator();
        //这里使用Collection的remove方法删除元素会使得集合的元素发生改变，但是此时的迭代器并没有进行更新，所以会抛出异常ConcurrentModificationException！
        c.remove(123);
        System.out.println(it.next());


    }
}
~~~



~~~java
package  demo;


import java.util.*;

public class Main{
    public static void main(String[] args) {
        Collection c=new ArrayList();
        c.add(123);
        c.add(456);
        c.add(789);
        Iterator it=c.iterator();
        System.out.println(it.next());
        it.remove();//这里调用的是迭代器的remove方法，所以可以正常的删除。
        System.out.println(c.size());
    }
}
~~~

注意：

- 必须先调用next()方法越过将要删除的元素，才能调用迭代器的remove方法，两者存在依赖性。
- 迭代器的remove方法将会删除上次调用next方法返回的元素。





### List和Array转换

把`List`变为`Array`有三种方法，第一种是调用`toArray()`方法直接返回一个`Object[]`数组：

~~~java
public class Main{
    public static void main(String[] args){
        List<String> list=List.of("apple","orange");
       Object[] array=list.toArray();
       for (Object s: array){
           System.out.println(s);
       }
    }
}
~~~



这种方法会丢失类型信息，所以实际应用很少。

第二种方式是**给`toArray(T[])`传入一个类型相同的`Array`，`List`内部自动把元素复制到传入的`Array`中：**

~~~java
public class Main {
    public static void main(String[] args) {
        List<Integer> list = List.of(12, 34, 56);
        Integer[] array = list.toArray(new Integer[3]);
        for (Integer n : array) {
            System.out.println(n);
        }
    }
}
~~~

注意到这个`toArray(T[])`方法的泛型参数`<T>`并不是`List`接口定义的泛型参数`<E>`，所以，我们实际上可以传入其他类型的数组，例如我们传入`Number`类型的数组，返回的仍然是`Number`类型：

~~~java
public class Main {
    public static void main(String[] args) {
        List<Integer> list = List.of(12, 34, 56);
        Number[] array = list.toArray(new Number[3]);
        for (Number n : array) {
            System.out.println(n);
        }
    }
}

~~~

但是，如果我们传入类型不匹配的数组，例如，`String[]`类型的数组，由于`List`的元素是`Integer`，所以无法放入`String`数组，这个方法会抛出`ArrayStoreException`。

如果我们传入的数组大小和`List`实际的元素个数不一致怎么办？根据[List接口](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/util/List.html#toArray(T[]))的文档，我们可以知道：

如果传入的数组不够大，那么`List`内部会创建一个新的刚好够大的数组，填充后返回；如果传入的数组比`List`元素还要多，那么填充完元素后，剩下的数组元素一律填充`null`。

实际上，最常用的是传入一个“恰好”大小的数组：

```
Integer[] array = list.toArray(new Integer[list.size()]);
```

最后一种更简洁的写法是通过`List`接口定义的`T[] toArray(IntFunction<T[]> generator)`方法：

```
Integer[] array = list.toArray(Integer[]::new);
```

这种函数式写法我们会在后续讲到。

---



反过来，把`Array`变为`List`就简单多了，通过`List.of(T...)`方法最简单：

```
Integer[] array = { 1, 2, 3 };
List<Integer> list = List.of(array);
```

对于JDK 11之前的版本，可以使用`Arrays.asList(T...)`方法把数组转换成`List`。

要注意的是，返回的`List`不一定就是`ArrayList`或者`LinkedList`，因为`List`只是一个接口，如果我们调用`List.of()`，它返回的是一个**只读`List`**：

~~~java
public class Main {
    public static void main(String[] args) {
        List<Integer> list = List.of(12, 34, 56);
        list.add(999); // UnsupportedOperationException
    }
}

~~~

对只读`List`调用`add()`、`remove()`方法会抛出`UnsupportedOperationException`。

练习：

给定一组连续的整数，例如：10，11，12，……，20，但其中缺失一个数字，试找出缺失的数字：

~~~java
package demo;

import java.io.UnsupportedEncodingException;
import java.lang.reflect.Array;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Iterator;
import java.util.List;
import java.util.logging.Logger;


/*
 * @author shilongshen
 *
 *@
 *
 *
 * */

public class Main {
    public static void main(String[] args) {
        final int start = 10;
        final int end = 20;
        List<Integer> list = new ArrayList<>();
        for (int i = start; i <= end; i++) {
            list.add(i);
        }

        int removed = list.remove((int) (Math.random() * list.size()));

        int found = findMissingNUMber(start, end, list);

        System.out.println(list.toString());
        System.out.println("missing number:" + removed);
        System.out.println("found number:" + found);
        System.out.println(removed == found ? "测试成功" : "测试失败");
    }

    static int findMissingNUMber(int start, int end, List<Integer> list) {

//        int[] num={10,11,12,13,14,15,16,17,18,19,20};
        int[] num=new int[list.size()];
        for (int i=0;i< list.size();i++){
            num[i]=start;
            start++;
        }

        int j=0;
        int miss=0;
        for (int i=0;i<list.size();i++){
            Integer s=list.get(i);

            if (s==num[j]){
                j++;
            }
            else {

                miss=s-1;
                break;
            }

        }
        return miss;
    }
}

~~~

增强版：和上述题目一样，但整数不再有序，试找出缺失的数字：

~~~java
package demo;

import java.io.UnsupportedEncodingException;
import java.lang.reflect.Array;
import java.util.*;
import java.util.logging.Logger;


/*
 * @author shilongshen
 *
 *@
 *
 *
 * */

public class Main {
    public static void main(String[] args) {
        final int start = 10;
        final int end = 20;
        List<Integer> list = new ArrayList<>();
        for (int i = start; i <= end; i++) {
            list.add(i);
        }
        System.out.printf("before:%s\n",list.toString());
        Collections.shuffle(list);//随机交换List中的元素位置
        int removed = list.remove((int) (Math.random() * list.size()));//随机删除List中的一个元素

        int found = findMissingNUMber(start, end, list);

        System.out.printf("after:%s\n",list.toString());
        System.out.println("missing number:" + removed);
        System.out.println("found number:" + found);
        System.out.println(removed == found ? "测试成功" : "测试失败");
    }

    static int findMissingNUMber(int start, int end, List<Integer> list) {

//        int[] num={10,11,12,13,14,15,16,17,18,19,20};
        int[] num = new int[list.size()+1];
        for (int i = 0; i <= list.size(); i++) {
            num[i] = start;
            start++;
        }
        System.out.printf("size:%s\n",list.size());
        System.out.println(Arrays.toString(num));

        int miss = 0;

        for (int j = 0; j <= list.size(); j++) {
            for (int i = 0; i < list.size(); i++) {
                Integer s = list.get(i);
                if (num[j] == s) {
                    break;
                }

                if (i == (list.size() - 1)) {//如果list的最后一个数都不与num[j]相等，说明list中缺少num[j].
//                    miss=num[j];
                    if (num[j] != s) {
                        miss = num[j];
                    }
//                    else {
//                        break;
//                    }
                }
            }
        }

        return miss;
    }
}
~~~



## 编写equals方法

- 如果在collection中调用了contains和remove方法，存放进集合中的元素需要重写equals方法（String,Integer等对象不用，因为本身已经重写了equals方法）
- 调用的是存放进集合中元素的equals方法。

我们知道`List`是一种有序链表：`List`内部按照放入元素的先后顺序存放，并且每个元素都可以通过索引确定自己的位置。

**`List`还提供了`boolean contains(Object o)`方法来判断`List`是否包含某个指定元素。此外，`int indexOf(Object o)`方法可以返回某个元素的索引，如果元素不存在，就返回`-1`**。

~~~java
public class Main {
    public static void main(String[] args) {
        List<String> list = List.of("A", "B", "C");
        System.out.println(list.contains("C")); // true
        System.out.println(list.contains("X")); // false
        System.out.println(list.indexOf("C")); // 2
        System.out.println(list.indexOf("X")); // -1
    }
}

~~~

这里我们注意一个问题，我们往`List`中添加的`"C"`和调用`contains("C")`传入的`"C"`是不是同一个实例？

如果这两个`"C"`不是同一个实例，这段代码是否还能得到正确的结果？我们可以改写一下代码测试一下：

~~~java
public class Main {
    public static void main(String[] args) {
        List<String> list = List.of("A", "B", "C");
        System.out.println(list.contains(new String("C"))); // true 
        System.out.println(list.indexOf(new String("C"))); // 2 
    }
}

~~~

因为我们传入的是`new String("C")`，所以一定是不同的实例。结果仍然符合预期，这是为什么呢？

因<u>为`List`内部并不是通过`==`判断两个元素是否相等，而是使用`equals()`方法判断两个元素是否相等</u>，例如`contains()`方法可以实现如下：

~~~java
public class ArrayList {
    Object[] elementData;
    public boolean contains(Object o) {
        for (int i = 0; i < size; i++) {
            if (o.equals(elementData[i])) {
                return true;
            }
        }
        return false;
    }
}

~~~

因此，**要正确使用`List`的`contains()`、`indexOf()`这些方法，放入的实例必须正确覆写`equals()`方法**，否则，放进去的实例，查找不到。<u>我们之所以能正常放入`String`、`Integer`这些对象，是因为Java标准库定义的这些类已经正确实现了`equals()`方法。</u>



我们以`Person`对象为例，测试一下：

~~~java
public class Main {
    public static void main(String[] args) {
        List<Person> list = List.of(
            new Person("Xiao Ming"),
            new Person("Xiao Hong"),
            new Person("Bob")
        );
        System.out.println(list.contains(new Person("Bob"))); // false
    }
}

class Person {
    String name;
    public Person(String name) {
        this.name = name;
    }
}

~~~

不出意外，虽然放入了`new Person("Bob")`，但是用另一个`new Person("Bob")`查询不到，原因就是`Person`类没有覆写`equals()`方法。

### 编写equals

如何正确编写`equals()`方法？`equals()`方法要求我们必须满足以下条件：

- 自反性（Reflexive）：对于非`null`的`x`来说，`x.equals(x)`必须返回`true`；
- 对称性（Symmetric）：对于非`null`的`x`和`y`来说，如果`x.equals(y)`为`true`，则`y.equals(x)`也必须为`true`；
- 传递性（Transitive）：对于非`null`的`x`、`y`和`z`来说，如果`x.equals(y)`为`true`，`y.equals(z)`也为`true`，那么`x.equals(z)`也必须为`true`；
- 一致性（Consistent）：对于非`null`的`x`和`y`来说，只要`x`和`y`状态不变，则`x.equals(y)`总是一致地返回`true`或者`false`；
- 对`null`的比较：即`x.equals(null)`永远返回`false`。

上述规则看上去似乎非常复杂，但其实代码实现`equals()`方法是很简单的，我们以`Person`类为例：

```
public class Person {
    public String name;
    public int age;
}
```

首先，我们要定义“相等”的逻辑含义。<u>对于`Person`类，如果`name`相等，并且`age`相等，我们就认为两个`Person`实例相等</u>。

因此，编写`equals()`方法如下：

```
public boolean equals(Object o) {
    if (o instanceof Person) {
        Person p = (Person) o;
        return this.name.equals(p.name) && this.age == p.age;
    }
    return false;
}
```

对于引用字段比较，我们使用`equals()`，对于基本类型字段的比较，我们使用`==`。

如果`this.name`为`null`，那么`equals()`方法会报错，因此，需要继续改写如下：

```java
public boolean equals(Object o) {
    if (o instanceof Person) {
        Person p = (Person) o;
        boolean nameEquals = false;
        if (this.name == null && p.name == null) {
            nameEquals = true;
        }
        if (this.name != null) {
            nameEquals = this.name.equals(p.name);
        }
        return nameEquals && this.age == p.age;
    }
    return false;
}
```

如果`Person`有好几个引用类型的字段，上面的写法就太复杂了。要简化引用类型的比较，我们使用`Objects.equals()`静态方法：

```java
public boolean equals(Object o) {
    if (o instanceof Person) {
        Person p = (Person) o;
        return Objects.equals(this.name, p.name) && this.age == p.age;
    }
    return false;
}
```

因此，我们总结一下`equals()`方法的正确编写方法：

1. 先确定实例“相等”的逻辑，即哪些字段相等，就认为实例相等；
2. 用`instanceof`判断传入的待比较的`Object`是不是当前类型，如果是，继续比较，否则，返回`false`；
3. 对引用类型用`Objects.equals()`比较，对基本类型直接用`==`比较。

使用`Objects.equals()`比较两个引用类型是否相等的目的是省去了判断`null`的麻烦。两个引用类型都是`null`时它们也是相等的。

<u>如果不调用`List`的`contains()`、`indexOf()`这些方法，那么放入的元素就不需要实现`equals()`方法</u>。



例：

~~~java
package  demo;


import java.util.*;

public class Main{
    public static void main(String[] args) {
        Collection c=new ArrayList();
        Student s1=new Student("jack");
        Student s2=new Student("jack");

        c.add(s1);
        System.out.println(c.contains(s2));


    }
}

class Student{
    private String name;

    public Student(String name){
        this.name=name;
    }

    //重写equals方法
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Student student = (Student) o;
        return Objects.equals(name, student.name);
    }


}
~~~



练习：

给Person类增加equals方法，使得调用indexOf()方法返回正常：

~~~java
package demo;

import java.util.List;
import java.util.Objects;

public class Main {
    public static void main(String[] args) {
        List<Person> list = List.of(
                new Person("xiaoming", "xiamhong", 18),
                new Person("xiaohua", "xiamhong", 18),
                new Person("xiaori", "xiaocai", 18)
        );
        boolean exit = list.contains(new Person("xiaori", "xiaocai", 18));
        System.out.print(exit ? "测试成功" : "测试失败");
    }
}


class Person {
    String firstName;
    String lastName;
    int age;

    public Person(String firstName, String lastName, int age) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.age = age;
    }

    @Override
    public boolean equals(Object o) {
        if (o instanceof Person) {
            Person p = (Person) o;
            return Objects.equals(this.firstName, p.firstName) && Objects.equals(this.lastName, p.lastName) && this.age == p.age;
        }
        return false;
    }
}
~~~



## 使用Map

我们知道，`List`是一种顺序列表，如果有一个存储学生`Student`实例的`List`，要在`List`中根据`name`查找某个指定的`Student`的分数，应该怎么办？

最简单的方法是遍历`List`并判断`name`是否相等，然后返回指定元素：

```java
List<Student> list = ...
Student target = null;
for (Student s : list) {
    if ("Xiao Ming".equals(s.name)) {
        target = s;
        break;
    }
}
System.out.println(target.score);
```

这种需求其实非常常见，即通过一个键去查询对应的值。使用`List`来实现存在效率非常低的问题，因为平均需要扫描一半的元素才能确定，而`Map`这种键值（key-value）映射表的数据结构，作用就是能高效通过`key`快速查找`value`（元素）。

~~~java
public class Main {
    public static void main(String[] args) {
        Student s = new Student("Xiao Ming", 99);
        Map<String, Student> map = new HashMap<>();
        map.put("Xiao Ming", s); // 将"Xiao Ming"和Student实例映射并关联
        Student target = map.get("Xiao Ming"); // 通过key查找并返回映射的Student实例
        System.out.println(target == s); // true，同一个实例
        System.out.println(target.score); // 99
        Student another = map.get("Bob"); // 通过另一个key查找
        System.out.println(another); // 未找到返回null
    }
}

class Student {
    public String name;
    public int score;
    public Student(String name, int score) {
        this.name = name;
        this.score = score;
    }
}

~~~

通过上述代码可知：**`Map<K, V>`是一种键-值映射表，当我们调用`put(K key, V value)`方法时，就把`key`和`value`做了映射并放入`Map`。当我们调用`V get(K key)`时，就可以通过`key`获取到对应的`value`。如果`key`不存在，则返回`null`**。和`List`类似，`Map`也是一个接口，最常用的实现类是`HashMap`。

<u>如果只是想查询某个`key`是否存在，可以调用`boolean containsKey(K key)`方法。</u>

如果我们在存储`Map`映射关系的时候，对同一个key调用两次`put()`方法，分别放入不同的`value`，会有什么问题呢？例如：

~~~java
public class Main {
    public static void main(String[] args) {
        Map<String, Integer> map = new HashMap<>();
        map.put("apple", 123);
        map.put("pear", 456);
        System.out.println(map.get("apple")); // 123
        map.put("apple", 789); // 再次放入apple作为key，但value变为789
        System.out.println(map.get("apple")); // 789
    }
}

~~~

重复放入`key-value`并不会有任何问题，但是一个`key`只能关联一个`value`。在上面的代码中，一开始我们把`key`对象`"apple"`映射到`Integer`对象`123`，然后再次调用`put()`方法把`"apple"`映射到`789`，这时，原来关联的`value`对象`123`就被“冲掉”了。实际上，`put()`方法的签名是`V put(K key, V value)`，如果放入的`key`已经存在，`put()`方法会返回被删除的旧的`value`，否则，返回`null`。

始终牢记：Map中不存在重复的key，因为放入相同的key，只会把原有的key-value对应的value给替换掉。

此外，在一个`Map`中，虽然`key`不能重复，但`value`是可以重复的：

```java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 123);
map.put("pear", 123); // ok
```



### 遍历Map

对`Map`来说，要遍历`key`可以使用`for each`循环遍历`Map`实例的`keySet()`方法返回的`Set`集合，它包含不重复的`key`的集合：

~~~java
public class Main {
    public static void main(String[] args) {
        Map<String, Integer> map = new HashMap<>();
        map.put("apple", 123);
        map.put("pear", 456);
        map.put("banana", 789);
        for (String key : map.keySet()) {
            Integer value = map.get(key);
            System.out.println(key + " = " + value);
        }
    }
}
~~~

同时遍历`key`和`value`可以使用`for each`循环遍历`Map`对象的`entrySet()`集合，它包含每一个`key-value`映射：

~~~java
public class Main {
    public static void main(String[] args) {
        Map<String, Integer> map = new HashMap<>();
        map.put("apple", 123);
        map.put("pear", 456);
        map.put("banana", 789);
        for (Map.Entry<String, Integer> entry : map.entrySet()) {
            String key = entry.getKey();
            Integer value = entry.getValue();
            System.out.println(key + " = " + value);
        }
    }
}

~~~



`Map`和`List`不同的是，`Map`存储的是`key-value`的映射关系，并且，它*不保证顺序*。在遍历的时候，遍历的顺序既不一定是`put()`时放入的`key`的顺序，也不一定是`key`的排序顺序。使用`Map`时，任何依赖顺序的逻辑都是不可靠的。以`HashMap`为例，假设我们放入`"A"`，`"B"`，`"C"`这3个`key`，遍历的时候，每个`key`会保证被遍历一次且仅遍历一次，但顺序完全没有保证，甚至对于不同的JDK版本，相同的代码遍历的输出顺序都是不同的！

 **遍历Map时，不可假设输出的key是有序的！**

### 小结

`Map`是一种映射表，可以通过`key`快速查找`value`。

可以通过`for each`遍历`keySet()`，也可以通过`for each`遍历`entrySet()`，直接获取`key-value`。

最常用的一种`Map`实现是`HashMap`。

练习：

请编写一个根据`name`查找`score`的程序，并利用`Map`充当缓存，以提高查找效率：

~~~java
package demo;

import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Objects;

public class Main {
    public static void main(String[] args) {
        List<Student> list = List.of(
                new Student("Bob", 78),
                new Student("Alice", 85),
                new Student("Brush", 66),
                new Student("Newton", 99));
        var holder = new Students(list);
        System.out.println(holder.getScore("Bob") == 78 ? "测试成功!" : "测试失败!");
        System.out.println(holder.getScore("Alice") == 85 ? "测试成功!" : "测试失败!");
        System.out.println(holder.getScore("Tom") == -1 ? "测试成功!" : "测试失败!");
    }

}


class Students {
    List<Student> list;
    Map<String, Integer> cache;

    Students(List<Student> list) {
        this.list = list;
        cache = new HashMap<>();
    }

    /**
     * 根据name查找score，找到返回score，未找到返回-1
     */
    int getScore(String name) {
        // 先在Map中查找:
        Integer score = this.cache.get(name);
        if (score == null) {
            // TODO:
            cache.put(name, score);
            score = findInList(name);
        }
        return (score == null) ? -1 : score;
    }

    Integer findInList(String name) {
        for (var ss : this.list) {
            if (ss.name.equals(name)) {
                return ss.score;
            }
        }
        return null;
    }
}

class Student {
    String name;
    int score;

    Student(String name, int score) {
        this.name = name;
        this.score = score;
    }
}

~~~

## 编写equals和hashCode

我们知道Map是一种键-值（key-value）映射表，可以通过key快速查找对应的value。

以HashMap为例，观察下面的代码：

```
Map<String, Person> map = new HashMap<>();
map.put("a", new Person("Xiao Ming"));
map.put("b", new Person("Xiao Hong"));
map.put("c", new Person("Xiao Jun"));

map.get("a"); // Person("Xiao Ming")
map.get("x"); // null
```

`HashMap`之所以能根据`key`直接拿到`value`，原因是它内部通过空间换时间的方法，用一个大数组存储所有`value`，并根据key直接计算出`value`应该存储在哪个索引：

```ascii
  ┌───┐
0 │   │
  ├───┤
1 │ ●─┼───> Person("Xiao Ming")
  ├───┤
2 │   │
  ├───┤
3 │   │
  ├───┤
4 │   │
  ├───┤
5 │ ●─┼───> Person("Xiao Hong")
  ├───┤
6 │ ●─┼───> Person("Xiao Jun")
  ├───┤
7 │   │
  └───┘
```

如果`key`的值为`"a"`，计算得到的索引总是`1`，因此返回`value`为`Person("Xiao Ming")`，如果`key`的值为`"b"`，计算得到的索引总是`5`，因此返回`value`为`Person("Xiao Hong")`，这样，就不必遍历整个数组，即可直接读取`key`对应的`value`。

---

当我们使用`key`存取`value`的时候，就会引出一个问题：

我们放入`Map`的`key`是字符串`"a"`，但是，当我们获取`Map`的`value`时，传入的变量不一定就是放入的那个`key`对象。

换句话讲，两个`key`应该是内容相同，但不一定是同一个对象。测试代码如下：

~~~java
public class Main {
    public static void main(String[] args) {
        String key1 = "a";
        Map<String, Integer> map = new HashMap<>();
        map.put(key1, 123);

        String key2 = new String("a");
        map.get(key2); // 123

        System.out.println(key1 == key2); // false
        System.out.println(key1.equals(key2)); // true
    }
}

~~~

因为在`Map`的内部，对`key`做比较是通过`equals()`实现的，这一点和`List`查找元素需要正确覆写`equals()`是一样的，即正确使用`Map`必须保证：作为`key`的对象必须正确覆写`equals()`方法。

我们经常使用`String`作为`key`，因为`String`已经正确覆写了`equals()`方法。但如果我们放入的`key`是一个自己写的类，就必须保证正确覆写了`equals()`方法。



通过`key`计算索引的方式就是调用`key`对象的`hashCode()`方法，它返回一个`int`整数。`HashMap`正是通过这个方法直接定位`key`对应的`value`的索引，继而直接返回`value`。

因此，正确使用`Map`必须保证：

1. 作为`key`的对象必须正确覆写`equals()`方法，相等的两个`key`实例调用`equals()`必须返回`true`；
2. 作为`key`的对象还必须正确覆写`hashCode()`方法，且`hashCode()`方法要严格遵循以下规范：

- 如果两个对象相等，则两个对象的`hashCode()`必须相等；
- 如果两个对象不相等，则两个对象的`hashCode()`尽量不要相等。

即对应两个实例`a`和`b`：

- 如果`a`和`b`相等，那么`a.equals(b)`一定为`true`，则`a.hashCode()`必须等于`b.hashCode()`；
- 如果`a`和`b`不相等，那么`a.equals(b)`一定为`false`，则`a.hashCode()`和`b.hashCode()`尽量不要相等。

正确编写`equals()`的方法我们已经在[编写equals方法](https://www.liaoxuefeng.com/wiki/1252599548343744/1265116446975264)一节中讲过了，以`Person`类为例：

```
public class Person {
    String firstName;
    String lastName;
    int age;
}
```

把需要比较的字段找出来：

- firstName
- lastName
- age

然后，引用类型使用`Objects.equals()`比较，基本类型使用`==`比较。

在正确实现`equals()`的基础上，我们还需要正确实现`hashCode()`，即上述3个字段分别相同的实例，**`hashCode()`返回的`int`必须相同**：

```
public class Person {
    String firstName;
    String lastName;
    int age;

    @Override
    int hashCode() {
        int h = 0;
        h = 31 * h + firstName.hashCode();
        h = 31 * h + lastName.hashCode();
        h = 31 * h + age;
        return h;
    }
}
```

...

## 使用EnumMap

因为`HashMap`是一种通过对key计算`hashCode()`，通过空间换时间的方式，直接定位到value所在的内部数组的索引，因此，查找效率非常高。

如果作为key的对象是`enum`类型，那么，还可以使用Java集合库提供的一种`EnumMap`，它在内部以一个非常紧凑的数组存储value，并且根据`enum`类型的key直接定位到内部数组的索引，并不需要计算`hashCode()`，不但效率最高，而且没有额外的空间浪费。

我们以`DayOfWeek`这个枚举类型为例，为它做一个“翻译”功能：

~~~java
public class Main {
    public static void main(String[] args) {
        Map<DayOfWeek, String> map = new EnumMap<>(DayOfWeek.class);
        map.put(DayOfWeek.MONDAY, "星期一");
        map.put(DayOfWeek.TUESDAY, "星期二");
        map.put(DayOfWeek.WEDNESDAY, "星期三");
        map.put(DayOfWeek.THURSDAY, "星期四");
        map.put(DayOfWeek.FRIDAY, "星期五");
        map.put(DayOfWeek.SATURDAY, "星期六");
        map.put(DayOfWeek.SUNDAY, "星期日");
        System.out.println(map);
        System.out.println(map.get(DayOfWeek.MONDAY));
    }
}

~~~

使用`EnumMap`的时候，我们总是用`Map`接口来引用它，因此，实际上把`HashMap`和`EnumMap`互换，在客户端看来没有任何区别。

### 小结

如果`Map`的key是`enum`类型，推荐使用`EnumMap`，既保证速度，也不浪费空间。

使用`EnumMap`的时候，根据面向抽象编程的原则，应持有`Map`接口。

## 使用TreeMap

我们已经知道，`HashMap`是一种以空间换时间的映射表，它的实现原理决定了内部的Key是无序的，即遍历`HashMap`的Key时，其顺序是不可预测的（但每个Key都会遍历一次且仅遍历一次）。

还有一种`Map`，它在内部会对Key进行排序，这种`Map`就是`SortedMap`。注意到`SortedMap`是接口，它的实现类是`TreeMap`。

```ascii
       ┌───┐
       │Map│
       └───┘
         ▲
    ┌────┴─────┐
    │          │
┌───────┐ ┌─────────┐
│HashMap│ │SortedMap│
└───────┘ └─────────┘
               ▲
               │
          ┌─────────┐
          │ TreeMap │
          └─────────┘
```

`SortedMap`保证遍历时以Key的顺序来进行排序。例如，放入的Key是`"apple"`、`"pear"`、`"orange"`，遍历的顺序一定是`"apple"`、`"orange"`、`"pear"`，因为`String`默认按字母排序：

~~~java
package demo;


import java.time.DayOfWeek;
import java.util.EnumMap;
import java.util.Map;
import java.util.TreeMap;


public class Main{
    public static void main(String[] args){
        Map<String,Integer> map = new TreeMap<>();
        map.put("apple",2);
        map.put("orange",1);
        map.put("pear",3);
        for (String key:map.keySet()){
            System.out.println(key);
        }
    }
}
~~~



使用`TreeMap`时，放入的Key必须实现`Comparable`接口。`String`、`Integer`这些类已经实现了`Comparable`接口，因此可以直接作为Key使用。作为Value的对象则没有任何要求。

如果作为Key的class没有实现`Comparable`接口，那么，必须在创建`TreeMap`时同时指定一个自定义排序算法：

# 并发，多线程

## 创建线程的三种方式：

### 第一种方法：实现Runnable 接口，并覆写Run方法

### 第二种方法：继承Thread，并覆写Run方法

### 第三种方法：实现Callable接口，并重写call方法

### 选择使用实现Runable接口还是继承Thread

​			由于java中不支持类的多重继承，但是支持实现多个接口，因此如果需要继承其他的类，需要选择实现Runnable接口。

### start方法和run方法的区别

​			start方法由于启动线程，启动线程之后会运行线程中的run方法。如果没有调用start方法而直接调用run方法，则没有启动新线程，开辟新的栈空间，而是在原来的线程中调用run方法。

~~~java
//第一种方法：实现Runnable 接口，并覆写Run方法
public class Main{
    public static void main(String[] args){
//创建一个线程对象，构造方法需要传入一个可运行对象
        Thread t1=new Thread(new MyThread);
        t1.start;
    }
}

class MyThread implemtnts Runnable{
    
    @Override
    public void Run{
        //线程的方法
    }
}


~~~

~~~java
//第二种方法：继承Thread，并覆写Run方法
public class Main{
    public static void main(String[] args){
//创建一个线程对象
        Thread t1=new MyThread();
        t1.start;
    }
}

class MyThread extends Thread{
    
    @Override
    public void Run{
        //线程的方法
    }
}
~~~

~~~java
/*
 * 实现线程的第三种方法，实现Callable接口
 *
 * 优点：
 *      可以获得当前线程的执行结果
 * 缺点：
 *      效率比较低，在获取t线程执行结果时，当前线程受阻，效率较低。
 *
 * */
public class ThreadTest10 {
    public static void main(String[] args) throws ExecutionException, InterruptedException {

//创建一个“未来对象类”
        FutureTask futureTask = new FutureTask(new Callable() {//call()相当与run()
            @Override
            public Object call() throws Exception {

                System.out.println("线程执行");
                Thread.sleep(1000*5);
                return 22+30;//有返回值
            }
        });

        //创建线程对象
        Thread t1 = new Thread(futureTask);
//        启动线程
        t1.start();

        //如何在main线程中获取t1线程的返回值
        //通过get方法
//        get方法会导致main线程受阻
        Object o=futureTask.get();
        System.out.println(o);


//        只有到t1线程结束。main方法才能执行
        System.out.println("main线程");



    }
}
~~~

## 线程的状态

线程的状态包括了：

- 新建（new）: 刚new出来的线程
- 可运行 （Runnable）：new出来的线程通过start()方法进入可运行状态，可运行状态又可以细分为`可运行状态`和`运行状态`,其中`可运行状态表示`当前线程具有抢夺执行权的权利（java采用的是抢占式的调度方式），当一个线程抢到执行权后就开始执行Run方法进入`运行状态`，当执行权用完后会重性进入`可运行状态`。
- 阻塞 (Blocked) ： 线程遇到阻塞事件会放弃抢到的执行权，回到`可运行状态`。
- 等待 （waiting）：当线程占用锁的对象调用wait方法时，该线程会进入等待状态，进释放抢到的执行权。
- 计时等待 (Timed waiting)
- 终止 (Teminated)：线程的Run方法执行结束。



## 常见问题总结：

#### Java中Runnable和Callable有什么不同？

Runnable封装一个一部运行的任务，可以把它想象成一个没有参数和返回值的方法。Callable和Runnable方法类似，但是有返回值，Callable接口是一个参数化的类型，只有一个方法call,类型参数是返回值的类型	

#### Java中的volatile 变量是什么？

volatile关键字为实例字段提供了一种免锁机制。如果声明一个实例字段为volatile，那么编译器和java虚拟机就知道该字段可能被另一个线程并发更新。(保证了不同线程对这个变量进行操作时的可见性，即一个线程修改了某个变量的值，这新值对其他线程来说是立即可见的。)

####  什么是线程安全？Vector是一个线程安全类吗？

当存在多线程并发，并且多线程对同一个数据进行操作，且存在修改数据的行为，就会存在线程安全的问题。

Vector 是用同步方法来实现线程安全的, 而和它相似的ArrayList不是线程安全的。

#### Java中notify 和 notifyAll有什么区别？

notify:随机选择一个在这个对象上调用wait方法的线程，解除其阻塞状态。(notify方法不能唤醒某个具体的线程，所以只有一个线程在等 待的时候它才有用武之地)

notifyAll:解除所有这个对象上调用wait方法的线程 的阻塞状态。(notifyAll唤醒所有线程并允许他们争夺锁确保了至少有一个线程能继续运行)

#### 为什么wait, notify 和 notifyAll这些方法不在thread类里面？

这是个设计相关的问题，它考察的是面试者对现有系统和一些普遍存在但看起来不合理的事物的看法。回答这些问题的时候，你要说明为什么把这些方法放在  Object类里是有意义的，还有不把它放在Thread类里的原因。一个很明显的原因是JAVA提供的锁是对象级的而不是线程级的，**每个对象都有锁，通  过线程获得。如果线程需要等待某些锁那么调用对象中的wait()方法就有意义**了。如果wait()方法定义在Thread类中，线程正在等待的是哪个锁 就不明显了。简单的说，由于wait，notify和notifyAll都是锁级别的操作，所以把他们定义在Object类中因为锁属于对象。

#### 什么是ThreadLocal变量？

ThreadLocal使用场合主要解决多线程中数据数据因并发产生不一致问题。ThreadLocal为每个线程的中并发访问的数据提供一个副本，通过访问副本来运行业务，这样的结果是耗费了内存，单大大减少了线程同步所带来性能消耗，也减少了线程并发控制的复杂度。

ThreadLocal不能使用原子类型，只能使用Object类型。ThreadLocal的使用比synchronized要简单得多。

ThreadLocal和Synchonized都用于解决多线程并发访问。但是ThreadLocal与synchronized有本质的区别。synchronized是利用锁的机制，使变量或代码块在某一时该只能被一个线程访问。而**ThreadLocal为每一个线程都提供了变量的副本，使得每个线程在某一时间访问到的并不是同一个对象，这样就隔离了多个线程对数据的数据共享**。而Synchronized却正好相反，它用于在多个线程间通信时能够获得数据共享。

#### Java多线程中调用wait() 和 sleep()方法有什么不同？

**wait()会释放锁，而sleep不会释放锁**。

Java程序中wait 和 sleep都会造成某种形式的暂停，它们可以满足不同的需要。wait()方法用于线程间通信，如果等待条件为真且其它线程被唤醒时它会释放锁，而 sleep()方法仅仅释放CPU资源或者让当前线程停止执行一段时间，但不会释放锁。

####  Java中interrupted 和 isInterruptedd方法的区别？

Thread.interruped()是一个静态方法，它检查**当前线程是否被中断**，而且调用该方法会清除该线程的中断状态。

isInterruped()是一个实例方法，用于检查是否有线程被中断。调用这个方法不会改变中断状态。

#### Java中堆和栈有什么不同？

为什么把这个问题归类在多线程和并发面试题里？因为栈是一块和线程紧密相关的内存区域。<u>每个线程都有自己的栈内存</u>，用于存储本地变量，方法参数和栈  调用，一个线程中存储的变量对其它线程是不可见的。而堆是所有线程共享的一片公用内存区域。<u>对象都在堆里创建</u>，为了提升效率线程会从堆中弄一个缓存到自己 的栈，如果多个线程使用该变量就可能引发问题，这时volatile 变量就可以发挥作用了，它要求线程从主存中读取变量的值。

#### 怎么检测一个线程是否拥有锁？

在java.lang.Thread中有一个方法叫holdsLock()，它返回true如果当且仅当当前线程拥有某个具体对象的锁。

#### 有三个线程T1，T2，T3，怎么确保它们按顺序执行？

使用join方法

用线程类的join()方法在一个线程中启动另一个线程，另外一个线程完成该线程继续执行。为了确保三个线程的顺序你应该先启动最后一个(T3调用T2，T2调用T1)，这样T1就会先完成而T3最后完成。

#### Thread类中的yield方法有什么作用？

Yield方法可以暂停当前正在执行的线程对象，让其它有相同优先级的线程执行。它是一个静态方法而且只保证当前线程放弃CPU占用而不能保证使其它线程一定能占用CPU，执行yield()的线程有可能在进入到暂停状态后马上又被执行。

#### volatile 变量和 atomic 变量有什么不同？

这是个有趣的问题。首先，volatile 变量和 atomic  变量看起来很像，但功能却不一样。Volatile变量可以确保先行关系，即写操作会发生在后续的读操作之前,  但它并不能保证原子性。例如用volatile修饰count变量那么 count++  操作就不是原子性的。而AtomicInteger类提供的atomic方法可以让这种操作具有原子性如getAndIncrement()方法会原子性 的进行增量操作把当前值加一，其它数据类型和引用变量也可以进行相似操作。

#### 写出3条你遵循的多线程最佳实践

- 给你的线程起个有意义的名字。
  这样可以方便找bug或追踪。OrderProcessor, QuoteProcessor or  TradeProcessor 这种名字比 Thread-1. Thread-2 and Thread-3  好多了，给线程起一个和它要完成的任务相关的名字，所有的主要框架甚至JDK都遵循这个最佳实践。
- 避免锁定和缩小同步的范围
  锁花费的代价高昂且上下文切换更耗费时间空间，试试最低限度的使用同步和锁，缩小临界区。因此相对于同步方法我更喜欢同步块，它给我拥有对锁的绝对控制权。
- 多用同步类少用wait 和 notify
  首先，CountDownLatch, Semaphore, CyclicBarrier 和 Exchanger  这些同步类简化了编码操作，而用wait和notify很难实现对复杂控制流的控制。其次，这些类是由最好的企业编写和维护在后续的JDK中它们还会不断 优化和完善，使用这些更高等级的同步工具你的程序可以不费吹灰之力获得优化。
- 多用并发集合少用同步集合
  这是另外一个容易遵循且受益巨大的最佳实践，并发集合比同步集合的可扩展性更好，所以在并发编程时使用并发集合效果更好。如果下一次你需要用到map，你应该首先想到用ConcurrentHashMap。

#### 如何避免死锁？

Java多线程中的死锁
死锁是指两个或两个以上的进程在执行过程中，因争夺资源而造成的一种互相等待的现象，若无外力作用，它们都将无法推进下去。这是一个严重的问题，因为死锁会让你的程序挂起无法完成任务，死锁的发生必须满足以下四个条件：

- 互斥条件：一个资源每次只能被一个进程使用。
- 请求与保持条件：一个进程因请求资源而阻塞时，对已获得的资源保持不放。
- 不剥夺条件：进程已获得的资源，在末使用完之前，不能强行剥夺。
- 循环等待条件：若干进程之间形成一种头尾相接的循环等待资源关系。

避免死锁最简单的方法就是阻止循环等待条件，将系统中所有的资源设置标志位、排序，规定所有的进程申请资源必须以一定的顺序（升序或降序）做操作来避免死锁。

#### Java中的同步集合与并发集合有什么区别？

同步集合与并发集合都为多线程和并发提供了合适的线程安全的集合，不过并发集合的可扩展性更高。在Java1.5之前程序员们只有同步集合来用且在  多线程并发的时候会导致争用，阻碍了系统的扩展性。Java5介绍了并发集合像ConcurrentHashMap，不仅提供线程安全还用锁分离和内部分 区等现代技术提高了可扩展性。

####  什么是线程池？ 为什么要使用它？

创建线程要花费昂贵的资源和时间，如果任务来了才创建线程那么响应时间会变长，而且一个进程能创建的线程数有限。为了避免这些问题，在程序启动的时  候就创建若干线程来响应处理，它们被称为线程池，里面的线程叫工作线程。从JDK1.5开始，Java  API提供了Executor框架让你可以创建不同的线程池。比如单线程池，每次处理一个任务；数目固定的线程池或者是缓存线程池（一个适合很多生存期短 的任务的程序的可扩展线程池）。

---


