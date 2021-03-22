# A Style-Based Generator Architecture for Generative Adversarial Networks




> 
>
> 参考：
>
> [参考1](https://blog.csdn.net/lynlindasy/article/details/89555201?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase)
>
> [参考2](https://blog.csdn.net/weixin_43013761/article/details/100973679?utm_medium=distribute.pc_relevant.none-task-blog-baidujs-4)



该文章基于GAN设计风格迁移模型。该模型能够自动学习，无监督的分离高层属性（例如 pose，identity）以及能对生成图像进行随机变化（例如雀斑，头发）并且 能够对生成图像的分辨率进行控制。

> StyleGAN中的style借用自图像风格迁移，这篇文章是PG-GAN之后的，主要改变了生成器的结构实现无监督地生成可控性强的图像。
>
> PG-GAN中是通过一层层地给生成器和判别器增添卷积层同时提高分辨率的方法来生成高质量图像的。StyleGAN的motivation就是发现这种渐进的方法其实能控制图像的不同特性，低分辨率也就是coarse层主要影响图像的姿态，脸型等高级特征，高分辨率主要影响背景发色等低级特征(结合感受野的概念理解，低分辨率的特征获取更多全局信息并加以处理，比如特征从线条经过多次卷积一步步会组合出形状，会更加抽象，具有更高级的语义信息)。但PG-GAN这种结构在递进添加层的时候没有任何控制条件，导致整体的特征和细微的特征间存在耦合，耦合就导致了图像可控性差，没办法对单个特征进行调节。StyleGAN于是寻找了一种无监督但又可控性强的方法：<u>对不同level的卷积层进行操作</u>。
>
> ProGAN的网络框架：
>
> ![](https://gitee.com/shilongshen/image-bad/raw/master/image/20190425182657412.png)
>
> ------
>
> 在之前的传统的生成网络中，有一个问题存在，就是特征耦合，那么这个是什么呢？如上图左边，传统的生成网络，我们可以看到，其输入一个latent *Z*
>
> Z，一般为512的一个向量。假设这个向量代表一个人的脸，那么该向量就会存在特征耦合。因为一个脸是有很多特征的，并不是一个512维的向量就能完全表示的，如果非要表示，只能是这种方式，如下：
>
> 假设：维度1代表头发粗细，维度2代表皮肤颜色，维度3代表鼻子大小…，当512个维度全部是用完之后，其只能通过多个维度再去表示其他的特征，如：维度1与维度2综合起来表示了头发的颜色。这样，通过两个或者多个组合，512的向量，就能表示出接近无数的特征。
>
> 但是这样就很明显出现了一个问题，那就是我们怎么去控制我们想要图片的单个特征呢？如，我只想改变头发的粗细，但是又不能直接去修改第一个维度，因为第一个维度会影响其他的维度，可能会影响到皮肤的颜色，但是有的时候，我偏偏需要该表他头发的颜色，不修改皮肤的颜色。**不同的特征是互相关联的（特征耦合）**。

#### 1 Introduction

GAN能够生成高分辨率和高质量的图像，但是生成器仍然被视为一个黑箱子。尽管最近已经有了一些工作，但是对图像生成过程中变化因素的理解，例如随机特征的起源，仍然是缺乏的。latent space的性质也没有很好的被理解。

该文设计了一个能够设计了一个能够控制生成过程的生成器模型。生成器的输入为一个learned constant，根据latent code在每一个卷积层调整图像的style，从而直接控制不同尺度下图像特征的强度。通过将noise直接送入网络中，来实现从随机变化（）中进行高层属性（）的自动、无监督分离，并实现直观的特定尺度的混合和插值操作。

生成器首先将input latent code映射到intermediate latent space，这对于factors of variation在网络中是如何表达到有很重要的影响。input latent code的分布（input latent space）是符合training data的概率密度的，这会导致某种程度上信息的耦合。而intermediate latent space是没有这一限制的，因此可以进行信息的解耦。该文提出了两种新的评估latent space解耦程度的方法：感知路径长度和线性可分离性。该文的方法具有更高的线性度和更少的耦合度。

并且提出了了一个新的dataset of human faces（Flickr-Faces-HQ, FFHQ）。

#### 2 style-based generator

传统的方法是将input code通过input layer送入生成器。该文抛弃了这个设计，完全忽略了输入层，而是从一个learned constant开始。给定latent space $\mathcal{Z}$ 中的一个latent code $z$，非线性映射网络 $f:\mathcal{Z \rightarrow W }$, 将$z$映射为$w$：$w=f(z)$.其中$f$ 为8层的MLP。随后将$w$变换为styles:$y=(y_s,y_b)$来作为AdaIN的仿射参数：
$$
AdaIN(x_i,y)=y_{(s,i)}{\large(}\frac{x_i-\mu(x_i)}{\sigma(x_i)}{\large)}+y_{(b,i)}
$$

> $$
> z\rightarrow w \rightarrow y (仿射参数)\rightarrow style\\
> x \rightarrow content
> $$
>
> $y_s$为$w$的方差，$y_b$为$w$的均值.

最后通过引入显示噪声输入，使生成器直接进行随机细节的生成。这些**噪声是不相关的单通道图像**，并且在生成器的每一层都输入一个专用噪声。利用学习后的perfeature缩放因子将噪声图像广播到所有的feature map上，然后将噪声图像加到相应卷积的输出中。

> 当latent code只用于生成器的输入层时，随着生成器的层数增多，它的影响应该会越来越微弱，无法对latent code进行解耦合。
>
> StyleGAN中，生成器的输入变成了一个常量，而latent code分别作用于生成器的每一个卷积层后，以控制图像的生成。这里有两个重点：
>
> 1. latent code作用于卷积层之前有一个变换，可以达到解耦和
>
> 2. 解耦合后的latent code作用于每一卷积层，借鉴PG-GAN的递进概念，这些层所影响/控制的特征是不同的，于是**latent code对不同level的特征分别进行了控制**。
>
>    ![](https://gitee.com/shilongshen/image-bad/raw/master/image/20200606085219.png)



#### 3 properties of style-based generator

生成器的结构使得能够通过特定尺度（生成不同分辨率的图像）的修改来控制合成图像风格。我们可以把映射网络和仿射变换看作是一种从已知分布（input latent space）中为每种风格进行采样的方法（**从$z$求得$w$,利用$w$计算仿射参数$y=(y_s,y_b)$,因为生成器中共有18种不同分辨率的图像，所以共有18组仿射参数，这18组仿射参数对应了不同的style，这里的style表示的是姿态，脸型以及背景，颜色等特征。**），而生成网络则基于styles生成新图像（**通过AdaIN控制风格迁移**）。每种风格的效果在网络中都是局部的，修改风格的特定子集只能影响图像的某些方面（不同的仿射参数只影响特定的特征）。

> 不同的仿射参数可以控制不同的风格，即一个仿射参数只能控制一种风格（局部(localize)的解释）。
>
> 生成器能够生成不同分辨率的图像，对不同分辨率的图像分别进行AdaIN控制。
>
> 从latent space采样并进行非线性映射和仿射变换的过程可以视作是从已学习到的分布中对某style图像采样，即每一层卷积后融入的style都是不同的，最后生成器相当于从这么多种styles的集合中生成一张新图像，<u>这种方法使得每一种style产生的影响都是局部的</u>(相当于把styles解耦和了，但是这里所谓的styles代表着什么呢？应该就是姿势，肤色到眼睛，嘴巴等特征吧)，这样就可以很方便地操控图像的各种特征了。
>
> **低分辨率（low-level）也就是coarse层主要影响图像的姿态，脸型等高级特征，高分辨率（high-level）主要影响背景发色等低级特征**(结合感受野的概念理解，低分辨率的特征获取更多全局信息并加以处理，比如特征从线条经过多次卷积一步步会组合出形状，会更加抽象，具有<u>更高级的语义信息</u>)

#### 3.1 style mixing

为了进一步加强styles的“localize”，应用了 mixing regularization，即在训练过程中使用两个随机的latent code（而不是一个latent code）来生成给定百分比的图像。我们在生成网络中随机选取一个点，来将一个latent code转换为另一个latent code（style mixing）。具体来说，给定两个input latent code  $z1,z2$，通过$f$求得：$w1=f(z1),w2=f(z2)$,$w1,w2$分别控制不同的风格，$w1$应用在crossover point之前，$w2$应用到crossover point之后。

> 一个分辨率的图像中使用了**两个AdaIN**进行控制，进而实现style mixing。
>
> 为了进一步实现style的解耦和，文中还采用了mixing regularization的操作：在训练时，用两个latent code生成一部分图像，也就是一张图像不是由一个latent code生成的各种style控制的，而是两个latent code。两个latent code会有两个非线性映射后得到的w_1,w_2，<u>在生成器中随机选取一个节点，该节点前使用w_1，节点后使用w_2</u>。该方法可以阻止相邻styles的相关(大概是因为不同的latent code非线性映射后得到的style差距比一个latent code自己产生的要大，所以肯定styles间的关联更小，本身就更不耦合？)，这里其实并不太懂，但通过实验的图像能看出latent code在不同层改变时确实影响的是很准确的不同的特征，说明style的解耦和还蛮成功的

![](https://gitee.com/shilongshen/image-bad/raw/master/image/20200605214908.png)

#### 3.2 stochastic variation

在人物肖像中有许多方面可以被认为是随机的（人物肖像细节的生成），比如头发、胡茬、雀斑或皮肤毛孔的确切位置。只要它们遵循正确的分布，它们中的任何一个都可以被随机化，而不影响我们对图像的感知。

在该文中通过添加per-pixel noise来完成这些细节的生成，同时整体的组成，如姿态和identity不变。同时，在不同层添加noise会完成不同的细节生成。

> StyleGAN生成器的右侧还加入了方块B，这表示噪声。StyleGAN中认为，加入随机的噪声可以帮助生成图像更多样化也更真实。比如头发部分，发丝的细节有很高的随机性，真实情况也确实如此，只要服从正确的分布，随机采样得到的结果仍然是合理的。
>
> 之前的生成器是如何保证生成图像的多样性的呢？之前生成器的输入只有一个，也就是说网络的变量其实只有这一个输入的latent code。要想具有多样性，只能在后续的操作中实现：需要时，从前一层的激活值中产生空间上有变化的伪随机数值。这种变化其实是具有一定周期性的(可能因为是伪随机吧，虽然我不太懂)，很难不重复，因此我们会看到生成图像中总是会有很多相似的图像。**StyleGAN则是在每一次卷积后给每个像素都加了噪声以实现多样性，而且效果很好，这个噪声只影响了需要多样性的局部，不改变图像关键的特征**，如下图所示： 
>
> ![](https://gitee.com/shilongshen/image-bad/raw/master/image/20200606085601.png)


