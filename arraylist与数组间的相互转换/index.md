# ArrayList与数组之间的相互转换




### 将ArrayList转换为数组

#### 方法1：利用循环语句将ArrayList中的元素添加到数组中

```java
public class ArrayListTest {
    public static void main(String[] args) {
        List<Integer> list =new ArrayList();
        list.add(1);
        list.add(2);
        list.add(3);
        list.add(4);

        int[] b=new int[list.size()];
        for (int i=0;i<list.size();i++){
            b[i]= list.get(i);
        }

        for (int s:b){
            System.out.println(s);
        }
    }
}
```

#### 方法2：使用ArrayList中的`toArray`方法，将Arraylist转换为数组

```java
public class ArrayListTest {
    public static void main(String[] args) {
        List<Integer> list =new ArrayList();
        list.add(1);
        list.add(2);
        list.add(3);
        list.add(4);

        Integer[] a=new Integer[list.size()];//注意这里的数组类型为Integer,引用类型，而不是int，基本类型
        list.toArray(a);

       for (Integer s:a){
          System.out.println(s);
      }    
    }
}
```

这里需要注意的是ArrayList中的toArray方法，有两个重载方法

```
1.public Object[] toArray()
2.public <T> T[] toArray(T[] a)
```

对于`public Object[] toArray()`返回的是一个Object数组

对于`public <T> T[] toArray(T[] a)`能将list转化为你所需要类型的数组，当然我们用的时候会转化为与list内容相同的类型



对于第一种方法存在的局限性是，该方法只能够返回Object数组，而不能返回你所需要类型的数组，例如

```java
        List<Integer> list =new ArrayList();
        list.add(1);
        list.add(2);
        list.add(3);
        list.add(4);

        Integer[] s=list.toArray();

        for (Integer s1:s){
            System.out.println(s);
        }
```

会报错：

```
java: 不兼容的类型: java.lang.Object[]无法转换为java.lang.Integer[]//向下转型失败
表明只能够返回Object类型数组
```

第二种方法就比较方便，可以转换为我们想要的类型

```java
 List<Integer> list =new ArrayList();
        list.add(1);
        list.add(2);
        list.add(3);
        list.add(4);

        Integer[] a=new Integer[list.size()];//注意这里的数组类型为Integer,引用类型，而不是int，基本类型
        list.toArray(a);//会自动推断传入参数的类型

       for (Integer s:a){
          System.out.println(s);
      }    
```

但是如果是这样写

```java
 List<Integer> list =new ArrayList();
        list.add(1);
        list.add(2);
        list.add(3);
        list.add(4);

        int[] a=new int[list.size()];
        list.toArray(a);

       for (Integer s:a){
          System.out.println(s);
      }   
```

会报错：

```
java: 对于toArray(int[]), 找不到合适的方法
    方法 java.util.Collection.<T>toArray(java.util.function.IntFunction<T[]>)不适用
      (无法推断类型变量 T
        (参数不匹配; int[]无法转换为java.util.function.IntFunction<T[]>))
    方法 java.util.List.<T>toArray(T[])不适用
      (推论变量 T 具有不兼容的上限
        等式约束条件：int
        下限：java.lang.Object)
```

可以看出此时编译器无法推断出传入toArray中参数的类型，似乎传入的参数必须是引用类型（必须继承Object）



### 将数组转换为ArrayList

#### 使用Arrays.asList

```java
public class ArrayListTest02 {
    public static void main(String[] args) {
        String[] s={"1","2","3"};
        List<String> list=new ArrayList<>();
        list= Arrays.asList(s);
        for (String a:list){
            System.out.println(a);
        }
    }

}
```

不过通过`list= Arrays.asList(s);`直接赋值存在的一个缺点是list无法添加或删除元素，例如

```java
 		String[] s={"1","2","3"};
        List<String> list=new ArrayList<>();
        list= Arrays.asList(s);
        for (String a:list){
            System.out.println(a);
        }
        list.add("lll");//添加元素
 
```

会报错

```
Exception in thread "main" java.lang.UnsupportedOperationException
	at java.base/java.util.AbstractList.add(AbstractList.java:153)
	at java.base/java.util.AbstractList.add(AbstractList.java:111)
	at CollectionTest.ArrayListTest02.main(ArrayListTest02.java:22)
```

此时可以通过ArrayList的构造方法

```
public ArrayList(Collection<? extends E> c)
构造一个包含指定 collection 的元素的列表，这些元素是按照该 collection 的迭代器返回它们的顺序排列的。
```

进行转换，如

```java
 public class ArrayListTest02 {
    public static void main(String[] args) {
        String[] s={"1","2","3"};
        List<String> list1=new ArrayList<>(Arrays.asList(s));
        list1.add("lll");
        for (String s1:list1){
            System.out.println(s1);
        }
    }

}

```

此时可以添加/删除元素。




