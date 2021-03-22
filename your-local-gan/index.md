# Your local GAN


> https://arxiv.org/abs/1911.12287v2

#### 1.Your Local GAN: Designing Two Dimensional Local Attention Mechanisms for Generative Models

`该文提出了一种局部稀疏注意力层，其能够保存几何形状和局部性`

`同时该文提出了一种新的可视化注意力的方法`



转置卷积层是GAN基础算法的重要组成部分，因为其能够捕获空间不变性。但是存在的一个限制是：卷积操作不能够构建复杂的几何与长程依赖。为了解决这一问题，注意力机制已经被引入到深度生成模型中了。注意力机制能够构建长程空间依赖，这能够自动的找到图像中相关的部分，即使两个部分相距很远。注意力首先在SAGAN中被提出，随后再BigGAN被进一步的发展。

注意力机制存在的一些问题：

第一点是计算不高效：标准的密集注意力的计算需要较大的内存和较长的时间。

第二点是统计不高效：需要大量的训练数据来训练注意力层。统计特性的不高效同样也说明了密集注意力并没有从局部性中获利，因为在图像中的大多数依赖关系主要与邻域的像素点相关.(这里可以这样理解，密集注意力机制建立了一个像素点与其他所有像素之间的关系，但是实际上，与这个像素点相关的像素点主要集中在这个这个像素点的邻域内，因此密集注意力机制的统计是不高效的。)

##### Dense attention

给定两个特征向量：
$$
{X}\in \mathbb{R}^{N_x \times E_x}\\
{Y}\in \mathbb{R}^{N_y \times E_y}
$$

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201016142947.png)

Dense Attention的输出为：
$$
O=\sigma(X_Q Y_K^T )Y_V
$$


例如自注意力机制。

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201016140111.png)

##### Sparsified Attention

在Dense attention中的复杂性在于矩阵$A_{X,Y}=X_Q . Y_K^T$.

本文中提出的稀疏注意力机制将注意力权重矩阵的计算分解成多步，在第i步中，只关注输入位置的一部分，通过二进制掩码$M_i$来确定：
$$
A_{X,Y}^i[a,b]=\left\{
             \begin{array}{lr}
             A_{X,Y}^i[a,b], & M_i[a,b]=1 \\
             - \infty, & M_i[a,b]=0\\
               
             \end{array}
\right.
$$
其中$-\infty$表示这一位置在经过softmax处理后等于0.

**关键点在于如何设计一个合理的二进制掩码$M_i$?**


