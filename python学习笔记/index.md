# python学习笔记




# map 函数

~~~python
#map  函数 接收一个函数和一个序列
>>> def f(x):
...     return x * x
...
>>> r = map(f, [1, 2, 3, 4, 5, 6, 7, 8, 9])
>>> list(r)
[1, 4, 9, 16, 25, 36, 49, 64, 81]
~~~

# filter 函数

~~~python
#filter 函数
#filter()把传入的函数依次作用于每个元素，然后根据返回值是True还是False决定保留还是丢弃该元素。

~~~

# 匿名函数

~~~python
#匿名函数
lambda x:x*x
#等价于
def f(x):
    return x*x
#关键字lambda表示匿名函数，冒号前面的x表示函数参数。
#匿名函数有个限制，就是只能有一个表达式，不用写return，返回值就是该表达式的结果。
~~~

#  函数的装饰器

~~~python
#首先需要明确函数可以作为参数传递给其他函数
#函数也可以作为其他函数的返回值
#装饰器的作用就是为已经存在的对象添加额外的功能。
#简单的装饰器使用
def use_logging(func):
    def wrapper():
        print('nihao')
        return func()   # 把 foo 当做参数传递进来时，执行func()就相当于执行foo()
    return wrapper

def foo():
    print('i am foo')

foo = use_logging(foo)  # 因为装饰器 use_logging(foo) 返回的时函数对象 wrapper，这条语句相当于  foo = wrapper
foo()                   # 执行foo()就相当于执行 wrapper()

>>nihao
i am foo

~~~

~~~python
# @ 符号就是装饰器的语法糖，它放在函数开始定义的地方，这样就可以省略最后一步再次赋值的操作。
def use_logging(func):

    def wrapper():
        print('nihao')
        return func()
    return wrapper

@use_logging
def foo():
    print("i am foo")

foo()
>>nihao
i am foo
#如上所示，有了 @ ，我们就可以省去foo = use_logging(foo)这一句了，直接调用 foo() 即可得到想要的结果。
~~~

~~~python
#装饰器在 Python 使用如此方便都要归因于 Python 的函数能像普通的对象一样能作为参数传递给其他函数，可以被赋值给其他变量，可以作为返回值，可以被定义在另外一个函数内。

#可能有人问，如果我的业务逻辑函数 foo 需要参数怎么办？比如：
def foo(name):
    print("i am %s" % name)
   
我们可以在定义 wrapper 函数的时候指定参数：
def use_logging(func):
    def wrapper(*arg,**kw):
        print('nihao')
        return func(*arg,**kw)
    return wrapper

@use_logging
def foo(*arg,**kw):
    print("i am foo")

foo()

~~~

~~~python
#装饰器还有更大的灵活性，例如带参数的装饰器

def use_logging(level):
    def decorator(func):
        def wrapper(*args, **kwargs):
            if level == "warn":
                print('nihao')
            elif level == "info":
                print('china')
            return func(*args)
        return wrapper

    return decorator

@use_logging(level="warn")
def foo(name='foo'):
    print("i am %s" % name)

foo()
>>nihao
i am foo

#上面的 use_logging 是允许带参数的装饰器。它实际上是对原有装饰器的一个函数封装，并返回一个装饰器。我们可以将它理解为一个含有参数的闭包。当我 们使用@use_logging(level="warn")调用的时候，Python 能够发现这一层的封装，并把参数传递到装饰器的环境中。
@use_logging(level="warn") 等价于 @decorator
~~~

~~~python
#没错，装饰器不仅可以是函数，还可以是类，相比函数装饰器，类装饰器具有灵活度大、高内聚、封装性等优点。使用类装饰器主要依靠类的__call__方法，当使用 @ 形式将装饰器附加到函数上时，就会调用此方法。
class Foo(object):
    def __init__(self, func):
        self._func = func

    def __call__(self):
        print ('class decorator runing')
        self._func()
        print ('class decorator ending')

@Foo
def bar():
    print ('bar')

bar()
>>class decorator runing
bar
class decorator ending
~~~

~~~python
#使用装饰器极大地复用了代码，但是他有一个缺点就是原函数的元信息不见了，比如函数的docstring、__name__、参数列表，先看例子：
# 装饰器
def logged(func):
    def with_logging(*args, **kwargs):
        print (func.__name__)      # 输出 'with_logging'
        print (func.__doc__   )    # 输出 None
        return func(*args, **kwargs)
    return with_logging

# 函数
@logged
def f(x):
   """does some math"""
   return x + x * x

logged(f)
f.__name__
>>'with_logging'
print(f.__doc__)
>>None
#不难发现，函数 f 被with_logging取代了，当然它的docstring，__name__就是变成了with_logging函数的信息了
#wraps本身也是一个装饰器，它能把原函数的元信息拷贝到装饰器里面的 func 函数中，这使得装饰器里面的 func 函数也有和原函数 foo 一样的元信息了。
from functools import wraps
def logged(func):
    @wraps(func)
    def with_logging(*args, **kwargs):
        print (func.__name__)      # 输出 'f'
        print (func.__doc__ )      # 输出 'does some math'
        return func(*args, **kwargs)
    return with_logging

@logged
def f(x):
   """does some math"""
   return x + x * x
f(2)
>>f
does some math
6
~~~



# 函数的参数

~~~python
#函数的参数

#位置参数
def f(x):
    return x*x

#默认参数,默认参数必须指向不变对象
def power(x, n=2):
    s = 1
    while n > 0:
        n = n - 1
        s = s * x
    return s

#可变参数
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
##参数numbers接收到的是一个list
>>> nums = [1, 2, 3]
>>> calc(*nums)
14

#关键字参数
#关键字参数在函数内部自动组装为一个dict。
def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)

#可以只传入必选参数   
>>> person('Michael', 30)
name: Michael age: 30 other: {}
#也可以传入任意个参数
>>> person('Bob', 35, city='Beijing')
name: Bob age: 35 other: {'city': 'Beijing'}
>>> person('Adam', 45, gender='M', job='Engineer')
name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}
            
            
#命名关键字参数
#如果要限制关键字参数的名字，就可以用命名关键字参数，例如，只接收city和job作为关键字参数。这种方式定义的函数如下：
def person(name, age, *, city, job):
    print(name, age, city, job)
#和关键字参数**kw不同，命名关键字参数需要一个特殊分隔符*，*后面的参数被视为命名关键字参数。
>>> person('Jack', 24, city='Beijing', job='Engineer')
Jack 24 Beijing Engineer
#如果函数定义中已经有了一个可变参数，后面跟着的命名关键字参数就不再需要一个特殊分隔符*了：
def person(name, age, *args, city, job):
    print(name, age, args, city, job)
#命名关键字参数必须传入参数名，这和位置参数不同。如果没有传入参数名，调用将报错
>>> person('Jack', 24, 'Beijing', 'Engineer')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: person() takes 2 positional arguments but 4 were given
~~~

# 类的继承和多态

~~~python
class Animal(object):   #编写Animal类
    def run(self):
        print("Animal is running...")

class Dog(Animal):  #Dog类继承Amimal类，没有run方法
    pass

class Cat(Animal):  #Cat类继承Animal类，有自己的run方法
    def run(self):
        print('Cat is running...')
    pass

class Car(object):  #Car类不继承，有自己的run方法
    def run(self):
        print('Car is running...')

class Stone(object):  #Stone类不继承，也没有run方法
    pass

def run_twice(animal):
    animal.run()
    animal.run()

run_twice(Animal())
run_twice(Dog())
run_twice(Cat())
run_twice(Car())
run_twice(Stone())

#输出结果
Animal is running...
Animal is running...
Animal is running...
Animal is running...
Cat is running...
Cat is running...
Car is running...
Car is running...

AttributeError: 'Stone' object has no attribute 'run'
    
# run_twice(animal)其实就是一个接口函数，可以用来接收一个对象，不管什么类型的对象都能接受，并且只要这个对象有run()都可以输出。作者想表达的意思应该是多态，如果传进去的对象自身没有定义run()，就会调用基类的run()，如果子对象有run()，就会调用自身的run()，也就实现了多态。但是所说的鸭子什么的，也就是只要传进去的是一个对象，并且该对象有run()，就可以体现出run()的特性。
~~~

# 给实例绑定方法

~~~python
#
class Student(object):
    pass

>>> def set_age(self, age): # 定义一个函数作为实例方法
...     self.age = age
...
>>> from types import MethodType
>>> s.set_age = MethodType(set_age, s) # 给实例绑定一个方法，第一个参数是要绑定的方法，第二个参数是要绑定的对象，
>>> s.set_age(25) # 调用实例方法
>>> s.age # 测试结果
25

~~~

```python

import numpy as np
 
a = np.array([[1,2,3], [4,5,6],[7,8,9]])
b = a[1:3, 1:3]
c = a[1:3,[1,2]]
d = a[...,1:]  ->a[1:3,[1,2]]
e=a[:,:-1]
print(b)
print(c)
print(d)

[[5 6]
 [8 9]]
[[5 6]
 [8 9]]
[[2 3]
 [5 6]
 [8 9]]



e=a[:,:-1]
#array([[1, 2],
 #      [4, 5],
  #     [7, 8]])
```

```python
P_step=torch.randn(1,6,3,256,256)
P_step[:,:-1,...].shape  #->表示第二个维度取[0,6),即除了倒数第一个都取
#torch.Size([1, 5, 3, 256, 256])
```





# 使用@property

~~~python
#在实际使用中会产生一个严重的问题，__init__ 中定义的属性是可变的，换句话说，是使用一个系统的所有开发人员在知道属性名的情况下，可以进行随意的更改(尽管可能是在无意识的情况下)，但这很容易造成严重的后果。
class Rectangle(object):
  def __init__(self):
    self.width =10
    self.height=20
r=Rectangle()
print(r.width,r.height)
r.width=1.0#此时width属性被任意修改了
print(r.width,r.height)

#这是生产中很不情愿遇到的情况，这时候就考虑能不能将width属性设置为私有的，其他人不能随意更改的属性，如果想要更改只能依照我的方法来修改，@property就起到这种作用(类似于java中的private)　
class Rectangle(object):
  @property
  def width(self):
    #变量名不与方法名重复，改为true_width，下同
    return self.true_width
  
  @property
  def height(self):
    return self.true_height
s = Rectangle()

#此时如果对width和height进行修改的话会报错
r.height=120
>>
AttributeError: can't set attribute
#(@property使方法像属性一样调用，就像是一种特殊的属性)#

#同样为了解决对属性的操作，提供了封装方法的方式进行属性的修改
class Rectangle(object):
  @property
  def width(self):
    # 变量名不与方法名重复，改为true_width，下同
    return self.true_width
  @width.setter#允许对width进行修改
  def width(self, input_width):
    self.true_width = input_width
  @property
  def height(self):
    return self.true_height
  @height.setter
  #与property定义的方法名要一致
  def height(self, input_height):
    self.true_height = input_height
s = Rectangle()
# 与方法名一致
s.width = 1024
s.height = 768
print(s.width,s.height)

#@property的调用可以将属性设置为只读或可读可写
#@property广泛应用在类的定义中，可以让调用者写出简短的代码，同时保证对参数进行必要的检查，这样，程序运行时就减少了出错的可能性。
~~~

# numpy中的切片操作

~~~python
import numpy as np
a=np.arange(9).reshape(3,3)
a
Out[31]: 
array([[0, 1, 2],
       [3, 4, 5],
       [6, 7, 8]])

#读取第2行
a[1]
Out[32]: array([3, 4, 5])

#读取第2列
a[:,1]#注意要加一个逗号
Out[33]: array([1, 4, 7])

#读取第1列和第2列
a[:,[0,1]]
>>array([[0, 1],
       [3, 4],
       [6, 7]])

#三维矩阵索引
a=np.arange(27).reshape(3,3,3)
a
>>
array([[[ 0,  1,  2],
        [ 3,  4,  5],
        [ 6,  7,  8]],

       [[ 9, 10, 11],
        [12, 13, 14],
        [15, 16, 17]],

       [[18, 19, 20],
        [21, 22, 23],
        [24, 25, 26]]])

a[:,1,2]
>>array([ 5, 14, 23])
##索引规则为a[通道，行，列]
#当某一个元素用冒号‘ ： ’代替时，表示不对这个位置进行限制


~~~



~~~python
lower()
#将字符串中的大写转换为小写
title() 
#方法返回"标题化"的字符串,就是说所有单词都是以大写开始，其余字母均为小写(见 istitle())。
~~~

~~~python
#查看数据类型
type()
type(123)=int
isinstance()
isinstance(123, int)
>>True

#获取对象所有的属性和方法
dir()
#比如，获得一个str对象的所有属性和方法：
dir('ABD')
>>
['__add__',
 '__class__',
 '__contains__',
 '__delattr__',
 '__dir__',
 '__doc__',
 '__eq__',
 '__format__',
 '__ge__',
 '__getattribute__',
 '__getitem__',
 '__getnewargs__',
 '__gt__',
 '__hash__',
 '__init__',
 '__init_subclass__',
 '__iter__',
 '__le__',
 '__len__',
 '__lt__',
 '__mod__',
 '__mul__',
 '__ne__',
 '__new__',
 '__reduce__',
 '__reduce_ex__',
 '__repr__',
 '__rmod__',
 '__rmul__',
 '__setattr__',
 '__sizeof__',
 '__str__',
 .
 .
 .
 'zfill']
#查看内存地址
id()
#
~~~







# 列表（list）、元组（tuple）和字典（dict）

[参考](https://blog.csdn.net/qq_38358499/article/details/88651265)

## 元组

a. 概念：元组是一种**有序**集合

b. 元祖和列表的区别：

​	格式：元祖(),列表[]

​	列表中的元素可以增删，<u>元组中的元素不能进行修改</u>（不能够修改地址，但是能够修改内容）

```python
t=(1,2,3,[4,5,6])  #创建元组
l=[1,2,3,[4,5,6]]  #创建列表

l.pop()  #可以对列表进行删除
#此时的l为
#Out[21]: [1, 2, 3]
l.append(4)  #可以对列表增加元素
#此时的l为
#Out[23]: [1, 2, 3, 4]

#但是不能对元组增删元素，但可以修改内容
t[3][0]=0
#此时的t为
#Out[33]: (1, 2, 3, [0, 5, 6])

```



c. 元组和列表的相同点：

​	都是一种容器，可以同时存储不同类型的数据

d. 元组和列表的创建

```python
列表：list=[parm1,parm2,...]

元组：tuple=(parm1,parm2)

#注意：和列表类似，在元组中可以存储重复元素，同时存储不同类型的数据；
```

e. 元组的访问

​	获取值：语法如：元组名称[索引]

```python
#1.创建空元组
tuple1 = ()
 
#2.创建带有元素的元组
tuple2 = (10,20,30)
 
#1.获取元组值
tuple1 = (10,20,30,40,50)
print(tuple1[1])
#法一：获取元组中的最后一个值：从前向后获取
print(tuple1[4])
#法二：获取元组中的最后一个值：从后向前获取，-1表示最后一个元素
print(tuple1[-1])
 
#2.修改元素值:不支持元素的修改，指的是不能更改地址，可以修改内容，此处特殊情况
tuple2 = (1,3,43,5,[54,54,5])
print(tuple2)
list1 = tuple2[4]
list1[1] = 100
print(tuple2)
 
#3.删除元组
del tuple2
```

f. 元组的操作

​	f1. 元组截取【切片】：tuple1[2:4]  #截取2到4的元素，包前不包后，包头不包尾

​                						 tuple1[2:]   #截取索引为2以后的元素，包括元素2

​                						 tuple1[:4]   #截取从开头到4的元素，不包括元素4

​	f2. 获取元组中元素的个数： len(tuple1)

​	f3. 获取元组中元素的最大值和最小值： max(tuple1)和min(tuple1)

​	f4. 元组和列表之间的相互转换：取长补短，转换前后不是以前的元组了

```python
list1 = list(tuple1)
tuple2 = tuple(list2)
```

​	f5. 元组遍历

```python
for i,element in enumerate(tuple1):
    print(i,element)
```



## 字典

采用`key:value`的方式进行存储，和Java中的map一样

`key`是唯一的

a. 字典的创建

```python
dict={"zhangsan":90,"lisi":100}
```

b. 字典的访问

​	**通过Key获取Value**

```python
dict={"zhangsan":90,"lisi":100}
score=dict["zhangsan"]
```

c. 字典的操作

​	c1. 添加

​		如果键存在，则将对应键的值进行修改；如果键不存在，则添加新的键值对

​	c2. 删除

​		删除指定的key，对应的value也会删除掉

​	c3. 遍历

```python
dict1 = {"zhangsan":60,"jack":90,"tom":80}

#通过key获取value
for key in dict1:
   ...:     value=dict1[key]
   ...:     print(key,value)
   ...:     
zhangsan 60
jack 90
tom 80

#只获取value
list=dict1.values()
for value in list:
   ...:     print(value)
   ...:     
60
90
80

#
for i,key in enumerate(dict1):
   ...:     value=dict1[key]
   ...:     print(i,key,value)
   ...:     
0 zhangsan 60
1 jack 90
2 tom 80

#同时获得key,value
for key,value in dict1.items():
   ...:     print(key,value)
   ...:     
zhangsan 60
jack 90
tom 80



```



# list切片

```
"""
使用模式: [start:end:step]
    其中start表示切片开始的位置,默认是0
    end表示切片截止的位置(不包含),默认是列表长度
    step表示切片的步长,默认是1
    当start是0时,可以省略;当end是列表的长度时,可以省略.
    当step是1时,也可以省略,并且省略步长时可以同时省略最后一个冒号.
    此外,当step为负数时,表示反向切片,这时start值应该比end值大.
    注意:切片操作创建了一个新的列表.
"""
```



[参考](https://www.cnblogs.com/z-qinfeng/p/12032296.html)

# view

view()的作用相当于numpy中的reshape，重新定义矩阵的形状。

一、例1 普通用法：

```
import torch
v1 = torch.range(1, 16) 
v2 = v1.view(4, 4)  
123
```

其中v1为1*16大小的张量，包含16个元素。
 v2为4*4大小的张量，同样包含16个元素。注意view前后的元素个数要相同，不然会报错。

**二、例2 参数使用-1**

```
import torch
v1 = torch.range(1, 16) 
v2 = v1.view(-1, 4)  
123
```

和图例中的用法一样，**view中一个参数定为-1，代表动态调整这个维度上的元素个数，以保证元素的总数不变**。因此两个例子的结果是相同的。


























