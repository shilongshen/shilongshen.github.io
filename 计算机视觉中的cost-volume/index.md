# 计算机视觉中的cost volume




[参考](https://www.zhihu.com/question/366970399)

[参考文献](https://openaccess.thecvf.com/content_cvpr_2018/papers/Sun_PWC-Net_CNNs_for_CVPR_2018_paper.pdf)

在[文献](https://openaccess.thecvf.com/content_cvpr_2018/papers/Sun_PWC-Net_CNNs_for_CVPR_2018_paper.pdf)中是这样描述cost volume：

> ![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201024151655.png)

**明确**：在文献中是计算图像1（目标图像）和经过warp操作后的图像2（原图像）的cost volume。cost volume也称为correlation

### 计算cost volume的目的是什么？

 文章中是这样描述的：`A cost volume stores the data matching costs for associating a pixel with its corresponding pixels at the next frame`

即用于存储两张图像中各自像素之间的相似程度的。

### 如何计算cost volume

假设图像特征的大小为$x \in \mathbb{R}^{C \times H \times W}$，则像素共有$H\times W$个像素，每个像素的维度为$C$，即一个像素为$C$维的向量。

> 注意在机器学习领域中，通常将特征表示为向量的形式，所以在分析两个特征向量之间的相似性时，常用余弦相似度表示。
> $$
> cos(A,B)=\frac{A \cdot B}{||A||_2\cdot||B||_2}
> $$
> 
>
> 当将向量进行归一化之后，即$||A||_2=1$，$||B||_2=1$ 。  此时$cos(A,B)=A \cdot B$。也就是说我们可以通过向量的点积来表示两个向量之间的相似程度。
>
> 余弦相似度的取值范围是[-1,1]，相同两个向量的之间的相似度为1。



假设目标图像的维度为$x_1 \in \mathbb{R}^{C \times H \times W}$，原图像的维度为$x_2 \in \mathbb{R}^{C \times H \times W}$，那么如何计算cost volume？

实际上cost volume （或称为correlation）的计算分为两大类，一类是local  correlation，另一类称为global  correlation。

local  correlation计算目标图像像素点邻域内的相似性，常用于Optical Flow计算中。

而global  correlation则计算目标图像像素点与所有原图像像素点的相似性，常用于Geometric Correspondence，Semantic Correspondence的计算中

> 一个像素点实际上是由坐标和像素值构成的



#### local  correlation的计算

给定目标图像中的一个坐标，在原图像中找到相同的坐标。取目标图像中该坐标处的向量值分别与原图像中相同坐标处周围的$d \times d$个位置的向量值进行相乘(其中d为邻域的大小，d小于h,w)。

对每一个坐标都进行同样的操作，最终得到的correlation的维度为$d^2 \times h \times w$

#### global  correlation的计算

给定目标中的一个坐标，取该坐标的向量值分别于原图像的每一个向量进行相乘。

对每一坐标进行同样的操作，最终的得到的correlation的维度为$(h\times w) \times h \times w$

#### 
