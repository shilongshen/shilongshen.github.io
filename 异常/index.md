# 异常


### 异常分类

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201125193625.png)



所有的异常都派生于Throwable,在下一层又分解为Error和Exception。

Error类层次描述Java运行时系统的内部错误和资源耗尽错误，发生这类错误时，除了通知错误并尽力妥善终止程序外，你几乎无能为力，这一类错误很少发生。

Exception类又分为Runtime Exception和其他Exception。



Java将异常进行分类：

- 非检查型异常（unchecked）：派生于Error类和Runtime Exception类的所有异常
- 检查型异常（checked）:其他所有异常

**需要为所有检查型异常提供异常处理器**。

### 声明检查型异常

一个方法**必须**声明所有可能抛出的检查型异常，如果没有声明，编译器就会发出一个错误警告

声明的方法：在方法首部通过`throws` 声明这个方法可能抛出的检查型异常，例如

```java
class Test{
    
    public Image LoadImage(String s) throws IOException{
        ...
    }
    
}
```

这里需要注意的是：

- 如果子类覆写父类的方法，那么子类的这个方法声明的异常不能比父类更加通用。子类中的方法可以声明更加特殊的异常或不声明异常
- 如果父类中的方法没有声明任何异常，那么子类的方法也不能声明异常

### 如何抛出异常

例：

```java
String readData(Scanner in) throws EOFException{
    
    while(){
        if(!in.hasNext()){
            if(n<len){
                throw new EOFException;//抛出异常
            }
        }    
    }
    return s;
}
```

抛出异常的方法：

- 找到一个合适的异常类
- 创建这个类的一个对象
- 将对象抛出

一旦方法抛出了异常，这个方法就不会返回到调用者。

或者是通过`throws`进行抛出，具体的用法和声明异常一样，即在方法首部通过`throws` 抛出异常

`throw` 和`throws`的区别：[参考](https://blog.csdn.net/hhy62011980/article/details/5548278?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-2.not_use_machine_learn_pai&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-2.not_use_machine_learn_pai)

```
throw语句用在方法体内，表示抛出异常，由方法体内的语句处理。
throws语句用在方法声明后面，表示再抛出异常，由该方法的调用者来处理。

 

throws主要是声明这个方法会抛出这种类型的异常，使它的调用者知道要捕获这个异常。
throw是具体向外抛异常的动作，所以它是抛出一个异常实例。

 

throws说明你有那个可能，倾向。
throw的话，那就是你把那个倾向变成真实的了。

 

同时：
1、throws出现在方法函数头；而throw出现在函数体。
2、throws表示出现异常的一种可能性，并不一定会发生这些异常；throw则是抛出了异常，执行throw则一定抛出了某种异常。
3、两者都是消极处理异常的方式（这里的消极并不是说这种方式不好），只是抛出或者可能抛出异常，但是不会由函数去处理异常，真正的处理异常由函数的上层调用处理。
```

例：

```java
void doA(int a) throws Exception1,Exception3{
           try{
                 ......

           }catch(Exception1 e){
              throw e;
           }catch(Exception2 e){
              System.out.println("出错了！");
           }
           if(a!=b)
              throw new  Exception3("自定义异常");
}
```

代码块中可能会产生3个异常，(Exception1,Exception2,Exception3)。
如果产生Exception1异常，则捕获之后再抛出，由该方法的调用者去处理。
如果产生Exception2异常，则该方法自己处理了（即System.out.println("出错了！");）。所以该方法就不会再向外抛出Exception2异常了，void doA() throws Exception1,Exception3 里面的Exception2也就不用写了。
而Exception3异常是该方法的某段逻辑出错，程序员自己做了处理，在该段逻辑错误的情况下抛出异常Exception3，则该方法的调用者也要处理此异常。





### 捕获异常

如果只是声明异常或者是抛出异常而没有捕获异常，当发生异常时，**程序就会终止**，并在控制台上打印出一个消息。

如果捕获了异常程序不会立即终止，而是会对异常进行相应的处理

这里需要注意的是

- 检查型异常必须声明，而非检查型异常可以不用声明（也可以声明，但是一般是不用声明的）
- 检查型异常和非检查型异常都可以被捕获（但是一般都是捕获检查型异常）
- 实际上我们非检查型异常的处理是：对于Error异常，描述Java运行时系统的内部错误和资源耗尽错误，对于这类错误是无能为力的；对于Runtime Exception 异常，是**运行时的异常**，例如数组越界等，这是我们在编写程序时就应该避免发生的。而对于检查型异常，例如一个文件是否存在，我们就需要对其进行声明或捕获或抛出

捕获异常的语句块：

```java
try{
    code
}
catch(ExceptionType e){
    handle for this Type  
}
finally{
    must do 
}
```

执行逻辑

1. 如果try中某一语句抛出了catch子句指定的异常类，那么
2. 程序将跳过try语句中的其余代码
3. 程序执行catch字句中的处理代码
4. 如果try中语句没有抛出任何异常，那么程序就会跳过catch语句
5. 如果try中某一语句抛出了catch子句中没有声明的异常类，方法就会结束（应该尽量避免出现这种情况）
6. 无论前面是什么情况，finally语句中的代码一定会执行



例子：[参考](https://blog.csdn.net/Alexwym/article/details/81239692?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-3.not_use_machine_learn_pai&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-3.not_use_machine_learn_pai#commentBox)

`throw`,`throws`和`try...catch`

1、throws出现在方法函数头；而throw出现在函数体。
 2、throws表示出现异常的一种可能性，并不一定会发生这些异常；throw则是抛出了异常，执行throw则一定抛出了某种异常。
 3、两者都是消极处理异常的方式（这里的消极并不是说这种方式不好），只是抛出或者可能抛出异常，但是不会由函数去处理异常，真正的处理异常由函数的上层调用处理。

也就是异常处理是会一层层往上抛的，直到遇到了某个方法(try...catch)处理了这个异常或者最后抛给了JVM.

```java
package abnormalTest;
import java.io.IOException;
 
//定义一个测试类，检查JAVA中的异常处理机制
public class Test {
	int age;
	
	public void Abnormal() throws IOException{
		int i=0;
		int x=5/i;
		System.out.println(x);
	}
	
	//主函数入口
	public static void main(String[] args) throws IOException {
		Test t=new Test();
		t.Abnormal();
	}
	
}
```

分析：我们这里直接使用了JAVA中的IOException对象，由于我们在main函数中没有对这个异常进行处理，所以我们要给main函数加上throws  IOException，指明我不想处理这个异常，请帮我把它抛给上一级。**于是这个异常就被抛给了JAVA虚拟机**，JAVA虚拟机根据IOException所带的异常信息，判断这是一个整数除以0的异常，于是终止程序，并且打印出"/ by zero"的报错信息。

如果我们要对上一级方法中抛出来的异常进行处理，那么必须用到try...catch的结构。测试样例如下：

```java
package abnormalTest;
import java.io.IOException;
 
//定义一个测试类，检查JAVA中的异常处理机制
public class Test {
	int age;
	public void Abnormal() throws IOException {
		int i=0;
		if(i==0) {
			throw new IOException("除以0错误");
		}
		int x=5/i;
		System.out.println(x);
	}
	
	//主函数入口
	public static void main(String[] args) {
		try {
			Test t=new Test();
			t.Abnormal();
		}catch(IOException e){
			System.out.println("出现了IOException异常");
		}catch(NullPointerException e) {
			System.out.println("出现了空指针异常");
		}
	}
}
```

运行结果如下。这里打印出的是catch中的异常处理信息“出现了IOException”，而没有打印出"除以0错误"，说明这个异常在main函数中处理完就终止了，没有继续往上抛给JVM，这和我们前面的分析是一致的。然后我们这里定义两个catch方法分别来处理IOException和NullPointerException两种不同的异常

![img](https://img-blog.csdn.net/20180727191902283?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FsZXh3eW0=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)



`throw/throws把异常一层层地往上抛，直到有人去处理它。而try...catch就是那个劳苦工人，负责获取相应的异常并对它进行处理。`





相关问题：

有关finally语句块说法正确的是（A B C ）

```
A 不管catch是否捕获异常，finally语句块都是要被执行的
B 在try语句块或catch语句块中执行到System.exit(0)直接退出程序
C finally块中的return语句会覆盖try块中的return返回
D finally 语句块在 catch语句块中的return语句之前执行
```



```
结论：
1、不管有木有出现异常，finally块中代码都会执行；
2、当try和catch中有return时，finally仍然会执行；
3、finally是在return后面的表达式运算后执行的（此时并没有返回运算后的值，而是先把要返回的值保存起来，管finally中的代码怎么样，返回的值都不会改变，任然是之前保存的值），所以函数返回值是在finally执行前确定的；
4、finally中最好不要包含return，否则程序会提前退出，返回值不是try或catch中保存的返回值。


举例：


情况1：try{} catch(){}finally{} return;
显然程序按顺序执行。


情况2:try{ return; }catch(){} finally{} return;
程序执行try块中return之前（包括return语句中的表达式运算）代码；
再执行finally块，最后执行try中return;
finally块之后的语句return，因为程序在try中已经return所以不再执行。


情况3:try{ } catch(){return;} finally{} return;
程序先执行try，如果遇到异常执行catch块，
有异常：则执行catch中return之前（包括return语句中的表达式运算）代码，再执行finally语句中全部代码，
最后执行catch块中return. finally之后也就是4处的代码不再执行。
无异常：执行完try再finally再return.


情况4:try{ return; }catch(){} finally{return;}
程序执行try块中return之前（包括return语句中的表达式运算）代码；
再执行finally块，因为finally块中有return所以提前退出。


情况5:try{} catch(){return;}finally{return;}
程序执行catch块中return之前（包括return语句中的表达式运算）代码；
再执行finally块，因为finally块中有return所以提前退出。


情况6:try{ return;}catch(){return;} finally{return;}
程序执行try块中return之前（包括return语句中的表达式运算）代码；
有异常：执行catch块中return之前（包括return语句中的表达式运算）代码；
则再执行finally块，因为finally块中有return所以提前退出。
无异常：则再执行finally块，因为finally块中有return所以提前退出。

最终结论：任何执行try 或者catch中的return语句之前，都会先执行finally语句，如果finally存在的话。
如果finally中有return语句，那么程序就return了，所以finally中的return是一定会被return的，
编译器把finally中的return实现为一个warning。
```




