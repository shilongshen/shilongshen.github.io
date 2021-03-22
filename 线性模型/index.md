# 线性模型


线性模型是机器学习中应用最广泛的模型，指通过样本特征的线性组合来进行预测的模型。给定一个D维样本$\pmb{x}=[x_1,...,x_D]^T$ ,其线下你给组合函数为：
$$
f(x;w)=w_1x_1+w_2x_2+...+w_Dx_D+b=\pmb{w^Tx}+b
$$
​		其中$\pmb{w}=[w_1,...w_D]^T$为为D维的权重向量。直接用$f(x;w)$来预测输出目标$y=f(x;w)$。在分类问题中，由于输出目标$y$是一些离散的标签，而$f(x;w)$的置于为实数，因此无法直接用$f(x;w)$来进行预测，需要引入一个非线性的**决策函数**$g(.)$来预测输出目标
$$
y=g(f(x;w))
$$
其中$f(x;w)$也称为**判别函数**。典型的**二分类问题**的结构图如下：

![img](https://gitee.com/shilongshen/image-bad/raw/master/image/20200514123901.png)

![img](https://gitee.com/shilongshen/image-bad/raw/master/image/20200514123630.png)

### 3.1线性判别函数和决策边界

​		一个线性分类模型是由一个（或多个）线性的判别函数$f(\pmb{x;w})=\pmb{w^Tx}+b$和非线性的决策函数$g(.)$构成。

#### 3.1.1二分类

​		二分类问题的类别标签$y$只有两种取值，通常可以设为{+1,-1}。在二分类问题中，我们只需要一个线性判别函数$f(\pmb{x;w})=\pmb{w^Tx}+b$。特征空间$\cal{R}$中满足$f(\pmb{x;w})=0$的点组成一个分割超平面，称为**决策边界**。决策边界将特征空间一分为二，划成两个区域，每一个区域对应一个类别。

![img](https://gitee.com/shilongshen/image-bad/raw/master/image/20200514132429.png)

​		给定N个样本的训练集$\cal{D}={(x^{(n)},y^{(n)}})^N_{n=1}$.其中$y^{n}\in(+1,-1)$.线性模型视图学习到参数$\pmb{w^*}$，使得对于每个样本$(x^{n},y^n)$尽量满足
$$
f(\pmb{x}^(n);\pmb{w}^*)>0\qquad       if \quad y^{(n)}=1\\
f(\pmb{x}^(n);\pmb{w}^*)>0 \qquad		if \quad  y^{(n)}=1
$$


![img](https://gitee.com/shilongshen/image-bad/raw/master/image/20200514123901.png)

#### 3.1.2多分类

​		多分类问题是指分类的类别数C大于2.多分类一般需要多个线性判别函数，但是设计这些判别函数有很多种形式。

假设一个多分类的问题的类别是$\lbrace1,2,...C\rbrace$。常用的方式有以下三种：

（1）“一对其余”方式:把多分类问题转换为𝐶 个“一对其余”的二分类问
题．这种方式共需要𝐶 个判别函数，其中第𝑐 个判别函数$f_c$ 是将类别𝑐 的样本和
不属于类别𝑐 的样本分开．

（2）“一对一方式”：：把多分类问题转换为𝐶(𝐶 − 1)/2 个“一对一”的二分
类问题．这种方式共需要𝐶(𝐶 − 1)/2 个判别函数，其中第(𝑖, 𝑗) 个判别函数是把类
别𝑖 和类别𝑗 的样本分开．

（3）“argmax”方式：这是一种改进的“一对其余"方式，共需要C个判别函数
$$
f_c(\pmb{x,w_c})=\pmb{w_c^Tx}+b_c \qquad c\in\lbrace1,...C\rbrace
$$
对于样本𝒙，<u>如果存在一个类别𝑐，相对于所有的其他类别𝑐(̃ 𝑐 ̃ ≠ 𝑐)有𝑓𝑐(𝒙;𝒘𝑐) ></u>
<u>𝑓𝑐̃(𝒙,𝒘𝑐̃)，那么𝒙属于类别𝑐</u>．“argmax”方式的预测函数定义为
$$
y={argmax}_{c=1}^Cf_c(\pmb{x;w}_c)
$$






![img](https://gitee.com/shilongshen/image-bad/raw/master/image/20200514132247.png)

### 3.2 logistic回归

​		对于二分类问题，假定$y\in\lbrace0,+1\rbrace$,给定一个输入向量$\pmb{x}$，它可能对应一张图片（假设包含猫），比如你可能想要识别这张图片是否包含一只猫，你想要一个算法能够输出预测$\hat{y}$，也就是对实际值$y$的估计。也就是说你**想要让$\hat{y}$表示$y$等于1的可能性**，前提条件是给定了输入特征$\pmb{x}$。我们引入非线性函数sigmoid函数：$g（.）=\sigma$来预测后验概率$p(y=1|\pmb{x})$。
$$
let\quad y\approx\hat{y}=p(y=1|\pmb{x})=g(f(\pmb{x;w}))=g(\pmb{w^Tx}+b)=\sigma(\pmb{w^Tx}+b)
$$
其中sigmoid函数（也称为logistics函数）的图像为：

![img](https://gitee.com/shilongshen/image-bad/raw/master/image/20200514161934.png)

sigmoid函数函数表达式为：$\sigma(z)=\frac1{1+e^{-z}}$.由表达式可知，如果$z$很大的话$\sigma(z)\approx1$。相反的，如果$z$很小，那么$\sigma(z)\approx0$。因此**此时我们的工作就是让机器学习参数$\pmb{w},b$使得$z=\pmb{w^Tx}+b$较大，这样才能使$\hat{y}$成为对$y=1$这一情况的一个很好的估计**。

#### 3.2.1 参数学习

logistics回归采用交叉熵作为损失函数，并使用梯度下降法来对参数进行更新（优化）。

损失函数为（最小化损失函数）：
$$
\Re(\pmb{w},b)=-\frac1N\sum_{n=1}^N(y^{(n)}log(\hat{y}^{(n)})+(1-y^{n})log(1-\hat{y}^{(n)}))
$$
<u>直观的理解</u>：当$y=1$时损失函数为$-log(\hat{y})$,如果想要损失函数尽可能的小，那么$\hat{y}$就要尽可能的大，又因为sigmiod函数的取值范围为[0,1]，所以$\hat{y}$会尽可能的接近1。同理当$y=0$时损失函数为$-log(1-\hat{y})$，如果想要损失函数尽可能的小，就要使$\hat{y}$尽可能接近0。

### 3.3 softmax回归

https://note.youdao.com/ynoteshare1/index.html?id=a15461dfbaf9b46fe22a75dc0ef34b46&type=note

​		Softmax 回归（Softmax Regression），也称为多项Multinomial）或多类（Multi-Class）的Logistic 回归，是Logistic 回归在多分类问题上的推广。

​		对于多分类问题，类别标签$y\in\lbrace1,2,...C\rbrace$可以有C个可能取值。给定一个样本$\pmb{x}$，softmax回归预测的属于类别c的概率为
$$
p(y=c|\pmb{x})=softmax(\pmb{w_c^Tx})=\frac {exp(\pmb{w_c^Tx})}{\sum_\tilde{c}^{C}exp(\pmb{w_\tilde{c}^Tx})}
$$


其中$\pmb{w_c}$为第第c类的权重向量。

#### 3.3.1参数学习

​		采用交叉熵损失函数，softmax的损失函数为：
$$
\Re(\pmb{w},b)=-\frac1N\sum_{n=1}^Ny^{(n)}log(\hat{y}^{(n)})
$$


​		
