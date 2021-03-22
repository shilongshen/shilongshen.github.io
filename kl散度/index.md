# KL散度




[参考](https://blog.csdn.net/qq_29462849/article/details/80630684?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.channel_param)

#### KL散度用来做什么？

`KL散度的用途`：比较两个概率分布的接近程度。 

 在统计应用中，我们经常需要用一个简单的，近似的概率分布*f*∗

来描述一个复杂的概率分布*f*。这个时候，<u>我们需要一个量来衡量我们选择的近似分布 *f*∗相比原分布*f*究竟损失了多少信息量，这就是KL散度起作用的地方。</u>



#### 熵

为了更好的理解KL散度，在这里首先抛出熵的概念。在信息论这门学科中，一个很重要的目标就是<u>量化描述数据中含有多少信息</u>。 为此，提出了熵的概念，记作H，一个概率分布所对应的熵表达如下： 
$$
H= −\sum_{i=1}^N=p(x_i)⋅\log (p(x_i))
$$
果我们使用 log⁡2 作为底，熵可以被理解为：`我们编码所有信息所需要的最小位数`。  需要注意的是：通过计算熵，我们可以知道信息编码需要的最小位数，却不能确定最佳的数据压缩策略。怎样选择最优数据压缩策略，使得数据存储位数与熵计算的位数相同，达到最优压缩，是另一个庞大的课题。

#### KL散度的计算

现在，我们能够量化数据中的信息量了，就可以来衡量近似分布带来的信息损失了。KL散度的计算公式其实是熵计算公式的简单变形,在原有概率分布 *p* 上，加入我们的近似概率分布 *q*，<u>计算他们的每个取值对应对数的差</u>：
$$
D_{KL}(p||q)= ∑_{i=1}^Np(x_i)⋅(log(p(x_i))−log(q(x_i)))\\

=∑_{i=1}^Np(x_i)⋅log \frac{p(x_i)}{q(x_i)}
$$
换句话说，`KL散度计算的就是数据的原分布与近似分布的概率的对数差的期望值`。 在对数以2为底时，log2 ，可以理解为“我们损失了多少位的信息”

KL散度的连续定义：
$$
D_{KL}(p||q)=\int p(x)⋅log \frac{p(x)}{q(x)}dx
$$

