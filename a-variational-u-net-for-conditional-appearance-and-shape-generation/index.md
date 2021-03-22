# A Variational U-Net for Conditional Appearance and Shape Generation




该方法能够生成不同的姿态的人物图像以及改变人物的外观。而且这个模型能够在不改变shape的情况下从appearance distribution 中进行采样。

#### 1 Approach

记$x$为dataset $X$中的一张图片，我们想要理解$x$中的object是如何被其shape $y$和appearance $z$所影响的。因此图像生成器可以被表示为最大化后验概率（极大似然估计：给定$y$和$z$，哪种$x$最有可能发生。）
$$
arg\ max\ p(x|y,z)
$$

##### 1.1 VAE based on latent shape and appearance

$p(x|y,z)$可以视为隐变量(含两个隐变量)的生成模型，可以求得这个生成模型的联合概率分布$p(x,y,z)$。

$\because$
$$
p(x|y,z)=\frac{p(x,y,z)}{p(y,z)}
$$


$\therefore$
$$
p(x,y,z)=p(x|y,z)p(y,z)
$$
含隐变量的概率密度估计可以采用VAE的方法进行求解，求解过程包含了两个步骤推断和生成。实际上我们最终的目的是为了求出图像$x$的分布$p(x,\theta),\theta$为参数，给定样本$x$，其对数边际似然函数为
$$
\log p(x,\theta)=ELBO(q,x,\theta,\phi)+KL[q(y,z|x;\phi),p(y,z|x;\theta)]
$$
其中$q(y,z|x;\phi)$为变分密度函数，$\phi$为参数，ELBO为证据下界：
$$
ELBO(q,x,\theta,\phi)=\mathbb{E}_q\log \frac{p(x|y,z)p(y,z)}{q(y,z|x;\phi)}
$$

> $$
> \begin{aligned}
> \log p(x)&=\log \int p(x,y,z)dz\ dy\\
> &=\log \int \frac{p(x,y,z)}{q(y,z|x)}q(y,z|x)dz\ dy\\
> &\geq  \int q(y,z|x)\log \frac{p(x,y,z)}{q(y,z|x)}dz\ dy\\
> &= \mathbb{E}_q\log \frac{p(x,y,z)}{q(y,z|x;\phi)}\\
> &=\mathbb{E}_q\log \frac{p(x|y,z)p(y,z)}{q(y,z|x;\phi)}
> 
> 
> \end{aligned}
> $$
>
> 实际上
> $$
> \begin{aligned}
> \log p(x)&=\int q(y,z|x)\log \frac{p(x,y,z)}{q(y,z|x)}dz\ dy-\int q(y,z|x)\log \frac{p(y,z|x)}{q(y,z|x)}dz\ dy\\
> &=ELBO+KL[q(y,z|x)||p(y,z|x)]
> \end{aligned}
> $$
> 
>
> **VAE最终的优化目标为最大化ELBO**。
>
> 
>
> 从VAE的角度来说我们最终是要学习到图像的分布$p(x)$，但是直接学习到图像的分布式比较困难的，因此引入了两个隐变量$y,z$。我们的第一步就是从训练数据$\pmb{x}$中推断到隐变量$y,z$，即后验概率$p(y,z|\pmb{x})$。然后再从$y,z$中学习到图像的分布$p(x|y,z)$。



在VAE中假设$p(y,z)$服从标准正态分布,即$p(y,z)\sim \mathcal{N}(0,1)$。但是这种假设无法将变量$y$和$z$分离，因此我们需要添加一个额外的信息将$y,z$进行分离。

##### 1.2 CVAE with appearance

假设我们有一个估计函数$e$能够通过图像$x$来估计shape $y$，即$\hat{y}=e(x)$。那么$\hat{y}=e(x)$则通过最大化条件对数似然函数来得到：
$$
\begin{aligned}
\log p(x|\hat{y})&=\log \int _z p(x,z|\hat{y})dz\\
&=\log \int _z q(z|x,\hat{y})\frac{p(x,z|\hat{y})}{q(z|x,\hat{y})}dz\\
&\geq \int _z q(z|x,\hat{y})\log \frac{p(x,z|\hat{y})}{q(z|x,\hat{y})}dz\\

&= \mathbb{E}_q \log \frac{p(x,z|\hat{y})}{q(z|x,\hat{y})}\\
&=\mathbb{E}_q \log \frac{p(x|\hat{y},z)p(z|\hat{y})}{q(z|x,\hat{y})}
\end{aligned}
$$

> $$
> \because\\
> p(x,z|\hat{y})=\frac{p(x,\hat{y},z)}{p(\hat{y})}\\
> p(x|\hat{y},z)=\frac{p(x,\hat{y},z)}{p(\hat{y},z)}\\
> p(z|\hat{y})=\frac{p(\hat{y},z)}{p(\hat{y})}\\
> $$
>
> $$
> \therefore\\
> p(x,z|\hat{y})=p(x|\hat{y},z)p(z|\hat{y})
> $$
>
> 实际上
> $$
> \begin{aligned}
> p(x|\hat{y})&=p(x,z|\hat{y})\frac{1}{p(z|x,\hat{y})}\\
> \rightarrow\frac{p(x,\hat{y})}{p(\hat{y})}&=\frac{p((x,\hat{y},z))}{p(\hat{y})}\frac{p(x,\hat{y})}{p(x,\hat{y},z)}\\
> 则有\\
> \log p(x|\hat{y})&=\int q(z|x,\hat{y})[\log p(x,z|\hat{y})-\log p(z|x,\hat{y})]dz\\
> &=\int _z q(z|x,\hat{y})\log \frac{p(x,z|\hat{y})}{q(z|x,\hat{y})}dz-\int _z q(z|x,\hat{y})\log \frac{p(z|x,\hat{y})}{q(z|x,\hat{y})}dz \\
> &=ELBO+KL[p(z|x,\hat{y})||q(z|x,\hat{y})]
> \end{aligned}
> $$
> 

其中**条件（先验）概率$p(z|\hat{y})$可以从训练数据中估计得到**(通过网络E进行估计)，并能够获得shape 和 appearance之间的关系。例如一个正在跑步的人更有可能穿T恤而不是西装。

现在的问题为如何求概率$p(x|\hat{y},z)$以及概率$q(z|x,\hat{y})$。这里使用神经网络$G_\theta$来拟合$p(x|\hat{y},z)$以及使用神经网络$F_\phi$来拟合$q(z|x,\hat{y})$。

> $q(z|x,\hat{y})$表示如何从给定图像$x$中推断appearance $z$。
>
> $p(x|\hat{y},z)$表示如何从隐变量$z,\hat{y}$中采样得到图像$x$。

损失函数为:

> 此时想要最大化$p(x|\hat{y})$,可转换为最大化证据下界$ELBO=\mathbb{E}_q \log \frac{p(x|\hat{y},z)p(z|\hat{y})}{q(z|x,\hat{y})}$,由Jensen不等式的性质，当且仅当$q(z|x,\hat{y})=p(z|\hat{y})$时，$ELBO$最大。这通过最小化KL散度来保证。
>
> 而损失函数的第二项为重构损失函数。
>
> 实际上
> $$
> \begin{aligned}
> ELBO&=\int q(z|x,\hat{y})\log \frac{p(x|\hat{y},z)p(z|\hat{y})}{q(z|x,\hat{y})}dz\\
> &=\int q(z|x,\hat{y})\log p(x|\hat{y},z)dz+\int q(z|x,\hat{y})\log \frac{p(z|\hat{y})}{q(z|x,\hat{y})}dz\\
> &=\mathbb{E}_{q_\phi(z|x,\hat{y})}[\log p_\theta(x|\hat{y},z)]-KL[q_\phi(z|x,\hat{y})||p_\theta(z|\hat{y})]
> \end{aligned}
> $$
> 最终的目标为最大化ELBO。

$$
\mathcal{L}(x,\theta,\phi)=-KL[q_\phi(z|x,\hat{y})||p_\theta(z|\hat{y})]+\mathbb{E}_{q_\phi(z|x,\hat{y})}[\log p_\theta(x|\hat{y},z)]
$$
![](https://gitee.com/shilongshen/image-bad/raw/master/image/20200603110728.png)

##### 1.3 Generator

首先建立$G_\theta$。假设分布$p(x|\hat{y},z)$有常值偏差，以及$G_\theta(\hat{y},z)$为$\hat{y}$的确定函数。那么$G_\theta(\hat{y},z)$可以视为图像生成器，我们可以将损失函数的第二项进行替代，那么此时的损失函数为：
$$
\mathcal{L}(x,\theta,\phi)=-KL[q_\phi(z|x,\hat{y})||p_\theta(z|\hat{y})]+\sum_k\lambda_k||\Phi_k(x)-\Phi_k(G_\theta(\hat{y},z)||_1
$$
其中$\sum_k\lambda_k||\Phi_k(x)-\Phi_k(G_\theta(\hat{y},z)||_1$为感知损失函数，$\phi$为VGG19。

我们想要保留$\hat{y}$中的空间信息用于生成图片$\bar{x}$（其中$\hat{y}=e(x)$使用现有的姿态估计算法），因此这里使用了U-net结构来进行$\hat{y}$信息的保留。

appearance $z$是在高斯分布$q_\phi(z|x,\hat{y})$中采样得到的(  $q_\phi(z|x,\hat{y})$的分布形式已知，但是分布参数未知)，我们通过神经网络$F_\phi$来求参数$\phi$。$F_\phi$的优化包含两个部分：1.需要将足够多的关于$x$的信息融入到$z$中以至于$p(x|\hat{y},z)$能够很好的描述数据。2.需要通过最小化$q(z|x,\hat{y})$和$p(z|\hat{y})$的KL散度来减小先验分布$p(z|\hat{y})$的偏差（KL散度目的为学习到a shape invariant representation of theappearance）。

由于$G_\theta$中已经使用U-net来保存$\hat{y}$中的空间信息了，因此不需要在$z$中加入关于shape 的信息了（如果加入了shape 信息只会提高计算量，而不会为$p(x|\hat{y},z)$提供有用的信息）。在这种情况下，只需要将$z$包含在$G_\theta$的瓶颈处即可。

我们将$z$和$E_\theta(\hat{y})$进行叠加作为decoder 的输入，即$\gamma=[E_\theta(\hat{y}),z]$，$D_\theta(\gamma)$.

#### 2 experiment

##### 2.1 image reconstruction

给定图像$x$和其shape $\hat{y}$，我们能够利用$F_\phi$来推断$x$的appearance $z$。认为利用$F_\phi$生成分布$q_\phi(z|x,\hat{y})$的平均值为原始外观。然后再利用$\hat{y},z$来对$x$进行重构。

##### 2.2 Appearance sampling

该模型 主要优点是以$\hat{y}$为条件下生成不同appearance 的人物图像。**实现的方法为从先验分布$p(z|\hat{y})$中随机采样**$z$，而不是通过$F_\phi$来推断。因此**可以实现在shape 不变而appearance 改变**。

##### 2.3 Independent transfer of shape and appearance

通过对shape 和appearance 的解耦表示（能够自由改变其中一个特征而不影响另一个特征），能够实现在不同的shape上有同一appearance，也能够实现通过同一shape有不同appearance（与2.2相同）。
