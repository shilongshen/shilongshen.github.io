# Arbitrary Style Transfer in Real-time with Adaptive Instance Normalization


该文提出了一种简单但高效的实时（**转换速度更快**）的能进行**任意风格**迁移的模型。该模型的核心为adaptive instance normalization (AdaIN) layer，**AdaIN**能够对齐content feature 与style feature的均值和方差。 

#### 1 Introduction

深度神经网络能够将图像的内容以及风格信息进行编码，而且图像的内容和风格是可分离的，因此可以实现在保存图像内容的同时对图像的风格进行改变。

现存的方法存在两个弊端：1.能够实现任意风格的迁移，但是速度较慢。2.速度较快，但是只能够实现单一风格的迁移。

在这篇文章中实现了速度快且能够实现任意风格转换。

AdaIN由instance normalization (IN)所启发。IN在feed-forward风格迁移中是十分高效的，IN的作用可以解释为：**IN通过归一化包含图像风格信息的feature statistics（特征的统计特性，例如均值和方差）来进行风格归一化。**

AdaINs是IN的拓展，其以内容和风格作为输入，通过调整内容的均值和方差来匹配风格输入d的均值和方差。

#### 2 related work

##### style transfer

​		风格迁移问题起源于non-photo-realistic rendering（非真实性渲染：通过风格形式的艺术化加工。相对的真实性渲染强调其输出的外观尽可能的与目标图像相同。）,并且与纹理合成和转移密切相关。一些早期使用的方法包括了直方图匹配和非参数采样。这些方法通常依赖于低层次的统计特性并且不能够很好的捕获语义结构信息。Gray等人首次通过在深度神经网络中匹配卷积层的统计特性来进行风格迁移，并达到了十分出色的效果。

​		Gray等人提出的网络框架由于需要最小化内容损失函数和风格损失函数来迭代更新图像，因此优化的过程十分的缓慢，在实际的应用中常常需要较长的时间来处理图像（文中并没有采用常用的梯度下降的方法，采用的是L-BFGS的优化算法，其实本身这个风格转换**只是利用的VGG网络进行特征提取**，实际上L-BFGS优化的是从一张由白噪声组成的图片，最终根据定义的损失优化得到最终的风格转换图片）。一种常见的解决为使用训练后的前馈神经网络将优化过程替代，这能够极快的提升处理图像的速度，实现实时的转换。然而这些基于前馈网络的方法存在着局限性：一个网络框架只能够生成有限风格的图像。

...

------

各种归一化方法图例

![](https://gitee.com/shilongshen/image-bad/raw/master/20200728093230.png)

#### 3 Background

> 如果一个机器学习算法在缩放全部或部分特征后不影响它的它的学习和预测，我们就称该算法具有**尺度不变性**。
>
> 从理论上，神经网络应该具有尺度不变性，可以通过参数的调整来适应不同特征的尺度．但**尺度不同的输入特征会增加训练难度**．假设一个只有一层的网络𝑦 = tanh(𝑤1𝑥1 + 𝑤2𝑥2 + 𝑏)，其中𝑥1 ∈ [0, 10]，𝑥2 ∈ [0, 1]．之前我们提到tanh 函数的导数在区间[−2, 2] 上是敏感的，其余的导数接近于0．因此，如果𝑤1𝑥1 + 𝑤2𝑥2 + 𝑏 过大或过小，都会导致梯度过小，难以训练．为了提高训练效率，我们需要使𝑤1𝑥1 + 𝑤2𝑥2 + 𝑏 在[−2, 2] 区间，因此需要将𝑤1 设得小一点，比如在[−0.1, 0.1] 之间．可以想象，如果数据维数很多时，我们很难这样精心去选择每一个参数．因此，**如果每一个特征的尺度相似，比如[0, 1] 或者[−1, 1]，我们就不太需要区别对待每一个参数，从而减少人工干预**。
>
> <u>除了参数初始化比较困难外，不同输入特征的尺度差异比较大时，梯度下降法的效率也会受到影响</u>。
>
> 尺度不同会造成在大多数位置上的梯度方向并不是最优的搜索方向．当使用梯度下降法寻求最优解时，会导致需要很多次迭代才能收敛．如果我们把数据归一化为相同尺度，如图7.9b所示，大部分位置的梯度方向近似于最优搜索方向．这样，在梯度下降求解时，每一步梯度的方向都基本指向最小值，训练效率会大大提高
>
> ![](https://gitee.com/shilongshen/image-bad/raw/master/image/20200604092535.png)
>
> **归一化（Normalization）方法泛指把数据特征转化为相同尺度的方法**。
>
> 神经网络中几种常用的归一化方法：
>
> ###### 最大最小值归一化
>
> ...
>
> ###### 白化
>
> ...
>
> ###### 标准化
>
> 将每一维的特征都调整为均值为0，方差为1.假设有$N$个样本$\{ \pmb{x}^{(n)} \}_{n=1}^N$,对于每一维特征，我们先计算它的均值和方差：
> $$
> \mu=\frac{1}{N}\sum_{n=1}^Nx^{(n)}\\
> \sigma^2=\frac{1}{N}\sum_{n=1}^N(x^{(n)}-\mu)
> $$
> 然后对数据进行标准化
> $$
> \hat{x}^n=\frac{x^{(n)}-\mu}{\sigma}
> $$
>
> ###### 将归一化应用到神经网络中：
>
> ###### 逐层归一化
>
> 逐层归一化（Layer-wise Normalization）是将传统机器学习中的数据归一化方法应用到深度神经网络中，对神经网络中隐藏层的输入进行归一化，从而使得网络更容易训练．
>
> ###### 批量归一化（Batch Normalization，BN）
>
> 批量归一化（Batch Normalization，BN）是一种有效的逐层归一化。
>
> 设神经网络第$l$层的输入为$a^{(l-1)}$,输出为$a^{(l)}$
> $$
> a^{(l)}=f(z^{(l)})=f(wa^{(l-1)}+b)
> $$
> 对$z^{(l)}$实施归一化操作：
> $$
> \hat{z}^{(l)}=\frac{z^{(l)}-\mu[z^{(l)}]}{\sqrt{\sigma[z^{(l)}]+\epsilon}}
> $$
> $\mu[z^{(l)}]$表示均值，$\sigma^2[z^{(l)}]$表示方差。$z^{(l)}$的期望和方差通常是基于小批量样本的均值和方差近似估计，给定一个包含$K$个样本的小批量样本集合，均值和方差为：
> $$
> \mu[z^{(l)}]=\frac{1}{K}\sum_{k=1}^Kz^{(k,l)}\\
> \sigma^2[z^{(l)}]=\frac{1}{K}\sum_{k=1}^K{\huge[}z^{(k,l)}-\mu[z^{(k,l)}]{\huge]}^2
> $$
> 对净输入𝒛(𝑙) 的标准归一化会使得其取值集中到0 附近，如果使用Sigmoid激活函数时，这个取值区间刚好是接近线性变换的区间，减弱了神经网络的非线性性质．因此，为了使得归一化不对网络的表示能力造成负面影响，可以通过一个**附加的缩放和平移变换改变取值区间**:
> $$
> \hat{z}^{(l)}=\gamma{\huge(}\frac{z^{(l)}-\mu[z^{(l)}]}{\sqrt{\sigma[z^{(l)}]+\epsilon}}{\huge)}+\beta
> $$
> ...
>
> 

##### 3.1 BN

给定input batch $x \in \mathbb{R}^{N\times C \times H \times W}$（相当于上述的$z$），
$$
BN(x)=\gamma{\huge(}\frac{x-\mu{(x)}}{\sigma(x)}{\huge)}+\beta
$$

> $\mu(x)$和$\sigma(x)$如何计算？
>
> 假设batchsize=N，即一次输入N张图片，其维度为$x\in \mathbb{R}^{C\times H \times W}$，然后将$x$重构为维度为$x\in \mathbb{R}^{C\times (H \times W)}$，然后将N张图片进行叠加，**沿通道方向计算均值和方差**。<u>Batch Normalization是指N张图片的每一张图片的同一个通道一起进行Normalization操作</u>。**BN注重对每个batch进行归一化，从而保证数据分布的一致性**。但是BN对batchsize的大小比较敏感，由于每次计算均值和方差是在一个batch上，所以如果batchsize太小，则计算的均值、方差不足以代表整个数据分布；
>
> 
>
> ![](https://gitee.com/shilongshen/image-bad/raw/master/image/20200604112046.png)

其中每一个通道上的均值和方差的计算公式如下：
$$
\mu_c(x)=\frac{1}{N}\sum_{n=1}^N{\huge[} \frac{1}{HW}\sum_{h=1}^H\sum_{w=1}^Wx_{nchw}  {\huge]}\\
\sigma_c(x)=\sqrt{ 
\frac{1}{N}\sum_{n=1}^N{\huge[} \frac{1}{HW}\sum_{h=1}^H\sum_{w=1}^W(x_{nchw}-\mu_{c}(x))^2  {\huge]}+\epsilon
}
$$


##### 3.2 IN

$$
IN(x)=\gamma{\huge(}\frac{x-\mu{(x)}}{\sigma(x)}{\huge)}+\beta
$$

其中每个通道上的均值和方差计算公式如下(Instance Normalization是指单张图片的单个通道单独进行Noramlization操作。)：
$$
\mu_{nc}(x)= \frac{1}{HW}\sum_{h=1}^H\sum_{w=1}^Wx_{nchw}  \\
\sigma_{nc}(x)=\sqrt{ 
 \frac{1}{HW}\sum_{h=1}^H\sum_{w=1}^W(x_{nchw}-\mu_{nc}(x))^2  +\epsilon
}
$$


![](https://gitee.com/shilongshen/image-bad/raw/master/image/20200604113539.png)



> ​	BN和IN的区别：
>
> batch norm是对一个batch里所有的图片的所有像素求均值和标准差。而instance norm是对单个图片的所有像素求均值和标准差。
>
> BN适用于判别模型中，比如图片分类模型。因为BN注重对每个batch进行归一化，从而保证数据分布的一致性，而判别模型的结果正是取决于数据整体分布。但是BN对batchsize的大小比较敏感，由于每次计算均值和方差是在一个batch上，所以如果batchsize太小，则计算的均值、方差不足以代表整个数据分布；
>
> IN适用于生成模型中，比如图片风格迁移。因为图片生成的结果主要依赖于某个图像实例，所以对整个batch归一化不适合图像风格化中，在风格迁移中使用Instance Normalization不仅可以加速模型收敛，并且可以保持每个图像实例之间的独立**。**
>
> ​	补充：
>
> Ioffe和Szegedy将BN层引入了神经网络中，其能够通过特征归一化加快前馈神经网络的训练过程。BN层最初用于判别器的训练，但是在生成模型中也同样有效。



**IN通过归一化feature statistics（平均值和方差）来进行风格归一化**。因为BN归一化一个batch的样本的feature statistics，因此可以直观理解为将一个batch的样本归一化为同一风格。当在风格迁移中，不同的图片应该具有不同的风格。而IN可以将每张图片的风格转换为目标的风格，从这一角度出发IN（对IN中的$\gamma$和$\beta$选择不同的参数进行归一化操作）的有效性也可以解释清楚：<u>不同的仿射参数可以将feature statistics归一化到不同的值，因此可以将生成图片归一化不同的风格</u>。

> 不同的仿射参数对应不同的风格。

大量的研究工作表明卷积层的特征统计特性能够捕获图像的风格信息。我们认为IN通过归一化特征统计特性（均值和方差），来进行风格信息的归一化，也就是说生成网络的特征统计特性能够控制生成图像的风格。

#### 4 Adaptive Instance Normalization（AdaIN）

IN通过一组特定的仿射参数将输入图像归一化为单一的风格，那么可以通过自适应仿射参数来将输入图像转换为任意风格（AdaIN）。

AdaIN的输入为内容$x$和风格$y$，并将$x$和$y$的均值和方差进行匹配。AdaIN中不需要进行仿射参数的学习，而是通过$y$来计算仿射参数：
$$
AdaIN(x,y)=\sigma(y){\large(} \frac{x-\mu(x)}{\sigma(x)}  {\large)}+\mu(y)
$$
**其中我们根据$\sigma(y)$来缩放$x$，根据$\mu(y)$来平移$x$，目的是将$x$和$y$的均值和方差进行匹配**。AdaIN通过在feature space 中转换feature statistics（特别是通道方向的均值和方差）来进行风格迁移，

> 经过实验研究，风格转换中的风格与IN中的仿射参数有很大关系，AdaIN扩展了IN的能力，<u>使用风格图像的均值和标准差作为仿射参数</u>，基于这样一个假设：<u>给定任意的仿射参数能够合成具有任意风格的图像</u>。
>
> AdaIN层与BN、IN类似，都是在网络内部改变feature map的分布，即通过改变仿射参数来改变feature map的分布以此来实现风格迁移，所以可以把风格迁移的任务交给AdaIN，在网络结构上实现其他的任务

#### 5 experimental

![](https://gitee.com/shilongshen/image-bad/raw/master/image/20200604200554.png)

style transfer network T以content image $c$和arbitrary style image $s$作为输入，T采用encoder-decoder结构，其中encoder $f$为预训练的VGG-19的前几层（直到 relu4_1）,将$c$和$s$通过$f$编码后送入AdaIN，得到target feature map $t$：
$$
t=AdaIN[f(c),f(s)]
$$
decoder $g$将$t$映射回image space，产生风格迁移图片$T(c,s)$:
$$
T(c,s)=g(t)
$$
使用预训练的VGG-19来计算损失函数：
$$
\mathcal{L}=\mathcal{L}_c+\lambda\mathcal{L}_s
$$
这里使用了$t$作为content target，而不是使用一般的content image的feature response。在实验中发现这能够使得收敛速度稍微变快。
$$
\mathcal{L}_c=|| f(g(t))-t ||_2
$$
由于AdaIN只是将style feature的均值和方差进行了转换，因此style loss需要匹配这些统计特性。
$$
\mathcal{L}_s=\sum_{i=1}^L|| \mu(\phi_i(g(t)))- \mu(\phi_i(s) ||_2\\+\sum_{i=1}^L|| \sigma(\phi_i(g(t)))- \sigma(\phi_i(s) ||_2
$$

#### 6 AdaIN的pytorch实现

~~~python
import torch


def calc_mean_std(feat, eps=1e-5):
    # eps is a small value added to the variance to avoid divide-by-zero.
    size = feat.size()
    assert (len(size) == 4)
    #输入的特征图维度为（N，C，H，W）
    N, C = size[:2]#读取N，C的大小
    #feat.view(N, C, -1)将维度重构为（N，C，H*W）
    #.var(dim=2)计算单张图像单个通道的方差
    feat_var = feat.view(N, C, -1).var(dim=2) + eps  # 计算单个通道的方差，再加上一个小的偏移量
    #此时print(feat_var.shape)
    #>>(N,C)
    #.var的操作将dim=2的维度给取消了，将（N，C，H*W）->(N,C)
    #相当于将图像压扁了，二维矩阵上的每一个元素表示单张图像单个通道的方差
    
    #.view(N, C, 1, 1)将(N,C)->(N,C,1,1)
    feat_std = feat_var.sqrt().view(N, C, 1, 1)  # 计算标准差，最终需要的是标准差
    
    #均值同理
    feat_mean = feat.view(N, C, -1).mean(dim=2).view(N, C, 1, 1)  # 计算均值
    return feat_mean, feat_std


def adaptive_instance_normalization(content_feat, style_feat):
    assert (content_feat.size()[:2] == style_feat.size()[:2])
    size = content_feat.size()
   
    style_mean, style_std = calc_mean_std(style_feat)#计算style 均值和标准差
 
    content_mean, content_std = calc_mean_std(content_feat)#计算content 均值和标准差
   
    
    #content_mean.expand(size)将(N,C,1,1)的均值矩阵上升为（N，C，H，W）
    #对应的单个通道的每一个pixel的数值是相同的
    #对单张图像的单个通道进行归一化处理
    normalized_feat = (content_feat - content_mean.expand(
        size)) / content_std.expand(size)
    
    # 使用style code进行缩放和平移
    #对应的content feature,一个通道需要两个style参数,一共需要N*2*C个参数
    return normalized_feat * style_std.expand(size) + style_mean.expand(size)


#测试部分
con_feat = torch.randn(2, 2, 2, 2)
# print(con_feat.size())
sty_feat = torch.randn(2, 2, 2, 2)
# print(sty_feat)
adaptive_instance_normalization(con_feat, sty_feat)
# print(out)

~~~

<img src="https://gitee.com/shilongshen/image-bad/raw/master/img/20200723192510.jpg" style="zoom:50%;" />
