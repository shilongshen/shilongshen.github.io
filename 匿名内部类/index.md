# 匿名内部类




### 什么是内部类：定义在另一个类中的类称为内部类

### 内部类的分类：

 *   1.静态内部类：类似静态变量
 *   2.实例内部类：类似实例变量
 *  3.局部内部类：类似局部变量



代码示例：

```java
class Test {
    //静态变量->变量属于类
    static int a;
    //静态内部类
    static class inner1 {

    }

    //实例变量->属于实例
    int b;
    //实例内部类
    class inner2 {

    }

    //方法
    public void method1() {
        //方法中的变量称为局部变量
        int c;
        //定义在方法内部的类称为局部内部类
        class inner3 {

        }

    }
}
```





### 匿名内部类是局部内部类的一种，因为这种类没有名字而得名

首先我们先来看不是用匿名内部类时的情况：

```java
package InnerClass;

/**
 * ClassName:    InnerClass
 * Package:    InnerClass
 * Description:
 * Datetime:    2020/11/20   下午12:21
 * Author:   shilongshen
 */

public class InnerClass {
    public static void main(String[] args) {
        MySum mySum=new MySum();
        /*
        * MySum方法的第一个参数为Computeer 引用类型，但是Computeer为接口，无法实例化，所以使用了Computer c=new Computerimplement()
        * 即父类引用指向子类对象。
        * 执行int value=c.sum(x,y);时调用的是子类中的方法->多态
        * */
       mySum.mysum(new Computerimplement(),100,200);
        
    }
}

interface Computer{//接口
    int sum(int a, int b);
}

class  Computerimplement implements Computer{
    @Override
    public int sum(int a, int b) {
        return a+b;
    }
}

class MySum{
    public void mysum(Computer c,int x,int y){//MySum方法的第一个参数为Computeer 引用类型
        int value=c.sum(x,y);
        System.out.println(x+"+"+y+"="+value);

    }
}

```

但是这样我们要多定义一个接口Computer的实现类Computerimplement，比较麻烦。来看看匿名内部类是如何解决的。

```java
package InnerClass;

/**
 * ClassName:    InnerClass
 * Package:    InnerClass
 * Description:
 * Datetime:    2020/11/20   下午12:21
 * Author:   shilongshen
 */

public class InnerClass {
    public static void main(String[] args) {
        MySum mySum=new MySum();
       
        
        /*
       * 使用匿名内部类
       * */
       mySum.mysum(new Computer() {
           @Override
           public int sum(int a, int b) {
               return a+b;
           }
       },100,200);
        
    }
}

interface Computer{//接口
    int sum(int a, int b);
}

class MySum{
    public void mysum(Computer c,int x,int y){//MySum方法的第一个参数为Computeer 引用类型
        int value=c.sum(x,y);
        System.out.println(x+"+"+y+"="+value);

    }
}
```

匿名内部类本质上也是定义了一个接口的实现类，只不过这个实现类没有名字。

匿名内部类的定义方式：

~~~java
new 接口名 (){
    @Override//重写接口中的方法
    ...
}
~~~


