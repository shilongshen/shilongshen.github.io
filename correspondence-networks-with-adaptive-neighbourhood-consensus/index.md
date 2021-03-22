# Correspondence Networks with Adaptive Neighbourhood Consensus




该文提出了一种建立两张图像间密集语义对应关系的模型（ANC-Net）,

- non-isotrppic（非各向同性） 4D convolution kernel  -- 核心，
- multi-scale self-similarity module
- orthogonal loss

ANC-Net 以两张图像作为输入，输出为4D correlation map -- 包含两张图像间所有可能匹配的匹配分数。

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200910101415.png)

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200910101440.png)

# Method

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200910105234.png)

网络结构如图所示。

- 输入$(I^s,I^t)$
- feature extractor $\mathcal{F}$  ->输出 $F^s$和$F^t$
- multi-scale self-similarity $\mathcal{S}$ ->输出 multi-scale self-similarity $S^s$和$S^t$->captures the complex self-similarity feature map
-  We can then obtain the 4D correlation map $C_s$ from $S^s$ and $S^t$ , and the **4D correlation map** $C_f$ from $F^s$ and $F^t$ . However, $C_s$ and $C_f$ are often noisy as they lack the constraints to enforce the correspondence validity, and thus are unreliable for directly extracting correspondences.
- ANC module $\mathcal{N}$ -> a stack of non-isotropic 4D convolutions ->takes $C_s$ and $C_f $ as inputs ,  refining them by considering neighbourhoods with varying sizes.
-  Finally, the ANC module combines the refined correlation maps by simply summing up the two, producing a single 4D correlation map $\bar{C}$  from which reliable correspondences can be retrieved. 

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200910111121.png)



## Multi-scale self-similarity

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200910112010.png)

给定特征$F\in \mathbb{R}^{h_f\times \omega_f \times d}$ , self-similarity map 度量每一个特征位置之间的局部相似性。

计算$F$中位置$(i,j)$处的特征$f_{i,j}$的self - similarity map 的方法是计算与它自身和它邻域之间的余弦相似性。

如上图所示，考虑一个特征$f_{i,j}$的$3\times 3$邻域，可以计算得到$3\times 3$个self-similarity scores，将其进行向量化，最终可以得到self-similarity features map $S_0\in \mathbb{R}^{h_f \times \omega_f \times 9}$。

为了进一步得到不同self-similarity features 之间的相关性，对$S_0$进行两次zero-padding convolution 操作，分别得到$S_1$和$S_2$。然后将三个不同尺寸的feature map $S_0, S_1 ,S_2$进行叠加得到$S$，其作为最后一层的输入。

## Adaptive neighborhood consensus

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200910171240.png)

> 什么是4D卷积核？
>
> 

各向同性的4D卷积核被用于修正4D correlation map 。各向同性的4D卷积核可以被认为建立了两张图像相同尺寸的两个邻域。

然而在实际的图像中的物体，有变化的尺寸和形状，描述相同语义的两个邻域可能有不同的尺寸。因此使用相同尺寸的邻域可能会引入噪声（例如不相关的背景）。为了解决这一问题引入了ANC module ，其包含了一系列的非各向同性4D卷积核。

在模型中，为了解决物体变化的尺寸和形状，将各向同性4D卷积核与非各向同性4D卷积核相结合，使得模型能够动态的决定使用哪一个卷积核

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200910185933.png)



![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200910190152.png)

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200910190233.png)



## Most likely matches

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200910191827.png)



![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200910191922.png)




























