# 常用函数


### 将字符串转换为字符：

```java
String str = "42";
char[] chars = str.toCharArray();
```

### 去除字符串的空格：

```java
String trim //trim()方法移除字符串两侧(头尾)的空白字符(空格、tab键、换行符)
String strip
    
    
    
1、trim()方法不足之处

trim()早在Java早期就存在，当时Unicode还没有完全发展到我们今天广泛使用的标准。

trim()方法移除字符串两侧的空白字符(空格、tab键、换行符)

支持Unicode的空白字符的判断应该使用isWhitespace(int)。

此外，开发人员无法专门删除缩进空白或专门删除尾随空白。

简单得说就是，trim()方法无法删除掉Unicode空白字符，但用Character.isWhitespace©方法可以判断出来。

2、strip()方法

JAVA11(JDK11)中的strip()方法，适用于字符首尾空白是Unicode空白字符的情况，通过一段代码来具体看一下，

public static void main(String[] args) {
String s = “\t abc \n”;
System.out.println( “abc”.equals(s.trim()));//true
System.out.println(“abc”.equals(s.trim()));//true
Character c = ‘\u2000’;
String s1 = c + “abc” + c;
System.out.println(Character.isWhitespace©);//true
System.out.println(s1.equals(s1.trim()));//true，trim无法删除Unicdoe空白字符
System.out.println(“abc”.equals(s1.strip()));//true
}
上面输出结果都是true， Character c = ‘\u2000’;中’\u2000’就是Unicdoe空白字符。
```

### 切割字符串：

```java
String substring(int beginIndex, int endIndex)
    
    得到的字符从beginIndex到endIndex-1
```

### 构建字符串：

```java
StringBuilder build=new StringBuilder();//构造StringBuilder

build.append(String s);//向StringBuilder中添加元素，可以是字符串或字符，也可以是其他值，具体请参照API
build.append(char c);

build.toString();//将StringBuilder转换为String

```



### 依次读取字符串中的字符：

```java
char charAt(int index)
//例如：
    String str = "42";
	char chars=str.charAt(0);//'4'
```

### 整型的极值：

```java
int max=Integer.MAX_VALUE;//2147483647
int min=Integer.MIN_VALUE;//-2147483648
```

### 将字符串转换为数字：

```java
String str = "42";
int a=Integer.parseInt(str);//42
```

### 将字符转换为字符串：

```java
tring str=String.valueOf('2');
```



```
一、string 和int之间的转换

1、string转换成int  :Integer.valueOf("12")

2、int转换成string : String.valueOf(12)

二、char和int之间的转换

1、首先将char转换成string

String str=String.valueOf('2')

2、转换

Integer.valueof(str) 或者Integer.PaseInt(str)

Integer.valueof返回的是Integer对象，Integer.paseInt返回的是int
```

### 优先队列（堆）：

```java
//默认是小顶堆，即最小的值在根结点
//可以通过重写comparator,作为比较器传入构造函数中，将其修改为大顶堆
PriorityQueue<Integer> priorityQueue = new PriorityQueue<>(new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o2 - o1;
            }
        });
```

### 打印数组：

```java
int[] nums = {1, 3, -1, -3, 5, 3, 6, 7};
System.out.println(Arrays.toString(a));
```


