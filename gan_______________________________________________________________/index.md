# GAN训练中生成器损失函数的变化应该是怎么样的？


[参考](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=8476290)

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200917215601.png)

<img src="https://gitee.com/shilongshen/image-bad/raw/master/img/20200917215624.png" style="zoom:150%;" />

注意到图c，此时生成器的损失函数是平稳上升的，训练过程是稳定的（在实际的GAN训练过程中，生成器的损失函数是上升的，判别器的损失函数是下降的？还是生成器和判别器的损失函数最终都是下降的？）从该论文的观点来看似乎是前者是正确的。



但是可以肯定的是，如果生成器的损失函数出现了a，b的情况就表示GAN的训练过程是不稳定的。



---

# DCGAN验证实验

[code](https://github.com/shilongshen/DCGAN/tree/master)

## 实验1：没有进行谱归一化

训练过程中损失函数的变化：

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200918164822.png)



真实图像和生成图像的对比：

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200919203402.png)

从损失函数的变化来看，生成器的损失函数是逐渐增大的，而判别器的损失函数似乎并没有发生多大的变化。从生成图像来看，生成的图像虽然不太逼真，但是似乎并没有发生模型崩溃的情况，似乎可以认定训练过程是稳定的。




















