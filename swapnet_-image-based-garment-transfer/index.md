# SwapNet: Image Based Garment Transfer




该文提出了`Swapnet`，能够实现任意姿态下的虚拟换装。

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200828141901.png)

网络分为两阶段：

- warping : 将desired clothing根据pose进行warping,生成clothing segmentation。
- texturing :  利用desired clothing information 对clothing segmentation进行细节的服装纹理合成。

给定包含desired clothing的图像A，以及包含desired pose的图像B，目标是生成具有B的姿态且有A的服装的图像$B'$ （B中的人物穿上了A的服装）。



## Warping Module

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200828143445.png)

以A的语义分割图$A_{cs}$和B的语义分割图$B_{cs}$为输入，合成$B'_{CS}$,其同时具有A的语义形状、标签以及B的体型和姿态。

生成的结果strongly conditioned on the body segmentation以及weakly conditioned on the clothing segmentation。这是通过将 clothing segmentation经过下采样再上采样的操作（？）。这种设计网络结构的好处在于：（？）

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200828145030.png)

损失函数部分：

没看懂

## Texturing Module

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200828145719.png)

损失函数:
$$
\mathcal{L}_{L1}=|| f_g-A ||_1
\\ \mathcal{L}_{feat}=\sum_l\lambda_l || \phi_l(f_g)-\phi_l(A) ||_2
\\ \mathcal{L}_{GAN}s
$$











