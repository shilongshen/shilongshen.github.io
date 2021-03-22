# ClothFlow A Folw-Based Model for Clothed Person Generation




该文提出了基于外观流（clothflow）的生成模型来进行姿态引导下的人物图像生成研究（以及虚拟试衣）。

![](https://gitee.com/shilongshen/image-bad/raw/master/20200708080730.png)

通过估计source clothing 和target clothing之间的光流，能够的建立两者之间的几何变换。

![](https://gitee.com/shilongshen/image-bad/raw/master/20200708100339.png)

![](https://gitee.com/shilongshen/image-bad/raw/master/20200708100429.png)

------

共分为三个阶段:

###### 第一阶段：

以条件姿态为指导，来生成target person semantic layout 来对生成过程提供丰富的指导。将姿态和外观进行解耦，使得clothflow生成更加空间相关的结果。

![](https://gitee.com/shilongshen/image-bad/raw/master/20200708093043.png)

###### 阶段二：

![](https://gitee.com/shilongshen/image-bad/raw/master/20200708081228.png)

![](https://gitee.com/shilongshen/image-bad/raw/master/20200708093507.png)

阶段二为clothflow flow 估计阶段（flow的作用是什么：**用于表示原图像中的哪些像素可以被用于生成目标图像的二维坐标向量）。**

![](https://gitee.com/shilongshen/image-bad/raw/master/20200708110124.png)

使用上一阶段得到的target person semantic layout作为输入来得到cloth flow。source cloth region之后通过cloth flow进行warping，以解决几何变形。 预测的外观流提供了视觉对应关系的准确估计，并有助于无缝转移源衣服区域以合成目标图像。

###### 阶段三：

生成模型以warped clothing region作为输入来对target pose进行渲染。

![](https://gitee.com/shilongshen/image-bad/raw/master/20200708095425.png)

------



#### introduction

![](https://gitee.com/shilongshen/image-bad/raw/master/20200708081808.png)

受到image-to-image translation工作的启发，一些工作直接将原图像和目标姿态作为输入，来生成目标图像。但是这些工作并没有考虑由于人体非刚性的特征引起的变形和遮挡问题，这导致了不能够生成精细的纹理细节。

为了解决geometric deformation问题以更好的进行appearance transfer，提出了两种不同的方法：**deformation-based methods** and **DensePose-based methods**.

![](https://gitee.com/shilongshen/image-bad/raw/master/20200708083409.png)

**deformation-based methods** estimate a transformation，包括了使用affine和TPS来对source image pixel或者是feature map进行deform，以解决由于姿态变化引起的不对齐问题。尽管通过这两种几何建模方法已经取得了很大的进步，但是这种方法的自由度不够高，不能够准确的进行deform。

![](https://gitee.com/shilongshen/image-bad/raw/master/20200708084433.png)

**DensePose-based methods**能够将2Dpixel映射到3D body surface，这能够更容易的获得纹理信息。但是基于dense pose的方法生成的图像引入artifacts，例如在原图像和目标图像中有不对应的部分，生成的图像产生空洞。除此之外，基于dense pose的方法的计算量较大。

![](https://gitee.com/shilongshen/image-bad/raw/master/20200708090112.png)

------

#### related work

###### Warping-based Image Matching and Synthesis

在这篇文章中，我们的目标是将source cloth  warp 成 target cloth。cloth region是非刚性的，source和target之间没有明确的对应关系。

###### Optical Flow Estimation

![](https://gitee.com/shilongshen/image-bad/raw/master/20200708104606.png)

光流法常用于视频处理中，以两个连续的视频帧作为孪生神经网络的输入，之后将第一帧的pixel或feature warp成第二帧。

------

![](https://gitee.com/shilongshen/image-bad/raw/master/20200708105325.png)

###### Conditional Layout Generation

生成目标图像的语义布局图来对外观生成进行指导（限制）。
$$
\hat{s}_t=G_{layout}(I_s,s_s,p_t)
$$
$s_t,s_s$分别表示目标图像和原图像的segmentation map，$\hat{s}_t$表示生成目标图像的segmentation map。

![](https://gitee.com/shilongshen/image-bad/raw/master/20200708112528.png)

> 为什么不直接用$s_t$？

###### Cascaded Clothing Flow Estimation

![](https://gitee.com/shilongshen/image-bad/raw/master/20200708154120.png)

双金字塔结构：source feature pyramid以及target feature pyramid

source feature pyramid：输入为$c_s:source \ cloth;s_s$,通过下采样得到特征$\{ S_1,S2,S3,S4 \}$

target feature pyramid：输入为$target \ seg: \ s_t$通过下采样得到特征$\{ T_1,T2,T3,T4 \}$

通过级联的方式计算clothing flow $F_1,F_2,F_3,F_4$。计算方式如下
$$
F_4=E_4([S_4,T_4])\\
F_3=\mathcal{U}（F_4)+E_3(\ [\mathcal{W}\ (S_3,\mathcal{U}(F_4)\ ),T_3]\ )\\
F_2=\mathcal{U}（F_3)+E_2(\ [\mathcal{W}\ (S_2,\mathcal{U}(F_3)\ ),T_2]\ )\\
F_1=\mathcal{U}（F_2)+E_1(\ [\mathcal{W}\ (S_1,\mathcal{U}(F_2)\ ),T_1]\ )
$$
其中$E_N$表示卷积层，$\mathcal{U}(\ .)$表示$a \times 2$ nearest-neighbor upsampling。$\mathcal{W}(S,F)$表示根据$F$使用双线性插值将feature map $S$进行warping，这使得网络能够通过反向传播进行训练。最后利用$F_1$将source cloth $c_s$进行warping：$c_s^"=\mathcal{W}\ (c_s,\mathcal{U}(F_1)\ )$。最终的目标：

![](https://gitee.com/shilongshen/image-bad/raw/master/20200708160704.png)

###### Clothing Preserving Rendering

![](https://gitee.com/shilongshen/image-bad/raw/master/20200708162113.png)

![](https://gitee.com/shilongshen/image-bad/raw/master/20200708163038.png)

> 既然有I_t了为什么得不到s_t？
