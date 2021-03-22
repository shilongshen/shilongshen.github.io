# SieveNet: A Unified Framework for Robust Image-Based Virtual Try-On


![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200914161443.png)

---

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200914161830.png)

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200914161945.png)



- two-stage spatial-transformer  - to capture fine details in the geometric iwarping stage
- conditional segmentation mask generation module - to prevent garment textures from bleeding onto skin and other areas.
- perceptual geometric matching loss - to impove warping output
- duelling triplet loss strategy - to improve output from the translation network



## inputs

- $I_P$ : try-on cloth image
- $I_{priors}$ : 19 channel pose and body-shape map

- $I_m$ : target model image 



## Coarse2Fine Warping

目标：将$I_p$进行warp操作，与$I_m$的姿态和形状进行对齐。

基于STN进行warp操作



### Tackling Occlusion and Pose-variation

作者认为若要进行准确的warping 操作需要考虑一下两点：

1. Large variations in shape or pose between the try-
on cloth image and the corresponding regions in the
model image.
2. Occlusions in the model image. For example, the long
hair of a person may occlude part of the garment near
the top.

**基于STN进行warp操作**:

<img src="https://gitee.com/shilongshen/image-bad/raw/master/img/20200915111247.png" style="zoom:200%;" />





第一阶段（Coarse）以$I_p$和$I_{priors}$作为输入，产生的参数$\theta$,对$I_p$进行warp操作，产生$I_{stn}^0$

第二阶段（fine）以$I_{stn}^0$和$I_{priors}$作为输入，产生参数$\Delta \theta$，以$\theta+\Delta\theta$对$I_p$进行warp操作，产生$I_{stn}^1$

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200915112059.png)

---

### Perceptual Geometric Matching Loss

$$
L_{warp}=\lambda_1L_s^0+\lambda_2L_s^1+\lambda_3L_{pgm}
$$

其中：
$$
L_S^0=\|  I_{gt}-I_{stn}^0 \|  \\
L_S^1=\|  I_{gt}-I_{stn}^1 \|
$$

$$
L_{pgm}=\lambda_4L_{push}+\lambda_5L_{align}
$$

又
$$
L_{push}=k*L_s^1-\| I_{stn}^1-I_{stn}^0 \|
$$
这是为了确保$I_{stn}^1$比$I_{stn}^0$更加接近$I_{gt}$,$I_{gt}$表示目标服装。
$$
V^0=VGG(I_{stn}^0)-VGG(I_{gt})\\
V^1=VGG(I_{stn}^1)-VGG(I_{gt})\\
\\
L_{align}=(CosineSimilarity(V^0,V^1)-1)^2
$$
实际上是采用了余弦距离进行度量，目的是保证向量$V^0$和$V^1$更加的接近。同时最小化$L_{align}$能够促进最小化$L_{push}$。

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200915144448.png)



## Texture Transfer

### Conditional Segmentation Mask Prediction

现有方法的关键问题是不能够准确的遵守clothing product 和skin的界限。会出现clothing product pixels**渗色**到skin pixel，或者是skin pixel渗色到clothing product pixels。在自遮挡的情况下，skin pixels可能会被完全的替代掉。当try-on image和clothing in the model image上的形状不一致时，这种情况尤为严重。同时当target model有复杂的姿势时也会这种情况。



输入为$I_{priors}$、$I_p$

输出为$M_{exp}$ -->  “expected ” segmentation mask.target model is wearing the try-on cloth.

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200915151111.png)

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200915153725.png)

注意损失函数的使用。

损失函数为加权交叉熵损失函数。在skin和background处增加了权重。在skin处增加权重能够更好的解决自遮挡问题。在background处增加权重能够阻止skin pixels渗色到background。



### Segmentation Assisted Texture Translation

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200915163822.png)

输入为：

- The warped product image $I_{stn}^1$
- The expected seg. mask $M_{exp}$
- Pixels of $I_m$ for the unaffected regions, (Texture Trans-
  lation Priors in Figure 3). E.g. face and bottom cloth,
  if a top garment is being tried-on.

输出：

- an RGB rendered person image $I_{rp}$
- a composition mask $M_{cm}$  -> 

最终的try-on image:
$$
I_{try-on}=M_{cm}*I_{stn}^1+(1-M_{cm})*I_{rp}
$$
![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200915155651.png)

损失函数：
$$
L_{tt}=L_{l1}+L_{percep}+L_{mask}\\
L_{l1}=\| I_{try-on}-I_m \|\\
L_{percep}=\| VGG(I_{try-on})-VGG(I_m) \|\\
L_{mask}=\| M_{cm}-M_{gt}^{cloth} \|
$$
![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200915160116.png)

注意此处的训练策略：

前K步使用$L_{tt}$进行训练，得到一个较为合理的结果。

之后再用$L_{tt}$加上triplet loss进行细粒度挖掘。



### Duelling Triplet Loss Strategy

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200915162501.png)

其中$I_{try-on}^{i_{prev}}$表示上一步的输出，其作为negative


