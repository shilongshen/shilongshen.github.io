# 多态的浅显理解



# **多态**：

- 定义：一个对象变量（对象引用）可以指向多个实际类型的现象称为**<u>多态</u>**。 
  - 例如：父类引用指向子类对象，父类引用可以指向父类对象。但是子类引用不能指向父类对象

- 对象引用调用的方法究竟属于哪一个对象（实际类型）是在运行期间动态确定的，这称为**<u>动态绑定</u>**。
  - 当父类引用指向子类对象。如果子类重写了父类的方法，真正执行的是子类覆盖的方法，如果子类没有覆盖父类的方法，执行的是父类的方法。



例：

son的实际类型是Son,它是Father的子类。如果Son定义了方法（重写）method1，则调用Son中的method1,否则调用Father中的method1。这是在运行期间动态决定的。

~~~java
public class    test01 {
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
- 多态不能调用“只在子类存在但在父类不存在”的方法；
- 如果子类重写了父类的方法，真正执行的是子类覆盖的方法，如果子类没有覆盖父类的方法，执行的是父类的方法。

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201120140547.png)
