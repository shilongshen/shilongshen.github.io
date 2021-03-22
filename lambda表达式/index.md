# lambda表达式


### 为什么引入lambda表达式

在Java中是面向对象进行编程，如果你想要调用一个方法，你必须创建一个类，并在类中构造一个方法，方法体中为实现的方法。当你想要调用这个方法时，你必须将类进行实例化才能够调用这个方法。从某种意义上来说，这是比较复杂的，lambda就是为了简化这一过程。

### lambda表达式的语法

- lambda表达式的形式为`参数，箭头（->），以及一个表达式`。

- 如果代码要完成的计算无法放在一个表达式中，可以将放在`{}`中

- `{}`中可以显式包含return语句：

  ```java
  （String first,Sring second）->{
      if (first.length>second.length) return -1;
      else return 0;
  }
  ```

- 即使lambda表达式中没有参数，任要提供空括号，就像空参数一样：

  ```java
  ()->{
      for (int i=0;i<100;i++)
          System,out.println(i);
  }
  ```

- 如果可以推导出一个参数的类型，则可以忽略其类型：

  ```java
  Comparator<String> Com=(first,second)->{
      first.length-second.length;
  }
  ```

- 无需指定lambda表达式的返回类型。lambda表达式的返回类型总是能够从上下文推断得到：

  ```java
  （String first,Sring second）->  first.length>second.length //返回类型为int
  ```

  ### 函数式接口

  定义：只具有一个抽象方法的接口称为函数式接口

  这里需要注意的一个问题是，我们可能认为接口中的方法可能都是抽象方法，其实不然，因为接口中可以声明非抽象方法，例如默认方法，静态方法，私有方法。判断一个接口是否为函数为：对于Java自带的标准库里的大量单一方法的接口，很多都已经标记为`@FunctionalInterface`，表明该接口可以作为函数使用。

  例如Comparator接口只具有一个抽象抽象方法compare

```java
//使用匿名内部类的方式
Arrays.sort(employees, new Comparator<Employee>() {
            @Override
            public int compare(Employee o1, Employee o2) {
                return Integer.compare(o1.getSalary(), o2.getSalary());
            }
        });//对数组进行排序，此时传入的参数为对象数组以及比较器。


//使用lambda表达式
 Arrays.sort
                (employee1s, (
                                (o1, o2) -> {
                                    return Integer.compare(o1.getSalary(), o2.getSalary());
                                }
                        )
                );//对数组进行排序，此时传入的参数为对象数组以及比较器。

```

### 方法引用

如果函数式接口的实现恰好可以通过调用**一个**方法来实现，那么我们可以使用方法引用

方法引用分为

- 静态方法方法引用

- 非静态方法引用

- 构造函数的方法引用

  ```java
  
  public class Demo {
      public static void main(String[] args) {
          // 静态方法引用--通过类名调用
          Consumer<String> consumerStatic = Java3y::MyNameStatic;
          consumerStatic.accept("3y---static");
  
          //实例方法引用--通过实例调用
          Java3y java3y = new Java3y();
          Consumer<String> consumer = java3y::myName;
          consumer.accept("3y---instance");
  
          // 构造方法方法引用--无参数
          Supplier<Java3y> supplier = Java3y::new;//方法名为new
          System.out.println(supplier.get());
      }
  }
  
  class Java3y {
      // 静态方法
      public static void MyNameStatic(String name) {
          System.out.println(name);
      }
  
      // 实例方法
      public void myName(String name) {
          System.out.println(name);
      }
  
      // 无参构造方法
      public Java3y() {
      }
  }
  ```

  














