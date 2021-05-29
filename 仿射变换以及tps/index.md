# 仿射变换以及TPS




[参考](https://www.cnblogs.com/happystudyeveryday/p/10547316.html)

[参考](https://www.matongxue.com/madocs/244.html)

#### 仿射变换

Affine  Transformation是一种二维坐标到二维坐标之间的线性变换，保持二维图形的“平直性”（译注：straightness，即变换后直线还是直线不会打弯，圆弧还是圆弧）和“平行性”（译注：parallelness，其实是指保二维图形间的相对位置关系不变，平行线还是平行线）

仿射变换可以通过一系列的原子变换的复合来实现，包括：**平移（Translation）、缩放（Scale）、翻转（Flip）、旋转（Rotation）和剪切（Shear）**。

![](https://gitee.com/shilongshen/image-bad/raw/master/20200712110443.png)

仿射变换的矩阵表达式：
$$
\begin{bmatrix}x^s\\y^s\end{bmatrix}=\begin{bmatrix}\theta_{11}&\theta_{12}&\theta_{13}\\\theta_{21}&\theta_{22}&\theta_{23}\end{bmatrix}\begin{bmatrix}x^t\\y^t\\1\end{bmatrix}
$$



> [参考](https://www.zhihu.com/question/20666664)
>
> 仿射变换”就是：“线性变换”+“平移”
>
> <img src="https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210507103147.png" style="zoom:67%;" />
>
> <img src="https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210507103226.png" style="zoom:67%;" />
>
> <img src="https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210507103254.png" style="zoom:67%;" />
>
> 

#### 仿射变换与透视变换

https://zhuanlan.zhihu.com/p/36082864

![](https://gitee.com/shilongshen/image-bad/raw/master/20200712111740.png)

![](https://gitee.com/shilongshen/image-bad/raw/master/20200712111834.png)
