# group convolution.














[参考](https://zhuanlan.zhihu.com/p/65377955)

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201212101749.png)![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201212101813.png)

第一张图代表标准卷积操作。若输入特征图尺寸为 ![[公式]](https://www.zhihu.com/equation?tex=H+%5Ctimes+W+%5Ctimes+c_1) ，卷积核尺寸为 ![[公式]](https://www.zhihu.com/equation?tex=h_1+%5Ctimes+w_1+%5Ctimes+c_1) ，输出特征图尺寸为 ![[公式]](https://www.zhihu.com/equation?tex=H+%5Ctimes+W+%5Ctimes+c_2) ，标准卷积层的参数量为： ![[公式]](https://www.zhihu.com/equation?tex=%5Cbm%7B%28h_1+%5Ctimes+w_1+%5Ctimes++c_1%29+%5Ctimes+c_2%7D) 。*（一个滤波器在输入特征图 ![[公式]](https://www.zhihu.com/equation?tex=h_1+%5Ctimes+w_1+%5Ctimes+c_1) 大小的区域内操作，输出结果为1个数值，所以需要 ![[公式]](https://www.zhihu.com/equation?tex=c_2+) 个滤波器。）*

第二张图代表分组卷积操作。将输入特征图按照通道数分成 ![[公式]](https://www.zhihu.com/equation?tex=g) 组，则每组输入特征图的尺寸为 ![[公式]](https://www.zhihu.com/equation?tex=H+%5Ctimes+W+%5Ctimes%28+%5Cfrac+%7Bc_1%7D%7Bg%7D%29) ，对应的卷积核尺寸为 ![[公式]](https://www.zhihu.com/equation?tex=h_1+%5Ctimes+w_1+%5Ctimes+%28%5Cfrac%7Bc_1%7D%7Bg%7D%29) ，每组输出特征图尺寸为 ![[公式]](https://www.zhihu.com/equation?tex=H+%5Ctimes+W+%5Ctimes+%28%5Cfrac+%7Bc_2%7D%7Bg%7D%29) 。将 ![[公式]](https://www.zhihu.com/equation?tex=g) 组结果拼接(concat)，得到最终尺寸为 ![[公式]](https://www.zhihu.com/equation?tex=H+%5Ctimes+W+%5Ctimes+c_2)  的输出特征图。分组卷积层的参数量为 ![[公式]](https://www.zhihu.com/equation?tex=h_1+%5Ctimes+w_1+%5Ctimes+%28%5Cfrac%7Bc_1%7D%7Bg%7D%29+%5Ctimes+%28%5Cfrac%7Bc_2%7D%7Bg%7D%29+%5Ctimes+g+%3D+%5Cbm%7Bh_1+%5Ctimes+w_1+%5Ctimes+c_1+%5Ctimes+c_2++%5Ctimes+%5Cfrac%7B1%7D%7Bg%7D%7D) 。

**深入思考一下，常规卷积输出的特征图上，每一个点是由输入特征图 ![[公式]](https://www.zhihu.com/equation?tex=h_1+%5Ctimes+w_1+%5Ctimes+c_1) 个点计算得到的；而分组卷积输出的特征图上，每一个点是由输入特征图 ![[公式]](https://www.zhihu.com/equation?tex=h_1+%5Ctimes+w_1+%5Ctimes+%28%5Cfrac%7Bc_1%7D%7Bg%7D%29) 个点计算得到的。自然，分组卷积的参数量是标准卷积的 ![[公式]](https://www.zhihu.com/equation?tex=1%2Fg) 。**

> 将输入特征图沿着通道方向进行划分，分割成不同组，每一组分别采用一个卷积和进行卷及操作。
>
> 输入特征图的大小、输出特征图的大小和标准的卷积一样，指示卷积核的参数整体减小了。





## 深度可分离卷积（Depthwise separable convolution）

这张图怎么少的了呢：

![img](https://pic3.zhimg.com/80/v2-3060b36fe063bbfe99622be4a4b23a02_720w.jpg)

图(a)代表标准卷积。假设输入特征图尺寸为 ![[公式]](https://www.zhihu.com/equation?tex=D_F+%5Ctimes+D_F+%5Ctimes+M) ，卷积核尺寸为 ![[公式]](https://www.zhihu.com/equation?tex=D_K+%5Ctimes+D_K+%5Ctimes+M) ，输出特征图尺寸为 ![[公式]](https://www.zhihu.com/equation?tex=D_F+%5Ctimes+D_F+%5Ctimes+N)，标准卷积层的参数量为： ![[公式]](https://www.zhihu.com/equation?tex=%5Cbm%7B%28D_K+%5Ctimes+D_K+%5Ctimes++M%29+%5Ctimes+N%7D) 。

图(b)代表深度卷积，图(c)代表逐点卷积，两者合起来就是深度可分离卷积。深度卷积负责滤波，尺寸为(DK,DK,1)，共M个，作用在输入的每个通道上；逐点卷积负责转换通道，尺寸为(1,1,M)，共N个，作用在深度卷积的输出特征映射上。

深度卷积参数量为 ![[公式]](https://www.zhihu.com/equation?tex=%5Cbm%7B%7B%28D_K+%5Ctimes+D_K+%5Ctimes++1%29+%5Ctimes+M%7D%7D) ，逐点卷积参数量为 ![[公式]](https://www.zhihu.com/equation?tex=%5Cbm%7B%281%C3%971%C3%97M%29%C3%97N%7D) ，所以深度可分离卷积参数量是标准卷积的 ![[公式]](https://www.zhihu.com/equation?tex=%5Cfrac%7BD_K%C3%97D_K%C3%97M%2BM%C3%97N%7D%7BD_K%C3%97D_K%C3%97M%C3%97N%7D+%3D+%5Cfrac%7B1%7D%7BN%7D+%2B+%5Cfrac%7B1%7D%7BD_K%5E2%7D++) 。

**为了便于理解、便于和分组卷积类比，假设 ![[公式]](https://www.zhihu.com/equation?tex=M%3DN) 。深度卷积其实就是 ![[公式]](https://www.zhihu.com/equation?tex=g%3DM%3DN) 的分组卷积，只不过没有直接将 ![[公式]](https://www.zhihu.com/equation?tex=g) 组结果拼接，所以深度卷积参数量是标准卷积的 ![[公式]](https://www.zhihu.com/equation?tex=1%2FN) 。逐点卷积其实就是把![[公式]](https://www.zhihu.com/equation?tex=g) 组结果用** ![[公式]](https://www.zhihu.com/equation?tex=1%5Ctimes1+) **conv 拼接起来，所以逐点卷积参数量是标准卷积的** ![[公式]](https://www.zhihu.com/equation?tex=1+%2F+%7BD_K%7D%5E2) **。(\*只考虑逐点卷积，之前输出的特征图上每一个点是由输入特征图 ![[公式]](https://www.zhihu.com/equation?tex=D_K+%5Ctimes+D_K+) 区域内的点计算得到的；而逐点卷积输出上每一个点是由 ![[公式]](https://www.zhihu.com/equation?tex=1%5Ctimes1) 区域内的点计算得到的)。\*自然，深度可分离卷积参数量是标准卷积的 ![[公式]](https://www.zhihu.com/equation?tex=%7B1%7D%2F%7BN%7D+%2B+%7B1%7D%2F%7BD_K%5E2%7D) 。**
