# Towards Fine-grained Human Pose Transfer with Detail Replenishing Network


现有的姿态引导下图像生成方法存在着三个问题：细节缺失，内容模糊以及风格不一致，这严重降低可图像的质量以及真实性。本文提出了一种**细粒度**姿态引导下图像生成方法，该方法更加注重于语义的完整性以及细节的补充，该方法将**内容合成(local warping)和特征转移(style transfer)的概念以相互引导的方式结合在一起**。并提出了一个细节补充网络（DRN）。此外还提出了一套细粒度评估方法，包括了语义分析、结构检测和感知质量评估。HPT和FHPT的比较如下图：

![](https://gitee.com/shilongshen/image-bad/raw/master/20200613111643.png)



### A. HPT Methods

基于特征转换机制，我们可以将已有的HPT方法分为三类：global predictive methods、local warping methods 以及 hybrid methods。

![](https://gitee.com/shilongshen/image-bad/raw/master/image/20200610075036.png)

该图展示了三种方法不同。我们可以从输出的结果分析出这三种方法存在的两个缺陷：保存原图片语义和外观细节的能力以及在遮挡区域合成新的内容的能力。

#### Global predictive methods

该方法将HPT视为多模态image-to-image translation问题，使用具有跳跃连接的U-net网络进行特征的前向传播。通过将提取的姿态表示与原图像叠加来将“姿态引导”引入。例如上图中（a）将原姿态$I_s$和目标姿态$I_t$进行编码并与原图像$I_s$进行叠加，直接生成具有目标姿态的图像$\bar{I_t}$。然而，由于缺乏准确的变形建模，这些工作往往不能可靠地解决[35]不同姿态之间的结构不对齐问题。通常来说，**global predictive methods 会缺乏捕获相应局部特征的能力**，这会导致生成图像中细节缺失，例如模糊的细节和失真,过于平滑的衣服。

#### Local warping  methods

该方法受到了spatial transformer networks的启发，将deformation构建在特征的前向传播中。例如[DSC]()中使用part-wise 仿射变换，[PATN]()中使用注意力机制来增加deformation modeling的灵活性。

不同姿态之间的变形映射通常是通过相应有限的姿态关键点的插值实现的。假设原图像\目标图像中人体的区域为$\Omega_s$ \ $\Omega_t$.将deformation 视为一个函数：$T_{st}:\Omega_s \rightarrow \Omega_t$,将区域用关键点表示，则有$T_{st}(p_s)=p_t$.通常来说这样的映射不是唯一的，需要通过额外的正则项（blending energy）来减缓warped contents的失真。

由于视角变化和自遮挡的情况，无法保证estimated warping 覆盖整个target human body ,也就是说$\Omega_t-T_{st}\neq \phi$.因此**local warping network很难恢复原图像中没有精确对应的underlying content，这会导致内容的模棱两可**。

---

`由于视角变化和自遮挡的情况，无法保证estimated warping 覆盖整个target human body `的理解：

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201121101914.png)

我们建立warping 最终的目的就是将原图像进行形变操作。实际是可以将目标图像看作是原图像的形变版本（由于姿态的变化）。

建立的warping， 例如光流法，实际上就是寻找原图像和目标图像之间的关系。具体来说就是目标图像中像素点对应的是原图像的哪一个像素点，如上图中红点所示，或者更准确的说应该是：目标图像的像素点是根据原图像中的像素点采样而来的。

但是存在的一个问题是。在pose-guided person generation 任务中，由于姿态的变化，可能会导致目标图像中的像素点在原图像中找不到对应的像素点。如上图中蓝点所示。换个说法就是`无法保证estimated warping 覆盖整个target human body`。这个时候生成图像中的某一些部位可能就会产生`内容的模棱两可`。或者说是产生`hole`或者说明`unable to generate new contents`。

[论文](openaccess.thecvf.com/content_CVPR_2020/papers/Ren_Deep_Image_Spatial_Transformation_for_Person_Image_Generation_CVPR_2020_paper.pdf)中的描述：

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201121104603.png)



---

#### Hybrid methods

利用另一个global predictive branch在uncovered rigions来生成（hallucinate）新的内容，最后再将global和local生成结果进行结合（包含global branch和local branch）according to an estimated composition mask。这种方法常用于视频帧预测框架中。然而现有的hybrid methods通常将image-level的生成结果进行结合，intermediate-level 的特征是很少被研究的。通常来说，global branch和local branch通常是分离的。这会导致hallucinated和warped contents之间的style inconsistency。

### C. FHPT Network

![](https://gitee.com/shilongshen/image-bad/raw/master/20200613110130.png)

该文提出的FHPT与其他方法的对比如上图.

FHPT存在的根本问题是保存和补充细粒度语义和外观特征，这一问题之前是采用两种方法来解决的。其中一种是基于high-level 语义指导、用global predictive method来生成new contents。另一种方法是利用warping flow或注意力机制，将原图像的low-level特征进行local warping。而 hybrid methods则将两种方法进行了整合，但是由于该方法生成的图像会存在style inconsistency。针对以上问题，该问题提出了available image content的转换和new content的生成应该以一种相互指导的方式来共同保证style consistency和生成图像的逼真的细节。

该文提出了Detail Replenishing Network (DRN)来验证FHPT方法的有效性：1)detail replenishing module来使得style consistency。2）intermediate feature module pathway来促进相互指导。

![](https://gitee.com/shilongshen/image-bad/raw/master/20200612212456.png)

DRN包含两个branch：pose transfer branch和detail replenishing branch。

#### pose represention

提取原图像$I_S$的姿态$P_s$和目标图像$I_t$的姿态$P_t$。提取原图像的面部图像$B$,以及目标图像的面部姿态$H_F$。

#### pose transfer branch

$$
\hat{I_c}=G_p(I_s.P_s.P_t)
$$

其中$I_c$为粗糙的图像。这里的生成器采用了[PATN]()的级联模块。feature map $F_t$被用于detail replenishing module的spatial guidance。transfer branch应该提供有用的引导“where to add what kind  of details".

#### detail replenishing branch

detail replenishing branch包含几个detail replenishing module（DRM),具体来说，使用了两种类型的DRM来分别修正整张图片和面部图片。global module通过$F_t$和$I_s$来生成residual map$R_t$,并将其添加到$\hat{I_c}$.regional module通过面部图片$I_F$和目标面部姿态$H_F$来生成目标面部图片$\hat{I}_F$，通过这种方式能够生成更加逼真的面部图片。为了将两个module的输出进行结合，采用了具有高斯混合权重掩模的alpha blending。$M_F=g(\sigma)*1_{BF}$,*表示卷积操作，$g(\sigma)$表示离散2D卷积核，$1_{BF}$表示指示函数，当相应的pixel属于$B_F$时等于1，否则等于0.
$$
\hat{I}_t=(1-M_F)(\hat{I}_c+R_t)+M_F\hat{I}_F
$$

#### detail replenishing module

![](https://gitee.com/shilongshen/image-bad/raw/master/20200613085221.png)

利用残差下采样卷积块对原图片$I_s$的appearance and style information进行提取，为了获得更加鲁棒性的表示，采用了adaptive average pooling将encoderd feature 转换为style code $z_s$,并使用3-layer FC,通过style code，来计算AdaIN的参数，假设第i层的参数为$s^i=(s_w^i,s_b^i)$,则AdaIN的计算为
$$
AdaIN(F_t^i,s^i)=s_w^i\frac{F_t^i-\mu_t^i}{\sigma_t^i}+s_b^i
$$


使用AdaIN将style information整合入相应的姿态信息中。注意到$z_s$在这里起到了style-guild的作用，其控制不同区域的合成细节。(从风格迁移的角度来看，此处将中间特征作为了content)

#### loss function

$$
\mathcal{L}_1=\lambda_{recon}\mathcal{L}_{recon}+\lambda_{per}\mathcal{L}_{per}
$$

其中$\mathcal{L}_{recon}$为L1损失，$\mathcal{L}_{per}$ 为感知损失。
$$
\mathcal{L}_2=\lambda_{recon}\mathcal{L}_{recon}+\lambda_{per}\mathcal{L}_{per}+\lambda_{sty}\mathcal{L}_{sty}+\lambda_{GAN}\mathcal{L}_{GAN}
$$
其中$\mathcal{L}_{sty}$为Gram-matrix based style loss。

$\mathcal{L}_{GAN}$使用了两个判别器$D_A$和$D_S$来分别判别外观和姿态。
$$
\mathcal{L}_{GAN}=\mathbb{E}[\log (D_A(I_s,\hat{I}_t))]+\mathbb{E}[\log (1-D_A(I_s,{I}_t))]+\mathbb{E}[\log (D_S(P_t,\hat{I}_t))]+\mathbb{E}[\log (1-D_S(P_t,{I}_t))]
$$


#### optimization 

首先使用$\mathcal{L}_1$对pose branch进行更新，以生成$\hat{I}_c$以及$F_t$，其中$F_t$与target pose 大致对齐。

之后进行细粒度挖掘。使用$\mathcal{L}_2$对detail replenishing branch进行更新，以生成$\hat{I}_t$.注意这一过程中pose branch也通过$F_t$进行更新。

最后判别器更新3次。（生成器更新1次，判别器更新3次）

### D. Experiments

#### A. Ablation Study

#### B. Comparisons with Previous Works

##### Qualitative Comparison 

定性实验评价

##### Perceptual Evaluation 

定量实验评价，评价指标：IS，SSIM，FID，LPIPS

这评价的是整个生成

##### Structural Evaluation

评价的 是各个身体部位的生成质量

##### Semantic Evaluation

For retrieval, we utilize generated images to query from the database of all corresponding source images, and calculate the retrieval scores using ground truth annotations provided in [24]. Specifically,
