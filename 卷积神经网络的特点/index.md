# 卷积神经网络的特点




[参考1](http://www.huaxiaozhuan.com/%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0/chapters/5_CNN.html)

[参考2](https://www.cnblogs.com/wj-1314/p/9593364.html)

#### 1.稀疏连接

卷积层与下一层的连接数大大减小

或者可以说是上一层的某一个神经元只影响了下一层的某些神经元，而不是全部神经元



![](https://gitee.com/shilongshen/image-bad/raw/master/20200706085420.png)

或者说是下一层的某一神经元只被上一层的某些神经元所影响，而不是全部神经元

![image-20200706090125014](C:\Users\HIT\AppData\Roaming\Typora\typora-user-images\image-20200706090125014.png)

#### 2.参数共享

作为参数的卷积核$w^(l)$对于第$l$层的所有的神经元都是相同的。

![image-20200706090731377](C:\Users\HIT\AppData\Roaming\Typora\typora-user-images\image-20200706090731377.png)

参数共享可以理解为一个卷积核直捕捉输入数据中的一种特定的局部特征，因此，如果要提取多种特征就需要使用多个不同的卷积核。

#### 3.等变表示

![](https://gitee.com/shilongshen/image-bad/raw/master/20200706094436.png)

> **卷积层负责提取特征，采样层负责特征选择，全连接层负责分类**
