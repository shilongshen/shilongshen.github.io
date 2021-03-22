# 各种激活函数




[参考](https://blog.csdn.net/c2861024198/article/details/108192771)

## sigmoid

$$
f(z)=\frac{1}{1+e^{-z}}
$$



其图像如下:

![sigmoid](https://imgconvert.csdnimg.cn/aHR0cHM6Ly94aWFveGlhYmxvZ3MudG9wL3Vzci91cGxvYWRzLzIwMjAvMDgvNDA3NTAyMDc0NS5wbmc?x-oss-process=image/format,png)

### 特点

- 能够将输入的连续实值变换为0到1之间的输出

### 缺点

- 在深度神经网络中梯度反向传播是容易造成梯度爆炸和梯度消失

### sigmoid导数

$$
f'(z)=\frac{e^{-z}}{(1+e^{-z})^2}
$$



其导数图像如下:

![d_sigmoid.png](https://xiaoxiablogs.top/usr/uploads/2020/08/1444233458.png)

## tanh

$$
tanh(x)=\frac{e^{x}-e^{-x}}{e^{x}+e^{-x}}
$$



其图像如下:

![tanh.png](https://xiaoxiablogs.top/usr/uploads/2020/08/1815796145.png)

### 特点

解决了sigmoid函数不是zero-centered的问题, 但是梯度消失依旧存在

### 导数

$$
tanh'(x)=1-(\frac{e^{x}-e^{-x}}{e^{x}+e^{-x}})^2
$$



导数图像

![d_tanh.png](https://xiaoxiablogs.top/usr/uploads/2020/08/3123494289.png)

## Relu

$$
Relu(x)=max(0,x)
$$



函数图像

![relu.png](https://xiaoxiablogs.top/usr/uploads/2020/08/1778561105.png)

### 导数

$$
Relu'(x)=\left\{
             \begin{array}{lr}
             1  &x>0
            \\
			0 &x<0
             
             \end{array}
\right.
$$



![d_relu.png](https://xiaoxiablogs.top/usr/uploads/2020/08/3559356907.png)

### 优点

- 解决了梯度消失问题
- 计算速度非常快
- 收敛速度远快于sigmoid和tanh

### 缺点

- 输出的不是zero-centered
- 有些神经元可能永远不会被激活(Dead ReLU)
  - 不好的参数初始化
  - 学习率过高, 导致网络不幸进入这种情况

## Leaky Relu(PRelu)

$$
f(x)=max(\alpha x,x)
$$



函数图像*α*=0.1

α=0.1

![prelu.png](https://xiaoxiablogs.top/usr/uploads/2020/08/3685549432.png)

### 导数

$$
f'(x)=\left\{
             \begin{array}{lr}
             1  &x\ge0
            \\
			\alpha &x<0
             
             \end{array}
\right.
$$



图像

![d_prelu.png](https://xiaoxiablogs.top/usr/uploads/2020/08/763130677.png)

### 特点

- 具有ReLU的所有优点
- 不会有Dead ReLU问题

## ELU

$$
f(x)=\left\{
             \begin{array}{lr}
             x  &x>0
            \\
			\alpha(e^x-1) &x \leq0
             
             \end{array}
\right.
$$



函数图像*α*=1

α=1

![elu.png](https://xiaoxiablogs.top/usr/uploads/2020/08/244422543.png)

### 导数

$$
f'(x)=\left\{
             \begin{array}{lr}
             1  &x>0
            \\
			\alpha e^x &x \leq0
             
             \end{array}
\right.
$$



图像*α*=1

α=1

![d_elu.png](https://xiaoxiablogs.top/usr/uploads/2020/08/1843492480.png)

### 特点

- 类似于Leaky ReLU
- 计算量稍大
- 不会有Dead ReLU问题
- 均值接近于0

## SELU

$$
f(x)=\lambda\left\{
             \begin{array}{lr}
             x  &x>0
            \\
			\alpha e^x - \alpha &x \leq0
             
             \end{array}
\right.
$$



*λ*=1.0507009873554804934193349852946

*α*=1.6732632423543772848170429916717

函数图像

![selu.png](https://xiaoxiablogs.top/usr/uploads/2020/08/3785209122.png)

### 导数

$$
f'(x)=\lambda \left\{
             \begin{array}{lr}
             1  &x>0
            \\
			\alpha e^x &\leq0
             
             \end{array}
\right.
$$



图像:

![d_selu.png](https://xiaoxiablogs.top/usr/uploads/2020/08/3070524930.png)

### 特点

- 在ELU的基础上求解了最佳的*α*

α , 并且扩大了*λ*

- λ倍,
- SELU拥有ELU所有的优点
- 不存在死区

## SoftMax

$$
f(x_i)=\frac{e^{x_i}}{\sum_{j=1}^ne^{x_j}}
$$



简单地说, 就是当前元素的值就等与e的当前元素次方在所有元素的e的次方和的比例

### 导数

当交叉熵作为损失函数时,$loss=-\sum_it_i \ lny_i$,其中,表$t_i$示真实值当预测第*i*个时,可以认为,$t_i=1$那么$loss=-\sum_i\ lny_i$因为softmax的和为1,那么$\sum_{i=1}^n(\frac{e^{x_i}}{\sum_{j=1}^ne^{x_j}})$,对loss求导后为:
$$
y_i-1
$$


当交叉熵作为损失函数时,LOSS=−i∑tilnyi,其中,ti表示真实值当预测第i个时,可以认为ti=1,那么LOSS=−∑lnyi因为softmax的和为1,那么∑j=1nexjexi,对Loss求导后为−(1−∑jnexj∑i=jnexi)=yi−1

也就是说, 只要求出$y_i$,那么减一就是梯度.

### 特点

- Softmax会将整个超空间按照分类个数进行划分
- Softmax会比其他的激活函数更适合多分类问题最后的激活
