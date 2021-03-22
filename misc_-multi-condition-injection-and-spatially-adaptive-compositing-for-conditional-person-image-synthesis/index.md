# MISC: Multi-condition Injection and Spatially-adaptive Compositing for Conditional Person Image Synthesis




该文章实现了不同背景下多条件人物图像生成。

该文提出了一个概念：图像生成（image generation）和图像合成（image compositing）

# Introduction

条件人物图像生成可以分为两个阶段：

- conditional generation phase : 在几何条件的基础上，生成人物的细粒度纹理
- adaptive compositing phase :   根据不同的背景调整生成人物的色调，使得生成的人物能够和背景更加的融合。

对于conditional generation phase，以三个条件作为输入：geometry、pattern（灰度纹理）和color，对应到具体的输入为parsing mask、Gaussian noise以及multi-valued color attributes。其中patteern 和 color是抽象的并且与geometry相关。因此条件人物图像生成可以视为抽象条件的视觉具体化。

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200906182946.png)

另一方面，adaptive compositing phase 需要将生成的人物图像更好的与背景相融合。

---

​	MISC包含两个模块：

- conditional person generation model
- spatially-adaptive image composition model

其中conditional person generation model ：

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200906183727.png)

对于pattern condition ，将它视为Gaussian noise，引入一个条件归一化层：GAIN，通过Gaussian 来调剂激活值。

对于color condition，使用bipartite network project method 来讲attribute 映射到人物相应的几何位置上，并使用预训练的cross-modality similarity model 来增强attribute embedding的语义意义。

---

而对于spatially-adaptive image composition model：

利用生成的人物图像和提供背景的图像，来为前景人物图像推断出每一个像素的颜色转换参数，

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200906185117.png)



# Conditional person image synthesis

conditional person generation model：
$$
\hat{y}=F^G(x)
$$
其中$x$表示输入的条件。生成的$\hat{y}$可以被解耦为$\hat{y}^f$和$\hat{y}^b$

spatially-adaptive image composition model：
$$
(\rho ,\tau)=F^C(\hat{y}^f,y^b)
$$
其中$y^b$是背景图像。

通过仿射参数$(\rho , \tau)$可以将$\hat{y}^f$与$y^b$进行更好的融合（将生成的人物图像与背景更好的融合）：
$$
y=m \odot y^f+(1-m)y^b
$$

## Conditional generation

$F^G$的输入$x$包括了geometry $x^g$、pattern $x^p$以及color $x^c$。

- $x^g \in \mathbb{L}^{N_g \times H \times W}$ 表示body part parsing mask 。其中$\mathbb{L}\in \{ 0,1\}$ 
- $x^p \sim \mathcal{N}(0,1)$ 表示Gaussian noise vector。
- ![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200906205410.png)
- $x^c \in \mathbb{L}^{N_c \times N_v}$ 表示multi-valued attributes, 其中$\mathbb{L}\in \{ 0,1\}$，$N_c$和$N_v$表示the number of attributes



$F^G$是多模块的叠加：

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200906204902.png)


$$
h_{i+1}=F^G_{i+1}(h_i,x_i^g,e^p,e^c)
$$
$F^G$的目标是实现抽象条件$x^p,x^c$的视觉具体化，为了实现这一目标需要实现合理的条件注入机制。

###  Pattern injection

**GAIN**：结合了AdaIN和SPADE的优势。希望GAIN能够在$x_g$的指导下自适应的控制不同身体部位的texture uniformity 。

令$\hat{h}=\in \mathbb{R}^{K\times \hat{H} \times \hat{W}}$ 表示AdaIN的输入特征，该特征被分为两个部分$k_1-th$和$k_2-th$，分别通过两组参数来控制：$\gamma[k_1],\beta[k_1]$以及$\gamma[k_2],\beta[k_2]$ 。如果这两个部分属于相同的身体部分，我们希望这两个部分有uniform textures，也就是有限制：
$$
(\gamma[k_1]\sim \gamma[k_2])\land (\beta[k_1]\sim\beta[k_2])
$$
注意到这种限制不应该影响到无关身体部位的激活值，所以一个限制应该以空间自适应的方式应用到每一个通道上。因此引入了两个空间自适应门：
$$
g^\gamma\in \mathbb{M}^{K \times \tilde{H} \times \tilde{W}}   \ for \ \gamma\in \mathbb{R}^K \\
g^\beta\in \mathbb{M}^{K \times \tilde{H} \times \tilde{W}} \ for \ \beta\in \mathbb{R}^K
$$
其中$\mathbb{M}\in [0,1]$,通过“门”操作将$\gamma$，$\beta$变为：
$$
\bar{\gamma}\in \mathbb{R}^{K \times \tilde{H} \times \tilde{W}} \\
\bar{\beta}\in \mathbb{R}^{K \times \tilde{H} \times \tilde{W}} 
$$


采用SPADE来计算$g^\gamma$和$g^\beta$。

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200907091753.png)

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200907091905.png)



### Color injection

为了减轻由uniform injection 带来的”learning burdens“，我们需要准备和注入一个spatially-specific attribute condition $e^{gc}\in \mathbb{R}^{K \times H \times W}$,这可以分为两个部分：

- 每一个身体部位专有属性嵌入，例如$e^c\in \mathbb{R}^{K \times N_g}$,其中$K$表示嵌入的维度，$N_g$表示身体部位的个数。
- geometry condition $x^g \in \mathbb{N }^{N_g\times H\times W}$ 表示身体部位和空间位置的相关性

随后可以通过bipartie network 获取$e^{gc}$ :  
$$
e^{gc}=e^c \otimes x^g
$$
通过在每一阶段将$e^{g,c}$和输入的特征相叠加来惊醒color injection。

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200907095742.png)

---

如何获取$e^c$ : 

- 根据二进制属性内在关联将二进制颜色属性变为multi-valued attributes $x^c\in \mathbb{L}^{N_c\times N_v}$

- 利用$Enc^C$ 将每一个multi-valued attributes编码为向量，$\hat{e}^c\in \mathbb{R}^{K\times N_c}$ 。

- 确定身体部位和属性间的关联矩阵 $A\in \mathbb{L}^{N_g\times N_c}  $

  ![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200907101318.png)



![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200907101422.png)






