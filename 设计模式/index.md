# 设计模式


# 什么是设计模式

- 设计模式是软件设计中常见问题的<u>典型解决方案</u>。

- 每个设计模式就像是一张蓝图，你可以对其进行定制来解决代码中的特定设计问题。

> 设计模式与方法或库的使用方式不同， 你很难直接在自己的程序中套用某个设计模式。 模式并不是一段特定的代码， 而是解决特定问题的一般性概念。 你可以根据模式来实现符合自己程序实际所需的解决方案。

- 模式是针对软件设计中常见问题的解 决方案工具箱， 它们定义了一种让你的团队能更高效沟通的通用语言。

# 简单工厂方法

**定义一个工厂类，他可以根据参数的不同返回不同类的实例，被创建的实例通常都具有共同的父类**

```java
//定义一个接口
public interface Product {
}

//CreateProduct1,CreateProduct2,CreateProduct3都是接口的实现类
class CreateProduct1 implements Product{

}

class CreateProduct2 implements Product{

}

class CreateProduct3 implements Product{

}
```

```java
//定义简单工厂类
//可以通过参数（type）的选择返回不同的实例(CreateProduct1,CreateProduct2,CreateProduct3)
public class SampleFactoryMode {
    public Product creatrProduct(int type){
        if (type==1){
            return new CreateProduct1();
        }
        else if(type==2){
            return new CreateProduct2();
        }
        else {
            return new CreateProduct3();
        }
    }
}
```

```java
//主方法通过直接调用简单工厂类，来得到不同的实例（通过不同的参数），而不需要知道实例中的具体实现
public class main {
    public static void main(String[] args) {
        SampleFactoryMode sampleFactoryMode=new SampleFactoryMode();
        Product product=sampleFactoryMode.creatrProduct(1);
    }
}
```

1. SampleFactoryMode(工厂):**核心部分**，负责实现创建所有产品的内部逻辑，**工厂类可以被外界直接调用，创建所需对象**
2. Product(抽象类产品)：工厂类所创建的所有对象的父类，封装了产品对象的公共方法，所有的具体产品为其子类对象
3. CreateProduct(具体产品)：简单工厂模式的创建目标，所有被创建的对象都是某个具体类的实例。它要实现抽象产品中声明的抽象方法(有关[抽象类](https://baike.baidu.com/item/抽象类))



# 工厂方法

[参考](https://blog.csdn.net/qq_42428264/article/details/90721656)

考虑这样一个系统,使用简单工厂模式设计的按钮工厂类可以返回一个具体类型的 按钮实例,例如圆形按钮矩形按钮、菱形按钮等。

<img src="https://gitee.com/shilongshen/image-bad/raw/master/img/20210111212840.png" style="zoom:80%;" />

在这个系统中如果需要<u>增加一种新类型的按钮</u>,例如椭圆形按钮,那么除了增加一个新的具体产品类之外<u>还需要修改工厂类的代码</u>,这就使得整个设计在一定程度上违反了开闭原则。

现在对该系统进行修改,不再提供一个按钮工厂类来统一负责所有产品的创建,而是**将具体按钮的创建过程交给专门的工厂子类去完成**。

1. 先定义一个**抽象的按钮工厂类**

2. 再定义**具体的工厂类**来生产圆形按钮、矩形按钮、菱形按钮等,它们实现了在抽象按钮工厂类中声明的方法

   ![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210111213229.png)

这种抽象化的结果是使得这种结构可以在不修改具体工厂类的情况下引进新的产品,<u>如果出现新的按钮类型,只需要为这种新类型的按钮定义一个具体的工厂类就可以创建该新按钮的实例</u>,这种改进的设计方案即为工厂方法模式

> 在工厂方法模式中不再提供一个统一的工厂类来创建所有的产品对象,而是针对不同的产品提供不同的工厂
>
> **工厂方法模式:定义一个用于创建对象的工厂接口/或抽象类,但是让工厂实现类决定将哪一个产品类实例化。工厂方法模式让一个类的实例化延迟到其子类**

```java
public interface Product {
}

class CreateProduct1 implements Product {

}

class CreateProduct2 implements Product{

}

class CreateProduct3 implements Product{

}
```

```java
//创建抽象工厂类
public abstract class FactoryMode {
    abstract public Product factorymethod();
}

//创建具体工厂类，每一个具体工厂类负责实例化一个产品
class CreateFactory1 extends FactoryMode {

    @Override
    public Product factorymethod() {
        return new CreateProduct1();
    }
}

class CreateFactort2 extends FactoryMode {

    @Override
    public Product factorymethod() {
        return new CreateProduct2();
    }
}

class CreateFactory3 extends FactoryMode {

    @Override
    public Product factorymethod() {
        return new CreateProduct3();
    }
}
```

```java
public class main {
    public static void main(String[] args) {
        FactoryMode factoryMode=new CreateFactory1();//主方法可以通过选择具体工厂类来确定返回的实例化的产品
        Product product=factoryMode.factorymethod();//这时可以得到特定的实例化产品
        //do something
    }
}
```



# 抽象工厂方法

同种类称为同等级，也就是说：[工厂方法模式](http://c.biancheng.net/view/1348.html)只考虑生产同等级的产品，但是在现实生活中许多工厂是综合型的工厂，能生产多等级（种类） 的产品，如农场里既养动物又种植物，电器厂既生产电视机又生产洗衣机或空调，大学既有软件专业又有生物专业等。

抽象工厂模式将考虑<u>多等级产品的生产</u>，<u>将同一个具体工厂所生产的位于不同等级的一组产品称为一个产品族</u>

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210112085225.png)

> 抽象工厂模式创建的是对象家族，也就是很多对象而不是一个对象，并且<u>这些对象是相关的，也就是说必须一起创建出来</u>。而工厂方法模式只是用于创建一个对象，这和抽象工厂模式有很大不同。
>
> 抽象工厂方法可以创建出多个产品等级的对象，例如[海尔空调，TCL空调] ; [海尔电视机，TCL电视机]

抽象工厂模式的主要角色如下。

1. ​		抽象工厂（Abstract Factory）：提供了创建产品的接口，它包含多个创建产品的方法 newProduct()，可以创建多个不同等级的产品。
2. ​		具体工厂（Concrete Factory）：主要是实现抽象工厂中的多个抽象方法，完成具体产品的创建。有多个具体工厂，每一个具体工厂负责创建同一等级的具体产品（这些具体产品是相关的，必须同时创建）
3. ​		抽象产品（Product）：定义了产品的规范，描述了产品的主要特性和功能，抽象工厂模式有多个抽象产品。有多个种类的抽象产品
4. ​		具体产品（ConcreteProduct）：实现了抽象产品角色所定义的接口，由具体工厂来创建，它同具体工厂之间是多对一的关系。



//创建两个不同的产品接口，代表两类不同的产品

```java
public interface ProductA {
}
```

```java
public interface ProductB {
}
```

//为每一个产品接口创建具体的产品类

//产品A有两个具体产品A1，A2；产品B有两个具体产品B1，B2

```java
public class CreateProductA1  implements ProductA  {
}
```

```java
public class CreateProductA2 implements ProductA {
}
```

```java
public class CreateProductB1 implements ProductB {
}
```

```java
public class CreateProductB2 implements ProductB {
}
```

//创建抽象工厂类,其中有两个抽象方法

```java
public abstract class AbstarctFactory {
    abstract ProductA CreateFactoryA();
    abstract ProductB CreateFactoryB();
}
```

//具体工厂类,每一个具体工厂类负责创建属于不同产品但是属于同一等级的产品（注意此处与上述有出入，）

```java
public class ConcreteFactory1 extends AbstarctFactory {

    @Override
    ProductA CreateFactoryA() {
        return new CreateProductA1();
    }

    @Override
    ProductB CreateFactoryB() {
        return new CreateProductB1();
    }
}
```

```java
public class ConcreteFactory2 extends AbstarctFactory {
    @Override
    ProductA CreateFactoryA() {
        return new CreateProductA2();
    }

    @Override
    ProductB CreateFactoryB() {
        return new CreateProductB2();
    }
}
```

//主方法可以通过直接调用工厂类来获得想要的产品

```java
public class main {
    public static void main(String[] args) {
        AbstarctFactory Factory=new ConcreteFactory1();
        ProductA productA1= Factory.CreateFactoryA();
        ProductB productB1=Factory.CreateFactoryB();
        //do something with productA1,productB1 
    }
}
```



另外一个例子

```java
package AbstractFactory;

import java.awt.*;
import javax.swing.*;



//抽象产品：动物类
interface Animal {
    public void show();
}

//具体产品：马类
class Horse implements Animal {
    JScrollPane sp;
    JFrame jf = new JFrame("抽象工厂模式测试");

    public Horse() {
        Container contentPane = jf.getContentPane();
        JPanel p1 = new JPanel();
        p1.setLayout(new GridLayout(1, 1));
        p1.setBorder(BorderFactory.createTitledBorder("动物：马"));
        sp = new JScrollPane(p1);
        contentPane.add(sp, BorderLayout.CENTER);
        JLabel l1 = new JLabel(new ImageIcon("src/A_Horse.jpg"));
        p1.add(l1);
        jf.pack();
        jf.setVisible(false);
        jf.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);//用户点击窗口关闭
    }

    public void show() {
        jf.setVisible(true);
    }
}

//具体产品：牛类
class Cattle implements Animal {
    JScrollPane sp;
    JFrame jf = new JFrame("抽象工厂模式测试");

    public Cattle() {
        Container contentPane = jf.getContentPane();
        JPanel p1 = new JPanel();
        p1.setLayout(new GridLayout(1, 1));
        p1.setBorder(BorderFactory.createTitledBorder("动物：牛"));
        sp = new JScrollPane(p1);
        contentPane.add(sp, BorderLayout.CENTER);
        JLabel l1 = new JLabel(new ImageIcon("src/A_Cattle.jpg"));
        p1.add(l1);
        jf.pack();
        jf.setVisible(false);
        jf.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);//用户点击窗口关闭
    }

    public void show() {
        jf.setVisible(true);
    }
}

//抽象产品：植物类
interface Plant {
    public void show();
}

//具体产品：水果类
class Fruitage implements Plant {
    JScrollPane sp;
    JFrame jf = new JFrame("抽象工厂模式测试");

    public Fruitage() {
        Container contentPane = jf.getContentPane();
        JPanel p1 = new JPanel();
        p1.setLayout(new GridLayout(1, 1));
        p1.setBorder(BorderFactory.createTitledBorder("植物：水果"));
        sp = new JScrollPane(p1);
        contentPane.add(sp, BorderLayout.CENTER);
        JLabel l1 = new JLabel(new ImageIcon("src/P_Fruitage.jpg"));
        p1.add(l1);
        jf.pack();
        jf.setVisible(false);
        jf.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);//用户点击窗口关闭
    }

    public void show() {
        jf.setVisible(true);
    }
}

//具体产品：蔬菜类
class Vegetables implements Plant {
    JScrollPane sp;
    JFrame jf = new JFrame("抽象工厂模式测试");

    public Vegetables() {
        Container contentPane = jf.getContentPane();
        JPanel p1 = new JPanel();
        p1.setLayout(new GridLayout(1, 1));
        p1.setBorder(BorderFactory.createTitledBorder("植物：蔬菜"));
        sp = new JScrollPane(p1);
        contentPane.add(sp, BorderLayout.CENTER);
        JLabel l1 = new JLabel(new ImageIcon("src/P_Vegetables.jpg"));
        p1.add(l1);
        jf.pack();
        jf.setVisible(false);
        jf.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);//用户点击窗口关闭
    }

    public void show() {
        jf.setVisible(true);
    }
}

//抽象工厂：农场类
interface Farm {
    public Animal newAnimal();

    public Plant newPlant();
}

//具体工厂：韶关农场类
class SGfarm implements Farm {
    public Animal newAnimal() {
        System.out.println("新牛出生！");
        return new Cattle();
    }

    public Plant newPlant() {
        System.out.println("蔬菜长成！");
        return new Vegetables();
    }
}

//具体工厂：上饶农场类
class SRfarm implements Farm {
    public Animal newAnimal() {
        System.out.println("新马出生！");
        return new Horse();
    }

    public Plant newPlant() {
        System.out.println("水果长成！");
        return new Fruitage();
    }
}


```

小结

<img src="https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210412194542.png" style="zoom:80%;" />

# 单例模式

[参考](http://c.biancheng.net/view/1338.html)

**确保一个类只有一个实例，并提供一个该实例的全局访问点。**

单例模式有 3 个特点：

1. ​		单例类只有一个实例对象；
2. ​		<u>该单例对象必须由单例类自行创建</u>；
3. ​		单例类对外提供一个访问该单例的全局访问点。

单例模式的优点：

- ​		单例模式可以保证内存里只有一个实例，减少了内存的开销。
- ​		可以避免对资源的多重占用。
- ​		单例模式设置全局访问点，可以优化和共享资源的访问。


 单例模式的缺点：

- ​		<u>单例模式一般没有接口，扩展困难。如果要扩展，则除了修改原来的代码</u>，没有第二种途径，违背开闭原则。
- ​		在并发测试中，单例模式不利于代码调试。在调试过程中，如果单例中的代码没有执行完，也不能模拟生成一个新的对象。
- ​		单例模式的功能代码通常写在一个类中，如果功能设计不合理，则很容易违背单一职责原则。

单例模式的应用场景：

- 例如，**Windows 中只能打开一个任务管理器**，这样可以避免因打开多个任务管理器窗口而造成内存资源的浪费，或出现各个窗口显示内容的不一致等错误。

- 在计算机系统中，还有 Windows 的回收站、操作系统中的文件系统、多线程中的线程池、显卡的驱动程序对象、打印机的后台处理服务、应用程序的日志对象、数据库的连接池、网站的计数器、Web 应用的配置对象、应用程序中的对话框、系统中的缓存等常常被设计成单例。

- 单例模式在现实生活中的应用也非常广泛，例如公司 CEO、部门经理等都属于单例模型



单例模式的主要角色如下。

- ​		单例类：包含一个实例且能自行创建这个实例的类。
- ​		访问类：使用单例的类。

Singleton 模式通常有两种实现形式。

## 	第 1 种：懒汉式单例

该模式的特点是**类加载时没有生成单例，只有当第一次调用 静态方法getlnstance 方法时才去创建这个单例**。代码如下：

```java
public class LazySingleton {
    private static volatile LazySingleton instance = null;    //保证 instance 在所有线程中同步

    private LazySingleton() {//私有构造函数
    }    //private 避免类在外部被实例化

    public static synchronized LazySingleton getInstance() {//静态方法，供外界获取它的静态实例
        //getInstance 方法前加同步，属于类方法，可以通过类名直接调用
        if (instance == null) {
            instance = new LazySingleton();//实例由类自行创建
        }
        return instance;//提供一个全局访问点
    }
}


```

注意：如果编写的是多线程程序，则不要删除上例代码中的关键字 volatile 和 synchronized，否则将存在线程非安全的问题。如果不删除这两个关键字就能保证线程安全，但是每次访问时都要同步，会影响性能，且消耗更多的资源，这是懒汉式单例的缺点。

> 为什么不能删除volatile关键字？
>
> 因为`instance = new LazySingleton();`不是原子操作，这段代码可以分为以下三个部分：
>
> - 为对象`LazySingleton`分配空间
> - 对象初始化
> - 将`instance`指向分配的地址空间
>
> 但是由于JVM具有指令重排的特性，执行顺序可能变成1->3->2。指令重排在单线程环境下不会出问题，但是在多线程环境下会导致一个线程获得还没有实例化的对象。例如线程a执行了1，3，此时线程b调用getInstance后发现instance不为空，因此返回instance，但是此时instance还没有被初始化，所以会导致空指针异常。
>
> 使用volatile关键字修饰变量可以避免JVM指令重排，使得变量在线程间具有可见性，这样就可以在多线程下使用了。

## 	第 2 种：饿汉式单例

该模式的特点是**类一旦加载就创建一个单例**，保证在调用 getInstance 方法之前单例已经存在了。

```java
public class HungrySingleton {
    private static final HungrySingleton instance = new HungrySingleton();//单例对象由单例类自行创建

    private HungrySingleton() {
    }

    public static HungrySingleton getInstance() {
        return instance;
    }
}
```

饿汉式单例在类创建的同时就已经创建好一个**静态**的对象供系统使用，以后不再改变，所以是线程安全的，可以直接用于多线程而不会出现问题。

> 懒汉式：要加锁，效率较低
>
> 饿汉式：无论用不用都先实例化，会浪费内存

## 第 3 种：双重校验锁

> 执行双重检查是因为，如果多个线程同时了通过了第一次检查，并且其中一个线程首先通过了第二次检查并实例化了对象，那么剩余通过了第一次检查的线程就不会再去实例化对象。这样，除了初始化的时候会出现加锁的情况，后续的所有调用都会避免加锁而直接返回，解决了性能消耗的问题。
>
> 分析：第一次校验：由于单例模式只需要创建一次实例，如果后面再次调用getInstance方法时，则直接返回之前创建的实例，因此大部分时间不需要执行同步方法里面的代码，大大提高了性能。<u>如果不加第一次校验的话，那跟上面的懒汉模式没什么区别，每次都要去竞争锁。</u>
>
> 　　 第二次校验：如果没有第二次校验，假设线程t1执行了第一次校验后，判断为null，这时t2也获取了CPU执行权，也执行了第一次校验，判断也为null。接下来t2获得锁，创建实例。这时t1又获得CPU执行权，由于之前已经进行了第一次校验，结果为null（不会再次判断），获得锁后，直接创建实例。结果就会导致创建多个实例。所以需要在同步代码里面进行第二次校验，如果实例为空，则进行创建。

```java
public class Singleton{
    private static volatile Singleton instance;//懒汉式
    private Singleton(){};
    public static Singleton Getinstance(){//仅可以通过Getinstance创建单例对象
        if(instance==null){//首先校验对象是否为空，为空才进入加锁代码   --->第一次校验,避免
            synchronized(Singleton.class){//获取类锁
                if(instance==null){ // 第二次校验
                    instance=new Singleton();
                }
            }
        }
        return instance;
    }
}
```



## 单例模式的应用实例

【例1】用懒汉式单例模式模拟产生美国当今总统对象。

 分析：在每一届任期内，美国的总统只有一人，所以本实例适合用单例模式实现，图 2 所示是用懒汉式单例实现的结构图

```java
/*
 * 用懒汉式单例模式模拟产生美国当今总统对象。
 * */
public class President {
    private static volatile President instance = null;//保证instance在所有线程中同步

    //private避免类在外部被实例化
    private President() {
        System.out.println("产生一个总统！");
    }

    public static synchronized President getInstance() {//getInstance是静态方法，属于类
//        在getInstance方法上加上同步
        if (instance == null) {
            instance = new President();//单例对象由单例类自行创建，且单例类自有一个单例对象
        } else {
            System.out.println("已经有一个总统，不能产生新的总统！");
        }
        return instance;//单例类提供一个对外的全局访问点
    }

    public void getname() {
        System.out.println("我是美国总统，奥巴马！");
    }
}
```

```java
public class main {
    public static void main(String[] args) {
        President a=President.getInstance();//只有当调用getInstance方法会创建一个单例对象；getInstance是静态方法，属于类，所以可以直接通过类名直接调用。
        a.getname();
        President b=President.getInstance();
        b.getname();

        if (a==b){
            System.out.println("他们是同一个人！");
        }
        else {
            System.out.println("他们不是同一个人！");
        }
    }
}

//产生一个总统！
//我是美国总统，奥巴马！
//已经有一个总统，不能产生新的总统！
//我是美国总统，奥巴马！
//他们是同一个人！
```

# 建造者模式（Bulider模式）

建造者（Builder）模式的定义：指将一个复杂对象的构造与它的表示分离，使同样的构建过程可以创建不同的表示，这样的[设计模式](http://c.biancheng.net/design_pattern/)被称为建造者模式。它是将一个复杂的对象分解为多个简单的对象，然后一步一步构建而成。它将变与不变相分离，即<u>产品的组成部分是不变的，但每一部分是可以灵活选择的。</u>

生活中这样的例子很多，如汽车中的方向盘、发动机、车架、轮胎等部件多种多样；

以上所有这些产品都是由多个部件构成的，各个部件可以灵活选择，但其创建步骤都大同小异。（即组装汽车的组成部件是一样的，但是每个部件可以可以选择不同的款式。）

该模式的主要优点如下：

1. ​		封装性好，构建和表示分离。
2. ​		扩展性好，各个具体的建造者相互独立，有利于系统的解耦。
3. ​		客户端不必知道产品内部组成的细节，建造者可以对创建过程逐步细化，而不对其它模块产生任何影响，便于控制细节风险。


 其缺点如下：

1. ​		产品的组成部分必须相同，这限制了其使用范围。
2. ​		如果产品的内部变化复杂，如果产品内部发生变化，则建造者也要同步修改，后期维护成本较大。


 建造者（Builder）模式和工厂模式的关注点不同：建造者模式注重零部件的组装过程，而[工厂方法模式](http://c.biancheng.net/view/1348.html)更注重零部件的创建过程，但两者可以结合使用。



建造者（Builder）模式的主要角色如下。

1. ​		产品角色（Product）：它是包含多个组成部件的复杂对象，由具体建造者来创建其各个零部件。
2. ​		抽象建造者（Builder）：它是一个包含创建产品各个子部件的抽象方法的接口，通常还包含一个返回复杂产品的方法 getResult()。
3. ​		具体建造者(Concrete Builder）：实现 Builder 接口，完成复杂产品的各个部件的具体创建方法。
4. ​		指挥者（Director）：它调用建造者对象中的部件构造与装配方法完成复杂对象的创建，在指挥者中不涉及具体产品的信息。

```java
/*
* 产品角色：包含多个组成部件的复杂对象。
* */
public class Product {
    private String A;
    private String B;
    private String C;

    public void setA(String a){
        this.A=a;
    }

    public void setB(String b) {
        B = b;
    }

    public void setC(String c) {
        C = c;
    }
}
```

```java
/*
 * 抽象建造者：包含创建产品各个子部件的抽象方法。
 * */
public abstract class Builder {
    //    创建产品对象
    protected Product product = new Product();

    public abstract void buildA();

    public abstract void buildB();

    public abstract void buildC();

    public Product getProduct() {
        return product;
    }
}

```

```java
/*
* 具体建造者：实现了抽象建造者。
 * */
public class ConcreteBuilder extends Builder {

    @Override
    public void buildA() {
        product.setA("build A");
    }

    @Override
    public void buildB() {
        product.setA("build B");
    }

    @Override
    public void buildC() {
        product.setC("build C");
    }
}
```

```java
/*
* 指挥者：调用建造者中的方法完成复杂对象的创建。
 * */
public class Director {
    private Builder builder;

    public Director (Builder builder){
        this.builder=builder;
    }

    //产品构建与组装方法
    public Product construct(){
        builder.buildA();
        builder.buildB();
        builder.buildC();
        return builder.getProduct();
    }
}

```

```java
public class main {
    public static void main(String[] args) {
        Builder builder=new ConcreteBuilder();
        Director director=new Director(builder);
        Product product= director.construct();
        //do something with product
    }
}
```



# 原型模式

原型（Prototype）模式的定义如下：用一个已经创建的实例作为原型，通过复制该原型对象来创建一个和原型相同或相似的新对象。在这里，原型实例指定了要创建的对象的种类。

原型模式包含以下主要角色。

1. ​		抽象原型类：规定了具体原型对象必须实现的接口。
2. ​		具体原型类：实现抽象原型类的 clone() 方法，它是可被复制的对象。
3. ​		访问类：使用具体原型类中的 clone() 方法来复制新的对象。

原型模式的克隆分为浅克隆和深克隆。

- ​		浅克隆：创建一个新对象，新对象的属性和原来对象完全相同，对于非基本类型属性，<u>仍指向原有属性所指向的对象的内存地址</u>。
- ​		深克隆：创建一个新对象，属性中引用的其他对象也会被克隆，不再指向原有对象地址。

```java
//定义抽象原型类，
public abstract class  Prototype {
    abstract Prototype myclone();
}
```

```java
//定义具体原型类，实现抽象原型类中的clone方法
public class ConcreatePrototype extends Prototype {

    private String filed;

    public ConcreatePrototype (String filed){
        this.filed=filed;
    }

    @Override
    Prototype myclone() {
        return new ConcreatePrototype(filed);
    }

    @Override
    public String toString() {
        return "ConcreatePrototype{" +
                "filed='" + filed + '\'' +
                '}';
    }
}

```

```java
//主方法，使用具体原型类中的clone方法来复制新对象
public class main {
    public static void main(String[] args) {
        Prototype prototype=new ConcreatePrototype("abc");
        Prototype clone=prototype.myclone();
        System.out.println(clone.toString());
    }
}
```



Java 中的 Object 类提供了浅克隆的 clone() 方法，具体原型类只要实现 Cloneable 接口就可实现对象的浅克隆，这里的 Cloneable 接口就是抽象原型类。其代码如下：

```java
//具体原型类
class Realizetype implements Cloneable {
    Realizetype() {
        System.out.println("具体原型创建成功！");
    }

    public Object clone() throws CloneNotSupportedException {
        System.out.println("具体原型复制成功！");
        return (Realizetype) super.clone();
    }
}

//原型模式的测试类
public class PrototypeTest {
    public static void main(String[] args) throws CloneNotSupportedException {
        Realizetype obj1 = new Realizetype();
        Realizetype obj2 = (Realizetype) obj1.clone();
        System.out.println("obj1==obj2?" + (obj1 == obj2));
    }
}


```



## 原型模式的应用实例

【例】用原型模式生成“三好学生”奖状。

 分析：同一学校的“三好学生”奖状除了获奖人姓名不同，其他都相同，属于相似对象的复制，同样可以用原型模式创建，然后再做简单修改就可以了。奖状类是具体原型类，而 Java 中的 Cloneable 接口是抽象原型类。

```java
//具体原型类
public class Citation implements Cloneable {
    private String name;
    private String info;
    private String college;

    public Citation(String name,String info,String college){
        this.name=name;
        this.info=info;
        this.college=college;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void display(){
        System.out.println(name+info+college);
    }

    public Object clone() throws CloneNotSupportedException {
        System.out.println("奖状拷贝成功！");
        return  (Citation)super.clone();

    }
}
```

```java
public class ProtoTypeCitation {
    public static void main(String[] args) throws CloneNotSupportedException {
        Citation t1=new Citation("张三", "同学：在2016学年第一学期中表现优秀，被评为三好学生。", "韶关学院");
        t1.display();
        Citation t2=(Citation) t1.clone();
        t2.setName("李四");
        t2.display();
    }
}
/*
张三同学：在2016学年第一学期中表现优秀，被评为三好学生。韶关学院
奖状拷贝成功！
李四同学：在2016学年第一学期中表现优秀，被评为三好学生。韶关学院
*/
```

# 责任链模式

责任链（Chain of Responsibility）模式的定义：为了避免请求发送者与多个请求处理者耦合在一起，于是将所有请求的处理者通过前一对象记住其下一个对象的引用而连成一条链；当有请求发生时，可将请求沿着这条链传递，直到有对象处理它为止。

在责任链模式中，客户只需要将请求发送到责任链上即可，无须关心请求的处理细节和请求的传递过程，请求会自动进行传递。所以责任链将请求的发送者和请求的处理者解耦了。

职责链模式主要包含以下角色。

1. ​		抽象处理者（Handler）角色：定义一个处理请求的接口，包含抽象处理方法和一个后继连接。
2. ​		具体处理者（Concrete Handler）角色：实现抽象处理者的处理方法，判断能否处理本次请求，如果可以处理请求则处理，否则将该请   求转给它的后继者。
3. ​		客户类（Client）角色：创建处理链，并向**链头**的具体处理者对象提交请求，它不关心处理细节和请求的传递过程。

> 责任链模式的本质是解耦请求与处理，让请求在处理链中能进行传递与被处理；理解责任链模式应当理解其模式，而不是其具体实现。责任链模式的独到之处是将其节点处理者组合成了链式结构，并允许节点自身决定是否进行请求处理或转发，相当于让请求流动起来。

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210113210403.png)



> 可以采用链表的方式来实现责任链

```java
//抽象处理者
public abstract class Handler {
    private Handler next;

    public void setNext(Handler next){
        this.next=next;
    }

    public Handler getNext(){
        return next;
    }

    public abstract void handRequest(String s);
}
```

```java
//具体处理者1
public class ConcreateHandler1 extends Handler {

    @Override
    public void handRequest(String s) {
        if (s.equals("one")){
            System.out.println("1进行处理");
        }
        else {
            if (getNext()!=null){
                getNext().handRequest(s);
            }
            else {
                System.out.println("无法进行处理");
            }
        }
    }
}
```

```java
//具体处理者2
public class ConcreateHandler2 extends Handler {
    @Override
    public void handRequest(String s) {
        if (s.equals("two")){
            System.out.println("2进行处理");
        }
        else {
            if (getNext()!=null){
                getNext().handRequest(s);
            }
            else {
                System.out.println("无法进行处理");
            }
        }
    }
}
```

```java
public class main {
    public static void main(String[] args) {
//        组装责任链
        Handler handler1=new ConcreateHandler1();
        Handler handler2=new ConcreateHandler2();
        handler1.setNext(handler2);

//        提交请求
        handler1.handRequest("two");
    }
}
```



# 命令模式

如何将方法的请求者与实现者解耦？命令模式就能很好地解决这个问题。

在现实生活中，命令模式的例子也很多。比如看电视时，我们只需要轻轻一按遥控器就能完成频道的切换，这就是命令模式，将换台请求和换台处理完全解耦了。电视机遥控器（命令发送者）通过按钮（具体命令）来遥控电视机（命令接收者）。



命令（Command）模式的定义如下：将一个请求封装为一个对象，使发出请求的责任和执行请求的责任分割开。这样两者之间通过命令对象进行沟通，这样方便将命令对象进行储存、传递、调用、增加与管理。

命令模式包含以下主要角色。

1. ​		抽象命令类（Command）角色：声明执行命令的接口，拥有执行命令的抽象方法 execute()。
2. ​		具体命令类（Concrete Command）角色：是抽象命令类的具体实现类，它拥有接收者对象，并通过调用接收者的功能来完成命令要执行的操作。
3. ​		实现者/接收者（Receiver）角色：执行命令功能的相关操作，是具体命令对象业务的真正实现者。
4. ​		调用者/请求者（Invoker）角色：是请求的发送者，它通常拥有很多的命令对象，并通过访问命令对象来执行相关请求，它不直接访问接收者。

```java
//命令调用者 -->调用命令
public class Invoker {
    private Command command;

    public Invoker(Command command) {
        this.command = command;
    }

    public void setCommand(Command command) {
        this.command = command;
    }

    public void call() {
        System.out.println("调用者执行命令command");
        command.execute();
    }
}
```

```java
//抽象命令
public interface Command {
    void execute();
}
```

```java
//具体命令--->命令的接收者
public class ConcreteCommand implements Command {
    private Receiver receiver;

    public ConcreteCommand() {
        receiver = new Receiver();
    }

    @Override
    public void execute() {
        receiver.action();
    }
}
```

```java
//命令的接受者
public class Receiver {
    public void action(){
        System.out.println("接受者的action方法被调用");
    }
}
```

```java
public class main {
    public static void main(String[] args) {
        Command cmd=new ConcreteCommand();//具体命令
        Invoker invoker=new Invoker(cmd);//调用者
        System.out.println("客户访问调用者的call方法");
        invoker.call();

    }
}
/*
客户访问调用者的call方法
调用者执行命令command
接受者的action方法被调用
*/
```





![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210114154133.png)

【例1】用命令模式实现客户去餐馆吃早餐的实例。

 分析：客户去餐馆可选择的早餐有肠粉、河粉和馄饨等，客户可向服务员选择以上早餐中的若干种，服务员将客户的请求交给相关的厨师去做。这里的点早餐相当于“命令”，服务员相当于“调用者”，厨师相当于“接收者”，所以用命令模式实现比较合适。

- ​		首先，定义一个早餐类（Breakfast），它是抽象命令类，有抽象方法 cooking()，说明要做什么；
- ​		再定义其子类肠粉类（ChangFen）、馄饨类（HunTun）和河粉类（HeFen），它们是具体命令类，实现早餐类的 cooking() 方法，但它们不会具体做，而是交给具体的厨师去做；
- ​		具体厨师类有肠粉厨师（ChangFenChef）、馄饨厨师（HunTunChef）和河粉厨师（HeFenChef），他们是命令的接收者。







# 参考

​	[参考1](https://github.com/CyC2018/CS-Notes/blob/master/notes/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%20-%20%E7%9B%AE%E5%BD%95.md)

​	[参考2](https://refactoringguru.cn/design-patterns#intro-patterns)

​	[参考3](http://c.biancheng.net/design_pattern/)

​	大话设计模式
