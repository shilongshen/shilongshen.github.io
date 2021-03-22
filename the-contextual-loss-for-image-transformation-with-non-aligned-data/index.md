# The Contextual Loss for Image Transformation with Non-Aligned Data




前馈卷积神经网络用于图像翻译任务时的一个关键在于损失函数能够准确的度量真实图像和生成图像之间的相似性。大多数的损失函数假设真实图像和生成图像在空间上是对齐的，直接比较在相应位置pixel的相似性。然而在大多数任务中，在空间上对齐的训练数据对是难以得到的。

**该文提出了一种不需要对齐训练数据对的损失函数，该损失函数基于content和semantics--在考虑整张图像的content的同时比较具有相似semantics meaning的区域**。



#### Introduction

在图像翻译任务中常用的损失函数有：

（1）pixel-to-pixel 损失函数，在在相应的空间位置进行比较。包括了L1,L2和感知损失函数。

（2）Global 损失函数，例如Gram 损失函数能够通过比较整张图像的统计特性来很好的捕获风格和纹理。

（3）对抗损失函数（GAN）

存在问题：

pixel-to-pixel 损失函数适用与生成图像和目标图像在空间上是对齐的情况，对于风格迁移等任务不是特别适合。

> 该损失函数的关键思想是将图像作为特征的集合，在忽视特征的空间位置，基于特征之间的相似性的基础上进行图像相似性的度量。我们在度量生成图像特征和真实图像特征相似性时考虑了生成图像的所有特征，因此将整张图像的语境(global image content)合并到了相似性度量中。**两张图像的相似性是通过匹配的特征的相似性来定义的**。
>
> 感知损失函数的一大优点是能够保证目标图像的外观。

#### Method

##### 3.1 contextual similarity between images 

将每一张图像表示为一系列高维的点（特征），如果两张图像相应的点是相似的则认为两张图像是相似的。我们认为**如果两张图像大多数特征相似，则认为两张图像相似**。

假设生成图像为$x$，目标图像为$y$，将其表示为一系列的点（特征，可以通过预训练的VGG来提取）：$X=\{ x_i \},Y=\{ y_i \}$,并假设$|Y|=|X|=N$（有$N$个点）。我们为每一个$y_i$从$X=\{ x_i\}$中寻找最相似的$x_i$,然后将所有$y_i$相应的特征相似值进行相加：
$$
CX(x,y)=CX(X,Y)=\frac{1}{N}\sum_j \max_i CX_{i,j}
$$
其中$CX_{i,j}$定义的是$x_i$和$y_j$之间的相似性。

> 通过$CX_{i,j}$将整张图像的语境(global image content)引入。

我们称$x_i$与$y_j$是contextually similar的，如果$x_i$与$Y$中其他点相比与$y_j$更加的接近。当$x_i$与$Y$中所有的点都不接近时，$CX_{i,j}$会非常的的小。

------

设$d_{i,j}$为$x_i$与$y_j$的余弦距离，当$d_{i,j}<<d_{i,k},k \neq j$时我们认为$x_i$和$y_j$是相似的。

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200823192427.jpg)

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200823192442.jpg)

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200823192449.jpg)





#### pytorch 实现
