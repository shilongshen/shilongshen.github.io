# hinge loss




[参考](https://blog.csdn.net/hustqb/article/details/78347713)

**Hinge loss专用于二分类问题**，标签值*y*=±1，预测值$\hat{y}\in R$。该二分类问题的目标函数的要求如下： 
  当$\hat{y}$大于等于+1或者小于等于-1时，都是分类器确定的分类结果，此时的损失函数loss为0；而当预测值$\hat{y}\in (-1,1)$时，分类器对分类结果不确定，loss不为0。显然，当时$\hat{y}=0$,loss达到最大值。

对于输出**y**=**±1**，当前$\hat{y}$的损失为**： 
$$
\mathcal{L}=max(0,1-y \cdot \hat{y})
$$



 上式是Hinge loss在二分类问题的的变体，可以看做双向Hinge loss。难以理解的话，可以先看单方向的hinge loss。以y=+1，为例。当*y*⩾1时，loss为0，否则loss线性增大。函数图像如下所示： 



![Hinge函数图像](https://img-blog.csdn.net/20171025220245568?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaHVzdHFi/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast) 
