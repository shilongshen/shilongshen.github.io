# 关于深度生成模型的再次思考




深度生成模型的目的分为两个，第一个是通过在未知分布中采集一些样本，利用这些已知的样本结果信息来反推出最有可能导致这些样本结果出现的模型参数，这一步称为概率密度估计。第二步是在第一步中已学习到的概率分布中进行采样（即生成样本）。

在概率密度估计中，假设采集的样本为$\pmb{x}=\{ x_1,x_2...x_n \}$,假设这些样本服从一个概率密度$p(\pmb{x};\theta)$，其中$\theta $为待估计的概率密度的参数。采用极大似然法进行估计，则对数似然函数为
$$
\log p(\pmb{x};\theta)=\sum_{i=1}^n\log p(x^i;\theta)
$$
这样参数估计问题就转化为最优化问题：
$$
{\theta}^{ML}
=arg \max\limits_{\theta}\sum_{i=1}^n\log p({x}^{(i)};\theta)
$$
在机器学习中，密度估计是一类无监督学习问题．比如在手写体数字图像的密度估计问题中，我们将图像表示为一个随机向量𝑿，其中每一维都表示一个像素值．假设手写体数字图像都服从一个未知的分布$p_r(\pmb{x})$，希望通过一些观测样本来估计其分布。但是，手写体数字图像中不同像素之间存在复杂的依赖关系（比如相邻像素的颜色一般是相似的），很难用一个明确的图模型来描述其依赖关系，所以直接建模$p_r(\pmb{x})$ 比较困难．因此，我们通常通过引入隐变量𝒛来简化模型。如果要建模含隐变量的分布，就需要利用EM算法来进行密度估计。

给定样本$\pmb{x}=\{ x_1,x_2...x_n \}$，因为$p(\pmb{x,z};\theta)=p(\pmb{z|x};\theta)p(\pmb{x};\theta)$，所以有$\log p(\pmb{x,z};\theta)=\log p(\pmb{z|x};\theta)+\log p(\pmb{x};\theta)$，进而有$\log p(\pmb{x};\theta)=\log p(\pmb{x,z};\theta)-\log p(\pmb{z|x};\theta)$。引入变分分布概率密度$q(\pmb{z};\phi)$，有$\int q(\pmb{z};\phi)dz=1  $,其中$\phi$为变分分布的参数。则概率密度$p(\pmb{x};\theta)$的对数似然函数可以转换为：
$$
\begin{aligned}
\log p(\pmb{x};\theta)&=\int q(\pmb{z};\phi)\log p(\pmb{x};\theta)dz\\
&=\int q(\pmb{z};\phi)[\log p(\pmb{x,z};\theta)-\log p(\pmb{z|x};\theta)]dz\\
&=\int q(\pmb{z};\phi)\log p(\pmb{x,z};\theta)dz-\int q(\pmb{z};\phi)\log q(\pmb{z};\phi)dz-\int q(\pmb{z};\phi)\log (\pmb{z|x};\theta)dz+\int q(\pmb{z};\phi)\log q(\pmb{z};\phi)dz\\
&=\int q(\pmb{z};\phi)[\log p(\pmb{x,z};\theta)-\log q(\pmb{z};\phi)]dz-\int q(\pmb{z};\phi)[\log p(\pmb{z|x},\theta)-\log q(\pmb{z};\phi)]dz\\
&=\int q(\pmb{z};\phi)\log\frac{ p(\pmb{x,z};\theta)}{ q(\pmb{z};\phi)}dz-\int q(\pmb{z};\phi)\log \frac{p(\pmb{z|x};\theta)}{\log q(\pmb{z};\phi)}dz\\
&=ELBO(q,\pmb{x};\theta)+KL(q(\pmb{z})||p(\pmb{z|x};\theta))
\end{aligned}
$$
其中$ELBO(q,\pmb{x};\theta)$称为$\log p(\pmb{x};\theta)$的变分下界。最大化对数边际似然$\log p(\pmb{x};\theta)$可以用EM算法来求解。在EM算法的每次迭代中，具体可以分为两步：

（1） E 步：固定𝜃，寻找一个密度函数$q(\pmb{z};\phi)$使其等于或接近于后验密度函数$p(\pmb{z|x};\theta)$；

（2） M 步：固定$q(\pmb{z};\phi)$，寻找𝜃 来最大化$ELBO(q,\pmb{x};\theta,\phi)$．不断重复上述两步骤，直到收敛．

在EM 算法的每次迭代中，理论上最优的$q(\pmb{z};\phi)$ 为隐变量的后验概·率密度函数$p(\pmb{z|x};\theta)$,即
$$
p(\pmb{z|x};\theta)=\frac{p(\pmb{x|z};\theta)p(\pmb{z};\theta)}{\int p(\pmb{x|z};\theta)p(\pmb{z};\theta)d\pmb{z}}
$$


后验概率密度函数$p(\pmb{z|x};\theta)$ 的计算是一个统计推断问题，涉及积分计算．当隐变量𝒛 是有限的一维离散变量时，计算起来比较容易．但在一般情况下，这个后验概率密度函数是很难计算的，通常需要通过变分推断来近似估计。为了降低复杂度，通常会选择一些比较简单的分布$q(\pmb{z};\phi)$来近似推断，常用的变分推断方法有随机变分推断、可拓展变分推断以及非标准变分推断等。但是当$p(\pmb{z|x};\theta)$比较复杂时，近似效果不佳。

变分自编码器（Variational AutoEncoder，VAE）是一种深度生成模型，其思想是利用神经网络来建模复杂的条件概率密度函数。


