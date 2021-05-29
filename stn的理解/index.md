# STN的理解


![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9naXRlZS5jb20vc2hpbG9uZ3NoZW4vaW1hZ2UtYmFkL3Jhdy9tYXN0ZXIvaW1nLzIwMjAwNzExMTY1MzQzLnBuZw?x-oss-process=image/format,png)

#### 网络结构

网络分为三个部分：Localisation net、Grid generator以及Sampler

输入为feature map  $U$，输出为feature map $V$。

#### Localisation net

以$U$为输入，经过卷积层或者全连接层输出仿射参数$\theta \in (2,3)$

假设$\theta=\begin{bmatrix}\theta_{11}&\theta_{12}&\theta_{13}\\ \theta_{21}&\theta_{22}&\theta_{23}  \end{bmatrix}$

- [ ] 在编程实现中最好将$\theta$进行初始化为$\begin{bmatrix}1&0&0\\0&1&0\end{bmatrix}$

#### Grid generator

已知$V$的坐标，使用仿射变换求出$U$中对应的坐标。

- [ ] 在编程实现中需要预先定义$V$的shape, pytorch会根据shape的大小自动初始化一个坐标

  假设$V$的坐标为$\begin{bmatrix}x^t\\y^t\\1 \end{bmatrix}$ ，$U$中对应的坐标为$\begin{bmatrix}x^s\\y^s \end{bmatrix}$。则两者之间的关系为
  $$
  \begin{bmatrix}x^s\\y^s \end{bmatrix}=\begin{bmatrix}\theta_{11}&\theta_{12}&\theta_{13}\\ \theta_{21}&\theta_{22}&\theta_{23}  \end{bmatrix}\begin{bmatrix}x^t\\y^t\\1 \end{bmatrix}
  $$

> 注意此处的目标坐标和原坐标的对应关系，这里使用的是图像变换中的**向后映射**，[参考5](https://blog.csdn.net/glorydream2015/article/details/44873703)
>
> **图像变换的本质是将像素点的坐标通过一种函数关系映射到另外的位置，并根据对应关系进行采样**
>
> **向前映射**：已知原图像上的坐标，并且已知原图像坐标到目标图像坐标的映射关系，因此可以求得原图像上一点经过映射后在目标图像上的位置。
>
> **向后映射**：已知目标图像上的坐标，并且已知目标图像坐标到原图像坐标的映射关系，因此可以求得目标图像上一点经过映射后在原图像上的位置。
>
> ------
>
> **插值**：通常情况下一个整数的坐标$(x,y)$经过映射后往往都位于非整数位置，此时就要采用插值方法进行采样。（此处以双线性插值为例）
>
> **针对前向映射的插值方法**：当输入图像上的整数坐标$(x,y)$经过映射后，对应的目标图像上的坐标$(x',y')$为非整数，需要将其像素值按照一定的权重分配到其周围的四个像素点上。因此输出图像上的整数坐标的像素值是由多个输入图像像素点映射并按照一定权重叠加的结果。
>
> ![](https://gitee.com/shilongshen/image-bad/raw/master/20200712163041.png)
>
> **针对后向映射的插值方法**：当输出图像上的整数坐标$(x',y')$经过映射后，对应的原图像上的坐标$(x,y)$为非整数，利用其周围整数点位置的输入图像像素值进行插值，就得到了该点的像素值。我们遍历输出图像，经过坐标变换、插值两步操作，我们就能将其像素值一个个地计算出来，因此向后映射又叫图像填充映射。如下图所示。
>
> ![](https://gitee.com/shilongshen/image-bad/raw/master/20200712163513.png)







#### Sampler

由于计算得到的$\begin{bmatrix}x^s\\y^s \end{bmatrix}$为浮点数时会造成无法进行梯度传播，因此使用双线性插值进行采样：

<img src="https://gitee.com/shilongshen/image-bad/raw/master/img/20200711173807.png" style="zoom: 200%;" />

其中$U_{nm}^c$表示距离$(x^s,y^s)$最近的四个坐标所对应的$U$中的像素值，$V_i^c$表示使用双线性插值后$V$中某点像素值。



#### 反向传播

前向的传递过程为$U\rightarrow^{}$**Localisation net**  $\rightarrow^{\Large\theta}$  **Grid generator**  $\rightarrow^{\Large\begin{bmatrix}x^s\\y^s\end{bmatrix}}$   **Sampler**   $\rightarrow^{\Large V_i^c}$

- [ ] 因此反向传播的过程需要先计算出$V_i^c$关于$\begin{bmatrix}x^s\\y^s\end{bmatrix}$的梯度，再计算出$\begin{bmatrix}x^s\\y^s\end{bmatrix}$关于$\theta$的梯度。





<img src="https://gitee.com/shilongshen/image-bad/raw/master/img/20200711175159.png" style="zoom:200%;" />

同理可以计算出$V_i^c$关于$y^s$的导数，以及$\begin{bmatrix}x^s\\y^s\end{bmatrix}$关于$\theta$的导数（这个比较简单）



------

**通过以上的前向传播以及反向传播可以学习到仿射参数$\theta$，进而通过双线性插值得到经过warp后的特征。**



[参考1](https://blog.csdn.net/qq_39422642/article/details/78870629)

[参考2](https://www.cnblogs.com/liaohuiqiang/p/9226335.html)

[参考3](https://www.jianshu.com/p/723af68beb2e)

[参考4](https://github.com/kevinzakka/spatial-transformer-network)

[参考5](https://blog.csdn.net/glorydream2015/article/details/44873703)
