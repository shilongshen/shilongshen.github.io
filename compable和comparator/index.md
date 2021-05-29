# Comparable和Comparator


在Arrays类中的sort方法能够对对象数组进行排序，但是我们知道，排序要有排序规则，如果是自定义的类，必须明确这些规则才能够使用Arrays.sort方法对对象数组进行排序。实际上这些排序规则就是一种比较规则。

对于基本数据类型,例如int等，或者是常用的对象,例如String ,其本身就定义了比较规则，因此我们不需要在重新定义比较规则。但是对于我们自己定义的类就必须重新定义比较规则。

在Java中实现Arrays.sort数组排序有两种方式：

- 实现Comparable接口，并覆写compareTo方法。在调用Arrays.sort只需要传入对象数组即可
- 在调用Arrays.sort时需要传入一个数组和一个比较器作为参数。比较器是实现了Comparator接口中compare方法的类的的实例。

---

### 首先来看第一种方式

类实现Comparable类中的compareTo方法，Arrays.sort传入对象数组

```java
public class ArraysTest {
    public static void main(String[] args) {
        Employee[] employees=new Employee[3];//构建一个对象数组
        employees[0]=new Employee(10000,"xiaoming");
        employees[1]=new Employee(20000,"xiaohong");
        employees[2]=new Employee(15000,"xiaocai");

        Arrays.sort(employees);//对数组进行排序，由于此时类已经实现了Comparable接口中的comparaTo方法,所以自需要传入对象数组即可
        
        for (Employee s:employees){
            System.out.println(s.getSalary());
        }
        /*
        输出结果：
        10000
		15000
		20000
        */
    }
}


class Employee implements Comparable<Employee>{
    private int salary;
    private String name;
    public Employee(int salary,String name){
        this.salary=salary;
        this.name=name;
    }
    public int getSalary(){
        return salary;
    }


    @Override
    public int compareTo(Employee o) {
        return Integer.compare(salary,o.salary);//按照薪水进行比较
    }
}

```

### 第二种方式

在调用Arrays.sort时需要传入一个数组和一个比较器作为参数。比较器是实现了Comparator接口中compare方法的类的的实例。

这听起来有一些拗口，来看一看代码

```java
public class ArraysTest {
    public static void main(String[] args) {
        Employee[] employees=new Employee[3];//构建一个对象数组
        employees[0]=new Employee(10000,"xiaoming");
        employees[1]=new Employee(20000,"xiaohong");
        employees[2]=new Employee(15000,"xiaocai");

        Arrays.sort(employees,new com());//对数组进行排序，此时传入的参数为对象数组以及比较器。
        
        for (Employee s:employees){
            System.out.println(s.getSalary());
        }
        /*
        输出结果：
        10000
		15000
		20000
        */
    }
}


class Employee {
    private int salary;
    private String name;
    public Employee(int salary,String name){
        this.salary=salary;
        this.name=name;
    }
    public int getSalary(){
        return salary;
    }

    
class com implements Comparator<Employee1>{//实现Comparator接口并覆写compare方法
    @Override
    public int compare(Employee1 o1, Employee1 o2) {
        return Integer.compare(o1.getSalary(), o2.getSalary());
    }
}
    
```

从代码中可以看出，实际上比较器就是实现了Comparator接口的实现类的一个对象。这一个对象也可以采用匿名内部类的方式来表示：

```java
public class ArraysTest {
    public static void main(String[] args) {
        Employee[] employees=new Employee[3];//构建一个对象数组
        employees[0]=new Employee(10000,"xiaoming");
        employees[1]=new Employee(20000,"xiaohong");
        employees[2]=new Employee(15000,"xiaocai");

        Arrays.sort(employees, new Comparator<Employee>() {
            @Override
            public int compare(Employee o1, Employee o2) {
                return Integer.compare(o1.getSalary(), o2.getSalary());
            }
        });//对数组进行排序，此时传入的参数为对象数组以及比较器。
        
        for (Employee s:employees){
            System.out.println(s.getSalary());
        }
        /*
        输出结果：
        10000
		15000
		20000
        */
    }
}


class Employee {
    private int salary;
    private String name;
    public Employee(int salary,String name){
        this.salary=salary;
        this.name=name;
    }
    public int getSalary(){
        return salary;
    }


    
```

或者采用lambda表达式：

```java
public class ArraysTest {
    public static void main(String[] args) {
        Employee[] employees=new Employee[3];//构建一个对象数组
        employees[0]=new Employee(10000,"xiaoming");
        employees[1]=new Employee(20000,"xiaohong");
        employees[2]=new Employee(15000,"xiaocai");
 		
        Arrays.sort
                (employee1s, (
                                (o1, o2) -> {
                                    return Integer.compare(o1.getSalary(), o2.getSalary());
                                }
                        )
                );//对数组进行排序，此时传入的参数为对象数组以及比较器。
        
        for (Employee s:employees){
            System.out.println(s.getSalary());
        }
        /*
        输出结果：
        10000
		15000
		20000
        */
    }
}


class Employee {
    private int salary;
    private String name;
    public Employee(int salary,String name){
        this.salary=salary;
        this.name=name;
    }
    public int getSalary(){
        return salary;
    }


    
```



实际上，不仅仅是Arrays.sort方法要求传入的对象数组必须实现Comparable接口或者Arrays.sort需要传入实现Comparator接口的比较器,对于TreeSet和TreeMap中的key部分也需要实现比较规则，因为TreeSet和TreeMap的底层结果为自平衡二叉树，其特点是可排序。

来看代码

### 1.TreeSet中的元素或TreeMap中的key部分需要实现Comparable接口

```java
public class TreeMapTest03 {
    public static void main(String[] args) {
        User u1=new User(25);
//        System.out.println(u1);
        User u2=new User(23);
        User u3=new User(30);
        User u4=new User(27);

        TreeSet<User> ts=new TreeSet<>();
        ts.add(u1);
        ts.add(u2);
        ts.add(u3);
        ts.add(u4);

        for (User it:ts){
            System.out.println(it);
        }
    }
}

class User implements Comparable<User> {//实现Comparable接口
    private int age;

    public User(int age){
        this.age=age;
    }

    public String toString(){
        return "age= "+age;
    }

    @Override
    public int compareTo(User o) {//重写CompareTo方法，o1.compareTo(o2)
        //其中this表示o1
        //o表示o2
        // o1和o2比较时就是this和o比较

//        int age1=this.age;
//        int age2=o.age;
//        if (age1==age2){
//            return 0;
//        }else if (age1>age2){
//            return 1;
//        }else{
//            return -1;
//        }


        return this.age-o.age;

        /*
        * 根据返回的正负数就可以进行排序
        * 为什么根据返回的正负数就可以进行排序？
        * 这是因为二叉树的数据结构（TreeSet的底层数据结构为二叉树）
        *
        * */
    }
}
```



### 2.给构造方法传入一个比较器

```java
public class TreeMapTest05 {
    public static void main(String[] args) {
        //给构造方法传递一个比较器
        TreeSet<wugui> wuguis =new TreeSet<>(new wuguicompare());

        //或者使用匿名内部类：这个类没有名字，直接new接口
//        TreeSet<wugui> wuguis=new TreeSet<>(new Comparator<wugui>() {
//            @Override
//            public int compare(wugui o1, wugui o2) {
//                return o1.age-o2.age;
//            }
//        });


        wugui w1=new wugui(500);
        wugui w2=new wugui(200);
        wugui w3=new wugui(700);

        wuguis.add(w1);
        wuguis.add(w2);
        wuguis.add(w3);

        for (wugui it:wuguis){
            System.out.println(it);
        }

    }
}

class wugui{
    public int age;
    public wugui(int age){
        this.age=age;
    }

    @Override
    public String toString() {
        return "wugui{" +
                "age=" + age +
                '}';
    }
}

class wuguicompare implements Comparator<wugui>{

    @Override
    //自定义比较规则
    public int compare(wugui o1, wugui o2) {
        return o1.age-o2.age;
    }
}
```

```
结论：
*   放到TreeSet或者TrssMap集合中的元素要想实现排序，包括两种方式：
*       1.放在集合中的元素要实现java.lang.Comparable接口（重写CompareTo方法）
*       2.在构造TreeSet或TreeMap集合的时候给它传一个比较器对象（实现Comparator接口）。
*
* Comparable和Comparator如何选择？
*   当比较规则不发生改变时，或者说比较规则只有一个时，建议选择Comparable
*   如果比较规则有多个或者比较规则之间需要频繁切换，建议选择Comparator
```



### Collections.sort

Collections.sort能够给List<T> list进行排序，其中集合需要自定义比较规则，也是一样的两种方式  -->Arrays.sort是对数组进行排序

- 实现Comparable接口，并覆写compareTo方法。在调用Collections.sort只需要传入集合即可
- 在调用Collections.sort时需要传入一个数组和一个比较器作为参数。比较器是实现了Comparator接口中compare方法的类的的实例。



### HashMap按照key或value进行排序

[参考](https://blog.csdn.net/xHibiki/article/details/82938480?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-2.control&dist_request_id=1328761.10591.16172379784854221&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-2.control)

```java
        Map phone=new HashMap();
        phone.put("Apple",7299);
        phone.put("SAMSUNG",6000);
        phone.put("Meizu",2698);
        phone.put("Xiaomi",2400);
        System.out.println(phone);
/*
{Meizu=2698, Apple=7299, Xiaomi=2400, SAMSUNG=6000}
*/
```

#### 按照key进行排序：

对key进行排序,首先要得到HashMap中键的集合(keySet),并转换为数组,这样才能用Arrays.sort()进行排序

```java
        Set set=phone.keySet();//得到key集合
        Object[] arr=set.toArray();//将key集合转换为数组
        Arrays.sort(arr);//对数组进行排序
        for(Object key:arr){
            System.out.println(key);
        }


        for(Object key:arr){
            System.out.println(key+": "+phone.get(key));
        }


```

#### 按照value进行排序

将entrySet转换为List,然后重写比较器比较即可.这里可以使用List.sort(comparator),也可以使用Collections.sort(list,comparato

```java
//将将entrySet转换为List
List<Map.Entry<String, Integer>> list = new ArrayList<Map.Entry<String, Integer>>(phone.entrySet()); //转换为list

//重写比较器，使用list.sort()排序
       list.sort(new Comparator<Map.Entry<String, Integer>>() {
          @Override
          public int compare(Map.Entry<String, Integer> o1, Map.Entry<String, Integer> o2) {
              return o2.getValue().compareTo(o1.getValue());//对value进行排序
          }
      });

//或者使用Collections.sort()排序
        Collections.sort(list, new Comparator<Map.Entry<String, Integer>>() {
           @Override
           public int compare(Map.Entry<String, Integer> o1, Map.Entry<String, Integer> o2) {
               return o2.getValue().compareTo(o1.getValue());
           }
       });



//for-each循环
           for (Map.Entry<String, Integer> mapping : list){
            System.out.println(mapping.getKey()+": "+mapping.getValue());
        }   

```










