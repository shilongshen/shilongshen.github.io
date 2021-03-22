# Deep Sketch-guided Cartoon Video Synthesis




该工作为根据提供的两张视频帧和草图，来来生成连续的视频帧。



![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201014162331.png)



1. 将粗糙的草图进行简化，提取出主要的结构

2. 建立草图与图像之间的语义对应关系：

   2.1 通过以两帧图片为条件的transformation module来将草图中的大部分空白区域进行填充，

   2.2 然后通过来两个独立的特征提取模块将图像和草图映射到一个common space

   2.3 为了解决遮挡问题，通过两张图像间光流来估计occlusion mask,并通过blending module使用mask来动态的从两帧之间挑选和结合像素。（合成与草图空间结构一样的视频帧）

3. 利用 arbitrary-time frame interpolation module生成任意中间的视频帧

4. 利用video post processing进一步改善结果

   

   #  Sketch Simplification and Generation

由于草图可能是粗糙且随意的，所以通过该模块来将草图多余的细节清除，只留下清晰的线条。采用的是现有的方法。

#  Sketch-guided Frame Synthesis

### 2.1 通过以两帧图片为条件的transformation module来将草图中的大部分空白区域进行填充：

 The transformer consists of several dilated residual

### 2.2 然后通过来两个独立的特征提取模块将图像和草图映射到一个common space

### 2.3 利用现有的光流预测模型预测光流，注意：预测的光流为双向光流：$f_{0-t}，f_{t-1}$。

损失函数：![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201014170410.png)










