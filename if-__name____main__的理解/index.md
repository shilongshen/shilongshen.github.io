# if __name__=='__main__'的理解




**直观理解：**

对于当前运行的程序one.py而言，__name__ 变量的值是"__main__"。

如果two.py调用了one.py，即import one，那么对one.py而言__name__变量的值是"__one__"，对于two.py而言__name__变量的值是"__main__"。



**背后原因：**

有时我们写了可以直接被执行的模块（.py文件），但是在另一个程序中调用它时，我们其实只是想用一用里面写好的函数，而不是全都执行一遍。那么我们就可以把执行的部分放到if __name__ == '__main__' 中进行判断。

<u>如果if name == 'main' 为真，就说明我们是在直接执行这个模块，那么所有的操作都要运行一遍；但如果为假，就说明我们是引用了这个模块，只有在需要用到它的函数时，才会被调用执行。</u>

~~~python
定义一个py文件：one.py
def func():
    print("func() in one.py")

#此时"__name__"等于“__main_-”
if __name__ == "__main__":
    print("one.py is being run directly")
else:
    print("one.py is being imported into another module")
~~~

~~~python
运行one.py的结果如下：
one.py is being run directly
~~~

~~~python
定义一个py文件：two.py
import one
#此时将会运行one.py的else部分。因为对于one.py来说“__name__”不等于“__main__”。
#输出为one.py is being imported into another module
#也就是说当一个py文件被别的py文件调用时，它的"__name__"属性不等于"__main__"

print("top-level in two.py")
one.func()

if __name__ == "__main__":
    print("two.py is being run directly")
else:
    print("two.py is being imported into another module")
~~~

~~~python
运行two.py的结果如下
one.py is being imported into another module
top-level in two.py
func() in one.py
two.py is being run directly
~~~


