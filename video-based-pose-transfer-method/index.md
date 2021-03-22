# video-based pose transfer method








#### Dance Dance Generation: Motion Transfer for Internet Videos

该文章可以实现在复杂背景下的pose transfer。

In summary, our **contributions** include the following.
1) We demonstrate personalized motion transfer on videos from the Internet. 

2) We propose a novel two-stage frame-work to synthesize people performing new movements and
**fuse them seamlessly with background scenes**. （主要贡献：实现复杂背景下的姿态转换）

3) We perform qualitative and quantitative evaluations validating the superiority of our method over existing state-of-the-art.

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201219222620.png)

<img src="https://gitee.com/shilongshen/image-bad/raw/master/img/20201219222456.png" style="zoom: 200%;" />

method: (主要的思想是先将利用语义分割图将前景中的人物进行分割，采用STN 将前景人物与目标人物进行对齐。随后通过阶段进行修正）

- 利用语义分割图将前景中的人物进行分割，采用STN 将前景人物与目标人物进行对齐。
- Human synthesis net：将对齐的body parts与target pose 作为输入，对body parts进行修正，并得到前景mask
- fusion net：将body parts + background +target pose 作为输入，进行前景和背景的融合，实现复杂背景下的pose transfer



这里需要注意的点;

- 如何保证生成视频帧在时间上是平滑的？方法: target pose采用多帧的姿态表示作为输入。

  ![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201219223607.png)



存在的问题：

当source person和target person将的body shape 存在较大差异时，可能生成的结果就不那么理想了。





#### TransMoMo: Invariance-Driven Unsupervised Video Motion Retargeting



![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201220093913.png)

难点：

1.原图像和目标图像间存在较大的结构和视角变化

2.难以构建合适的训练对进行训练

3.human motion的变化是复杂的。



解决思路:

三阶段网络：（这使得我们能够更加的关注motion retarget，其中步骤1,步骤3是直接采用现有最好的方法即可）

1.skeleton extraction

2.motion retarget（主要贡献处：invariance-driven disentanglement）

3.skeleton-to-video rendering



为了解决第2和第3个难点，利用了三个因素的不变性质：structure（表示**体型**），motion（表示**姿态**），view-angle（表示**相机视角**）。具体来说：

1.当structure 和 view-angle变化时，motion是不变的

2.当view-angle变化时，structure是不变的，同时structure不会随着时间的变化而变化

3.当structure变化时，view-angle是不变的，同时view-angle不会随着时间的变化而变化

这些不变特性使得我们能够设计一些无监督函数来将skeleton解耦成三个正交的隐变量：structure，motion，view-angle。



可以通过mix来自不同skeleton的structure（原图像的structure，即原人物体型）和motion（目标图像的motion，即目标姿态）来实现motion retarget; 通过在decoder阶段采用不同的view-angle来实现不同视角下的motion retarget



网络结构：

<img src="https://gitee.com/shilongshen/image-bad/raw/master/img/20201220094630.png" style="zoom:200%;" />



具体的实施步骤：

- 通过现有的姿态提取器提取source video中人物姿态（多帧）以及提取target video中人物姿态（多帧）。
- motion retarget network中由encoder 和 decoder组成，encoder将skeleton通过三个独立的解码器进行解码，得到view-angle code 、motion code和structure code。
- [将source video中的motion code和target video 中的 structure code结合以及任取一个view-angle code（用于实现视角变化] 经过decoder进行解码，得到一个3D的，可以具有不同视角的 Retargeted skeleton code ，最后再将这个3D  Retargeted skeleton code映射为2D Retargeted skeleton
- 将和 target video中人物的纹理通过skeleton-to-video rendering渲染到Retargeted skeleton 上。实现motion transfer task



在motion retarget network中的网络细节：

输入是一个skeleton 序列（多帧）：$x \in \mathbb{R}^{T \times 2N}$ 。$T$表示帧数，$N$表示骨骼点。encoder分为三个部分

- motion encoder： 结构：several layers of one dimensional temporal convolution 。输出$E_m(x)=m \in \mathbb{R}^{M \times C_m}$。$M$表示帧数，$C_m$表示通道数

  

- structure encoder：结构：several layers of one dimensional temporal convolution+ temporal max pooling。输出：$\bar{E}_s(x)=\bar{s} \in \mathbb{R}^{C_s} =pool(s)$   其中   $E_s(x)=s \in \mathbb{R}^{M \times C_s}$

- view-angle encoder: 结构：several layers of one dimensional temporal convolution+ temporal max pooling。输出：$\bar{E}_v(x)=\bar{v} \in \mathbb{R}^{C_v} =pool(s)$   其中   $E_v(x)=v \in \mathbb{R}^{M \times C_v}$

- 将$m,\bar{s},\bar{v}$进行组合，经由decoder得到3D  Retargeted skeleton code：${\large{\hat{X}}}=G(m,\bar{s},\bar{v}) \in \mathbb{R}^{T \times 3N}$





本文的**<u>关键点</u>**在于如何确保通过motion retarget network提取的structure（表示**体型**），motion（表示**姿态**），view-angle（表示**相机视角**）是解耦的。（skeleton ->  structure，motion，view-angle）

- 结构变化处理：

  将输入的skeleton进行缩放处理。

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201220134534.png)

- 视角变化处理：

  将输入的skeleton进行360度视角转换

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201220135658.png)

现在要通过loss term保证前面提到的三个不变特性：

> 1.当structure 和 view-angle变化时，motion是不变的
>
> 2.当view-angle变化时，structure是不变的，同时structure不会随着时间的变化而变化
>
> 3.当structure变化时，view-angle是不变的，同时view-angle不会随着时间的变化而变化





- **1. Invariance of motion** 

  - Cross Reconstruction Loss

  

  - ![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201220140207.png)

  ![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201220140312.png)

  ​	

  - Structural Invariance Loss

  

  ![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201220140814.png)

  - Rotation Invariance Loss

  ![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201220140847.png)



- **2.Invariance of Structure**

  - Triplet Loss（确保structure 不随时间变化，这个不太理解）

  ![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201220141239.png)

  

  -  Rotation Invariance Loss

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201220141322.png)

- **3.Invariance of View-Angle**

  - Triplet Loss（确保view-angle不随时间变化，这个不太理解）

  ![image-20201220141410582](/home/ssl/.config/Typora/typora-user-images/image-20201220141410582.png)

  -  Structural Invariance Loss

  
  
  ![image-20201220141430954](/home/ssl/.config/Typora/typora-user-images/image-20201220141430954.png)





可能存在的一些问题：

仅仅从skeleton的角度进行处理，并没有显示的考虑纹理信息（即没有考虑skeleton与纹理之间的对齐问题）。从这一角度出发是不是可以进行优化。

生成结果的时间连续性上的处理是采用多帧的skeleton进行输入。







#### Deep Spatial Transformation for Pose-Guided Person Image Generation and Animation



该文首先基于图像设计了一种新颖的网络框架，随后又将其拓展到视频生成（主要加上了skeleton降噪处理时间平滑性处理）

这里只介绍video-based person generation



**第一部分**为skeleton的降噪处理。

作者认为通过现有方法（如openpose）提取出来的skeleton的表示并不精确，因此首先对提取出来的skeleton进行降噪处理

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201221155128.png)

**第二部分**为视频帧时间平滑处理

![image-20201221155417583](/home/ssl/.config/Typora/typora-user-images/image-20201221155417583.png)



- 将source image(将source image根据目标姿态进行warp)和前一帧的输出(将前一帧的输出根据目标姿态进行warp)共同作为当前帧生成网络的输入

针对source image 和 前一帧的输出使用独立的 模块。最后再将两者的输出相加作为当前帧的输出。

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201221155858.png)



#### First Order Motion Model for Image Animation

[参考1](https://zhuanlan.zhihu.com/p/269635265)

[参考2](https://zhuanlan.zhihu.com/p/136606648)

> ## Method
>
> 
>
> ![img](https://pic4.zhimg.com/80/v2-947730fe3afaf619392747a3f43570d7_720w.jpg)
>
> 
>
> 首先因为没有完美的监督信息，所以文章借鉴了monkey-net的训练方法：用同一个视频同时作为source image和driving video来利用本身作为监督信息，这类似于一种自监督的学习机制。然后文章提出的方法大概包括以下三个模块：
>
> ### Local Affine Transformations for Approximate Motion Description
>
> 这个部分的理解我们首先需要考虑一个非常简单的问题：如何用一种最naive的方法来借助driving video中的关键点帮助调整source  image中的motion？这个问题的解答可能会让人想到一种简单的映射函数:R2->R2，也就是将一个帧里的像素映射到另一帧里面去，这种思想非常类似于inpainting里面的examplar的方法：像素迁移，这种映射关系在光流场中被称为后向光流场。
>
> 但是作者没有直接地将D映射到S，而是假设了**一种中间的reference帧来帮助建立过渡关系**，这一篇的独到之处在于用local affine  transformations来逼近运动的表述，也就是用泰勒展开来逼近于关键点在空间的位移，关键点和仿射系数都是由关键点检测的网络来输出。
>
> > 我对于这一步的理解其实很像光流的计算原理：
> >
> > 
> >
> > ![img](https://pic2.zhimg.com/80/v2-0a1512aae984aea1d152b43a55f55cf5_720w.jpg)
> >
> > 
> >
> > 也就是说可以**用关键点的位置加上一个映射的仿射系数和无穷小量来<u>表示运动之后的关键点的位置</u>**，其中关键点就是当前的位置信息，然后映射系数就是motion信息，最后无穷小量可以被忽略不计。
>
> 
>
> ![img](https://pic4.zhimg.com/80/v2-d2cc848d05de97ede7c91d122da0c25b_720w.jpg)
>
> 
>
> 当然这一步我觉得是需要基于一个物理假设的就是每一个关键点对应的一个刚体，其上的运动是一样的，然后就是可以用泰勒展开的方法来逼近这个刚体部分的运动。（文章提到了monkey-net其实就是只用了零阶的泰勒展开，而本文进一步优化提出了一阶的泰勒展开）
>
> ### Occlusion-aware Image Generation
>
> 第二步是由上一步预测得到的关键点和仿射系数来预测一个更加dense的光流场变化，并输出一个occlusion mask来指示哪个区域直接transfer哪个区域需要inpainting。
>
> 然后就是生成器首先用上一步预测得到的dense的光流场来warp source图像，并结合那些occlusion的区域进行inpainting得到最终的输出。



####  DwNet: Dense warp-based network for pose-guided human video generation

​	![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210118221454.png)

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210118221621.png)







#### Cross-Identity Motion Transfer for Arbitrary Objects Through Pose-Attentive Video Reassembling

贡献：

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210119100208.png)

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210119100340.png)

> - 基于warp的方法能够解决解决较小的形变问题，但是还是存在一下几点问题：
>
>   ​	1.难以构建大的、复杂的位移
>
>   ​	2.不能够使用多个原图像进行生成(使用多个原图像同时进行生成可以起到相互补充的作用)
>
> - 之前的方法仅仅使用一张原图像进行生成。当原图像和目标图像之间存在较大的形变时，就会出现生成图像和目标图像之间无法存在一一对应的关系，导致生成的结果差；通过使用多张原图像可以起到相互补充的作用(可以利用多张原图像的外观)
>
> - 采用交叉训练的方式，这能够使其进行不同外观物体之间的motion transfer

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210119100544.png)



![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210119100709.png)





Pose-Dependent Appearance Embedding

提取图像的姿态编码和外观编码

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210119104800.png)



Pose-Attentive Retrieval Block

使用注意力机制来融合多个原图像的外观信息

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210119110400.png)



Image Generation

考虑背景的影响，单独使用一个自编码器学习背景信息。最后再将其融合进前景中

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210119110822.png)



训练方式

训练方式分为了重构训练和交叉训练

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210119111102.png)

重构损失

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210119111245.png)

交叉损失

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210119111325.png)

总的损失函数

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210119111358.png)

推理阶段

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210119111709.png)

> **本文存在的最大的一个问题在于是一张一张的生成图像，并没有考虑帧与帧之间的连续性**,即一帧一帧的生成图像->可以考虑处理帧与帧之间的连续性
>
> 还有一个问题就是只在一个维度的特征空间中进行attention的融合，可以考虑多维度的融合
