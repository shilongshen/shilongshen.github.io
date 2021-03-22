# 可控人物图像生成综述




> 
>
> 
>
> 1.姿态表示的错误
> 2.由于身体自遮挡造成的姿态表示的模棱两可
> 3.稀少的姿态
> 4.稀少的外观
> 5.
>
> 从2D的层面来说肢体的遮挡是无可避免的

姿态引导下任务图像生成是计算机视觉领域一个热门的研究方向。该任务的目标为在目标姿态的引导下，在保留源图像外观的基础上进行人物姿态的转变。姿态引导下人物图像生成研究具有广泛的应用价值：例如图像编辑、电影制作、AR技术、虚拟换装、以及数据增强等。在这些应用中，用户通常会关注语义和细节更加丰富的部分，例如脸部和服装的细节。因此对语义信息进行保留以及对细粒度外观特征进行补充对于姿态引导下图像生成模型具有重要的意义。

姿态引导下的图像生成研究大致可以分为三种方法：全局预测方法、局部映射方法以及混合方法

<img src="https://gitee.com/shilongshen/image-bad/raw/master/20200615210534.png"  />

#### 全局预测方法

受到图像翻译工作的启发，早期的姿态引导下图像生成研究通常采用了全局预测的方法来直接将源图像姿态直接转换为目标姿态。例如文献【】中提出的模型中采用了具有跳跃连接的U-net进行图像的生成，其中跳跃连接用于前向传递解码特征给编码器。但是这种基于全局预测的方法缺乏捕获相应局部特征的能力，这会导致生成图像中细节的缺失，例如生成平滑的服装或者是变形的姿态。究其原因在于U-net首先文献【】被提出用于解决图像分割问题，其原域的图像与目标域的图像的局部结构是对齐的，解码器的特征映射与编码器的特征映射具有相同的空间分布，属于强约束。而对于姿态转换问题，由于人体非刚性的特点，（由于视角变化或者是姿态变化）输入图像和目标图像具有不同的空间位置，其特征映射是未对齐的，对于未对齐的特征映射则不属于强约束。因此直接使用U-net的跳跃连接只能够为解码器提供未对齐的特征映射，这对于进行姿态转换并不能取得很好的效果。

![](https://gitee.com/shilongshen/image-bad/raw/master/20200615201923.png)

> 具体解释：
>
> <img src="https://gitee.com/shilongshen/image-bad/raw/master/20200615204436.png"  />
>
> 假设U-net的结构如上，其中Encoder中进行的是的是源图像$I_s$和目标姿态$P_t$的信息融合，所以Encoder中的特征Encoder feature表示的是源姿态$P_s$和目标姿态$P_t$的一种融合信息（当然还包括了源图像的外观），而latent code可以理解为源图像的姿态已经转变为目标姿态（保留了源图像的外观），因此latent code可以视为生成$I_g$.(需要经过Decoder编码)。所以Encoder feature和latent code（可以理解为Decoder feature）在局部是不对齐的。
>
> 思考一下U-net中的跳跃连接的作用是什么：是为了前向传递Encoder feature，这能够有效的保留源图像中的信息。（保留图像中的局部信息，保留细节）
>
> 但是在姿态引导下的图像生成研究中，前向传递的特征是不对齐的，这会导致导致模型无法很好的捕获局部的信息，使得生成的图像细节的缺失，例如生成平滑的服装或者是变形的姿态



#### 局部映射方法

针对全局预测方法存在的缺陷，文献【】提出了一种改进的特征融合机制，对前向传递的解码器特征进行**仿射变换**后再传递给编码器。受到该方法的启发，有许多的研究人员采用了局部映射方法来进行姿态引导下的图像生成研究进行细粒度挖掘，例如例如引入**薄板样条函数**来进一步促进了非线性局部映射、引入了**局部注意力机制**增强了在局部映射建模中的灵活性、除此之外还有**光流法**以及**3D 建模法**等。由于局部映射方法往往需要对应的精确特征来进行细节重构，然而在实际的情况中，图像中人物往往由于视角的变化、遮挡或者姿态变化出现不可见的区域，这会导致源图像和目标图像中的某些部分区域无法对应，这些无法对应的部分就无法使用局部映射方法，这会导致相应部分的生成内容不确定。

> 常用的局部映射方法：仿射变换，薄板样条，局部注意力机制，光流法，3D建模
>
> 仿射变换和薄板样条不够灵活
>
> 3D建模的计算量太大

#### 混合方法

针对局部映射方法存在的缺陷，混合方法尝试利用另外一个全局预测分支在不对应部分生成新的内容，并与局部映射方法生成的内容相结合。但是现有的混合方法只是尝试在图像级别将生成结果进行混合，中间层的特征融合很少被研究。具体来说全局预测分支通常独立于局部映射方法，这使得两者生成的内容在风格上是不一致的。

