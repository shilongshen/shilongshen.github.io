# Unsupervised Pose Flow Learning for Pose Guided Synthesis




###### 主要的想法是利用光流法来捕获复杂的姿态变形。

> 基于光流的方法能够相比于仿射变换和薄板样条插值更加的灵活，能够解决复杂的变形问题

###### 为了解决遮挡问题，提出了一种无监督姿态流学习方法将外观转换到被遮挡的区域。

![](https://gitee.com/shilongshen/image-bad/raw/master/20200707215038.png)

![](https://gitee.com/shilongshen/image-bad/raw/master/20200707104503.png)

分为两个阶段：

阶段一：通过flow estimator估计多尺寸姿态流->表示的是原图像和目标图像之间姿态的运动关系（变形关系）

阶段二：基于多尺寸姿态流，对原图像进行多尺寸特征warped操作。使用coarse-to-fine的操作，coarse操作生成目标外观，fine操作进行精修，生成图像

![](https://gitee.com/shilongshen/image-bad/raw/master/20200707105814.png)

![](https://gitee.com/shilongshen/image-bad/raw/master/20200707105853.png)
