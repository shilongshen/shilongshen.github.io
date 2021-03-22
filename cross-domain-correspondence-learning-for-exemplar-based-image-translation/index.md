# Cross-domain Correspondence Learning for Exemplar-based Image Translation




**该文提出了一种新颖的图像翻译网络框架**。



#### cross-domain correspondence network：

网络的输入为style input和content input，由于style input和content input在结构上是不对齐的，为了建立style input和content input之间的对应关系，提出了一种cross-domain correspondence network,如图所示：

![](https://gitee.com/shilongshen/image-bad/raw/master/20200727173016.png)

该部分首先通过特征金字塔网络提取多尺度图像特征（这能够利用到局部和全局的图像内容），之后将提取到的多尺度特征通过下采样网络映射到相同的域$S$中（只有当$x_s$和$y_s$在同一个域时才能用某种相似性度量方法进行匹配（对齐））。数学表达式如下：
$$
x_s=\mathcal{F}_{A\rightarrow S}(x_A;\theta_{A\rightarrow S}) \quad x_s\in\mathbb{R}^{HW \times C}\\
y_s=\mathcal{F}_{B\rightarrow S}(x_B;\theta_{B\rightarrow S}) \quad y_s\in\mathbb{R}^{HW \times C}
$$
接下来使用**空间自注意力机制**操作计算$x_s$与$y_s$间pixel与pixel之间的相关性，如下图所示:

<img src="https://gitee.com/shilongshen/image-bad/raw/master/20200727174034.png" style="zoom: 200%;" />





#### translation network：

cross-domain correspondence network的输出视为将content input **warping** 之后的结果，translation network采用了类似style GAN的网络结构，如下图所示：

![](https://gitee.com/shilongshen/image-bad/raw/master/20200727175439.png)

与style GAN不同的是，这里将AdaIN模块进行了替换，替换的模块为SPADE block与PN的结合，表达式为：
$$
\alpha _{h,w}^i(r_{y \rightarrow x}) \times\frac{F_{c,h,w}^i-\mu_{h,w}^i}{\sigma_{h,w}^i}+\beta_{h,w}^i(r_{y \rightarrow x})
$$
$F_{c,h,w}^i$为输入值，注意此处的统计特征$\mu_{h,w}^i$和$\sigma_{h,w}^i$是在空间方向进行统计的，目的是为了保存$r_{y \rightarrow x}$的结构信息。



#### 损失函数：

该文采用了无监督的训练方式。



#### 总的网络结构：

![](https://gitee.com/shilongshen/image-bad/raw/master/20200727175816.png)


