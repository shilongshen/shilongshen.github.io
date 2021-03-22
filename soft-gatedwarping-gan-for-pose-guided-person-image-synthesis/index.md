# Soft-GatedWarping-GAN for Pose-Guided Person Image Synthesis




![](https://gitee.com/shilongshen/image-bad/raw/master/20200707165658.png)



target parsing:在特定的segmentation 区域进行纹理的渲染

warping block ：align the image features

> 通过**仿射变换**和**TSP**计算出transformation map用于warping condition image以减缓姿态不对齐问题

------

![](https://gitee.com/shilongshen/image-bad/raw/master/20200707164159.png)

第一阶段生成目标姿态的part segmentation map

第二阶段首先训练geometric matcher 来估计condition segmentation和synthesized segmentation的transformation 参数。基于这个参数将condition image 的feature map进行warping，并渲染到target segmentation map

![](https://gitee.com/shilongshen/image-bad/raw/master/20200707164926.png)

warping GAN的好处：1）如果存在大的姿态变换就会进行大的transformation，小的姿态变形就会进行小的transformation

2）在feature-level进行warping，能够合成更加真实的图片

3）warping block 能够通过attention layers自动选择有效的feature map 进行warping

------

Stage I: Pose-Guided Parsing

![image-20200707183807443](C:\Users\HIT\AppData\Roaming\Typora\typora-user-images\image-20200707183807443.png)

为了在part-level上学习condition image到target pose之间的映射，引入了pose-guide parser.![](https://gitee.com/shilongshen/image-bad/raw/master/20200707184229.png)

------

![](https://gitee.com/shilongshen/image-bad/raw/master/20200707184959.png)

###### Geometric Matcher

![](https://gitee.com/shilongshen/image-bad/raw/master/20200707185450.png)

将仿射变换和TPS相结合，通过孪生卷积神经网络来获得transformation map。

首先估计condition segmentation 和synthesized segmentation之间的仿射参数，基于仿射变换参数，估计经过仿射变换后的warping result和target parsing之间的TPS参数。

提取出来的transformation map用于warping条件图像的feature，减缓不对齐问题。

![](https://gitee.com/shilongshen/image-bad/raw/master/20200707190116.png)

###### Soft-gatedWarping-Block

![](https://gitee.com/shilongshen/image-bad/raw/master/20200707205604.png)

![](https://gitee.com/shilongshen/image-bad/raw/master/20200707205708.png)

------

网络结构

![](https://gitee.com/shilongshen/image-bad/raw/master/20200707210653.png)

------

消融实验

![](https://gitee.com/shilongshen/image-bad/raw/master/20200707211414.png)
