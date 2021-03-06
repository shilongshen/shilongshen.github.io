# 深度生成模型




> 
>
> 参考：https://nndl.github.io/
>
> https://www.spaces.ac.cn/archives/5253
>
> https://www.spaces.ac.cn/archives/5343

#### 为什么要研究生成模型？





概率生成模型（Probabilistic Generative Model），简称生成模型，是概率统计和机器学习领域的一类重要模型，指<u>一系列用于随机生成可观测数据的模型</u>。假设在一个连续或离散的高位空间$\cal{X}$中,存在一个随机向量$\pmb{X}$服从一个***未知的数据分布***$p_r(\pmb{x})$，$\pmb{x}\in\cal{X}$。生成模型是根据一些可观测样本$\{\pmb{x}^1,\pmb{x}^2,...,\pmb{x}^N \}$来学习一个<u>参数化的模型</u>$p_\theta(\pmb{x})$来近似位置分布$p_r(\pmb{x})$，<u>并可以用这个模型来生成一些样本</u>，使得生成样本和真实样本尽可能地相似。生成模型通常包含两个基本功能：**概率密度估计**和**生成样本（即采样）**。下图以手写体数字图像为例给出了生成模型的两个功能示例，其中左图表示手写体数字图像的真实分布$p_r(\pmb{x})$以及从中采样的一些“真实”样本，右图表示估计出了分布$p_\theta(\pmb{x})$以及从中采样的“生成”样本。

> 深度生成模型的目标是从训练数据中学习到复杂的概率分布

![img](https://gitee.com/shilongshen/image-bad/raw/master/image/20200514205515.png)

#### 1 概率生成模型

给定一组数据$\cal{D}=\lbrace\pmb{x}^{(n)}\rbrace_{n=1}^N$，假设它们都是独立地从相同的概率密度函数为$p_r(\pmb{x})$ 的未知分布中产生的．**密度估计（Density Estimation）是根据数据集$\cal{D}$来估计其概率密度函数$p_\theta(\pmb{x})$**。

在机器学习中，密度估计是一类无监督学习问题．比如在手写体数字图像的密度估计问题中，我们将图像表示为一个随机向量𝑿，其中每一维都表示一个像素值．假设手写体数字图像都服从一个未知的分布$p_r(\pmb{x})$，希望通过一些观测样本来估计其分布．但是，手写体数字图像中不同像素之间存在复杂的依赖关系（比如相邻像素的颜色一般是相似的），很难用一个明确的图模型来描述其依赖关系，所以直接建模$p_r(\pmb{x})$ 比较困难．因此，我们通常通过**引入隐变量𝒛来简化模型**，这样密度估计问题可以转换为估计变量(𝒙, 𝒛) 的两个局部条件概率$p_\theta(\pmb{z})$和$p_\theta(\pmb{x|z})$．一般为了简化模型，假设隐变量𝒛 的先验分布为标准高斯分布𝒩(0, 𝑰)．隐变量𝒛 的每一维之间都是独立的．在这个假设下，先验分布𝑝(𝒛; 𝜃) 中没有参数．因此，**密度估计的重点是估计条件分布$p{(\pmb{x|z},\theta)}$．**

如果要<u>建模含隐变量的分布</u>，就需要利用EM算法来进行密度估计，而在EM算法中，需要估计条件分布 $p(\pmb{x}|\pmb{z};\theta)$以及后验分布 $p(\pmb{z}|\pmb{x};\theta)$,当这两个分布比较复杂时就可以利用神经网络来建模，这就是**变分自编码器**的思想。

由条件概率公式可得：
$$
p_\theta(x，z)=p_\theta(\pmb{z})p_\theta(\pmb{x|z})
$$


在得到两个变量的局部条件概率$p_\theta(\pmb{z})$ 和$p_\theta(\pmb{x|z})$之后，我们就可以**生成数据**𝒙，具体过程可以分为两步进行：

(1)根据隐变量的先验分布$p_\theta(\pmb{z})$ 进行采样，得到样本$z$。

(2)根据条件分布$p_\theta(\pmb{x|z})$进行采样，得到样本$x$.

为了便于采样，通常$p_\theta(\pmb{x|z})$不能过于复杂。因此，<u>另一种生成样本的思想是从一个简单分布𝑝(𝒛), 𝒛 ∈ 𝒵（比如标准正态分布）中采集一个样本𝒛，并利用一个深度神经网络𝑔 ∶ 𝒵 → 𝒳 使得𝑔(𝒛) 服从$p_r(\pmb{x})$．</u>这样，我们就可以避免密度估计问题，并有效降低生成样本的难度，这正是**生成对抗网络**的思想。

#### 1.2 变分自编码器

##### 1.2.1 含隐变量的生成模型

假设一个生成模型（如图13.3所示）中包含隐变量，即有部分变量是不可观测的，其中观测变量𝑿 是一个**高维**空间𝒳 中的随机向量，隐变量𝒁 是一个相对**低维**的空间𝒵 中的随机向量。

![img](https://gitee.com/shilongshen/image-bad/raw/master/image/20200514215128.png)

这个生成模型的联合概率密度函数为：
$$
p(\pmb{x,z};\theta)=p(\pmb{x|z};\theta)p(\pmb{z};\theta)
$$
其中$p(\pmb{z};\theta)$为隐变量$z$的先验分布的概率密度函数，$p(\pmb{x|z};\theta)$为已知$z$时观测变量$\pmb{x}$的条件概率密度函数,$\theta$表示两个密度函数的参数。一般情况下我们可以假设$p(\pmb{z};\theta)$和$p(\pmb{x|z};\theta)$为某种参数化的分布族，比如正态分布，这种分布的形式已知，只是参数$\theta$未知，可以通过极大似然进行估计。

给定样本$\pmb{x}$，其**对数边际似然函数**$\log p(\pmb{x};\theta)$可以分解为
$$
\log p(\pmb{x};\theta)=ELBO(q,\pmb{x};\theta,\phi)+KL[q(\pmb{z};\phi),p(\pmb{z|x};\theta)]
$$
**其中$q(\pmb{z};\phi)$是额外引入的变分密度函数**，其参数为$\phi$，$ELBO(q,\pmb{x};\theta,\phi)$为证据下界，
$$
ELBO(q,\pmb{x};\theta,\phi)=\mathbb{E}_{\mathcal{z}\sim {q(\pmb{z};\phi)}}[\log \frac{p(\pmb{x,z},\theta)}{q(\pmb{z};\phi)}]
$$
最大化对数边际似然$\log p(\pmb{x};\theta)$可以用EM算法来求解。在EM算法的每次迭代中，具体可以分为两步：

（1） E 步：固定𝜃，寻找一个密度函数$q(\pmb{z};\phi)$使其等于或接近于后验密度函数$p(\pmb{z|x};\theta)$；

（2） M 步：固定$q(\pmb{z};\phi)$，寻找𝜃 来最大化$ELBO(q,\pmb{x};\theta,\phi)$．不断重复上述两步骤，直到收敛．

在EM 算法的每次迭代中，**理论上最优的$q(\pmb{z};\phi)$ 为隐变量的后验概率密度函数$p(\pmb{z|x};\theta)$，**即
$$
p(\pmb{z|x};\theta)=\frac{p(\pmb{x|z};\theta)p(\pmb{z};\theta)}{\int p(\pmb{x|z};\theta)p(\pmb{z};\theta)d\pmb{z}}
$$
后验概率密度函数$p(\pmb{z|x};\theta)$ 的计算是一个统计推断问题，涉及积分计算．当隐变量𝒛 是有限的一维离散变量时，计算起来比较容易．但在一般情况下，这个后验概率密度函数是很难计算的，通常需要通过**变分推断**来近似估计。为了降低复杂度，通常会选择一些比较简单的分布$q(\pmb{z};\phi)$来近似推断$p(\pmb{z|x};\theta)$．当$p(\pmb{z|x};\theta)$比较复杂时，近似效果不佳．此外，概率密度函数$p(\pmb{x|z};\theta)$ 一般也比较复杂，很难直接用已知的分布族函数进行建模。

> **什么是变分推断**：
>
> 后验概率密度等于：
> $$
> p(\pmb{z}|\pmb{x})=\frac{p(\pmb{x,z})}{\int p(\pmb{x,z})d\pmb{z}}
> $$
> 上式存在着积分运算，对于大多数的模型来说，计算积分项通常是比较困难的。变分推断的思想就是引入一个简单的分布$q_\lambda(\pmb{z})$来逼近后验概率$p(\pmb{z|x})$。具体来说就是通过控制参数$\lambda$来最小化两个分布之间的度量距离，这一度量距离通常使用KL散度。
> $$
> KL[q_\lambda(z) \ ||\  p(\pmb{z|x})]=-\mathbb{E}_{q_\lambda}(z)[\ \log \frac{p(\pmb{z|x})}{q_\lambda(z)} \ ]
> $$
> 通过这种方式，变分推断加那个贝叶斯推断转换为优化问题。理想情况下$q_\lambda(\pmb{z})=p(\pmb{z|x})$，但是当分布$p(\pmb{z|x})$复杂时，变分分布$q_\lambda(\pmb{z})$通常是欠参数化的，无法很好的逼近$p(\pmb{z|x})$。





变分自编码器（Variational AutoEncoder，VAE）是一种深度生成模型，**其思想是利用神经网络来分别建模两个复杂的条件概率密度函数**。

（1） 用神经网络来估计变分分布$q(\pmb{z};\phi)$，称为<u>推断网络</u>．理论上$q(\pmb{z};\phi)$可不依赖𝒙．但**由于$q(\pmb{z};\phi)$ 的目标是近似后验分布$p(\pmb{z|x};\theta)$，**其和𝒙 相关，因此变分密度函数一般写为$q(\pmb{z|x};\phi)$．**推断网络的输入为𝒙，输出为变分分布$q(\pmb{z|x};\phi)$．**
（2） 用神经网络来估计概率分布$p(\pmb{x|z};\theta)$，称为<u>生成网络</u>．生成网络的输入为𝒛，输出为概率分布$p(\pmb{x|z};\theta)$．

将推断网络和生成网络合并就得到了变分自编码器的整个网络结构，如图13.4所示，其中实线表示网络计算操作，虚线表示采样操作．

![](https://gitee.com/shilongshen/image-bad/raw/master/image/20200529164206.png)

变分自编码器的名称来自于其整个网络结构和自编码器比较类似． 我们可以把推断网络看作“编码器”，将可观测变量映射为隐变量；把生成网络看作“解码器”，将隐变量映射为可观测变量．然而，变分自编码器背后的原理和自编码器完全不同．**变分自编码器中的编码器和解码器的输出为分布（或分布的参数），而不是确定的编码**。

##### 1.2.2 推断网络

为了简单起见，假设$q(\pmb{z|x};\phi)$是服从对角化协方差的高斯分布
$$
q(\pmb{z|x};\phi)=\mathcal{N}(\pmb{z;\mu_I,\sigma_I^2I})
$$
其中$\mu_I$和$\sigma_I^2$表示高斯分布的均值和方差，可以通过推断网络$f_I(\pmb{x};\phi)$来预测。
$$
\left[ \begin{array}{cc|r}
\mu_I\\ \sigma_I^2 \end{array} \right]=f_I(\pmb{x};\phi)
$$
其中$f_I(\pmb{x};\phi)$可以是一般的全连接层或卷积网络，比如一个两层的全连接层网络，
$$
\pmb{h}=\sigma(\pmb{W^{(1)}x+b^{(1)}})
$$

$$
\mu_I=\pmb{W^{(2)}h+b^{(2)}}
$$

$$
\sigma_I^2=softplus(\pmb{W^{(3)}h+b^{(3)}})
$$

其中$\phi$为所有的网络参数$\{ \pmb{W^{(1)},b^{(1)},W^{(2)},b^{(2)},W^{(3)},b^{(3)}} \}$,$\sigma$和$softplus$为激活函数。这里使用$softplus$ 激活函数是由于方差总是非负的．在实际实现中，也可以用一个线性层（不需要激活函数）来预测$\log (\sigma_I^2)$。

推断网络的目标是使得$q(\pmb{z|x};\phi)$尽可能接近$p(\pmb{z|x};\theta)$，需要找到一组网络参数$\phi^*$来最小化两个分布的KL散度，即
$$
\phi^*=arg \min\limits_{\phi}KL[q(\pmb{z|x};\phi),p(\pmb{z|x};\theta)]
$$
然而直接计算上面的KL散度是不可能的，因为$p(\pmb{z|x};\theta)$一般无法计算。传统方法是利用采样或者变分法来近似推断。基于采样的方法效率很低且估计也不是很准确，所以一般是使用变分推断方法，即用简单分布q去近似复杂的分布$p(\pmb{z|x};\theta)$。但是在深度生成模型$p(\pmb{z|x};\theta)$通常比较复杂，很难用简单分布去近似，因此，我们需要找到一种间接计算方法。根据公式（3）可知：
$$
KL[q(\pmb{z|x};\phi),p(\pmb{z|x};\theta)]=\log p(\pmb{x};\theta)-ELBO(q,\pmb{x};\theta,\phi)
$$
因此推断网络的目标函数可以转换为
$$
\begin{aligned}
\phi^*&=arg \min\limits_{\phi}KL[q(\pmb{z|x};\phi),p(\pmb{z|x};\theta)]\\
&=arg\min\limits_{\phi}[\log p(\pmb{x};\theta)-ELBO(q,\pmb{x};\theta,\phi)]\\
&=arg\max\limits_{\phi}[ELBO(q,\pmb{x};\theta,\phi)]
\end{aligned}
$$
即推断网络的目标转换为寻找一组网络参数$\phi^*$使得证据下界$ELBO(q,\pmb{x};\theta,\phi)$最大。



##### 1.2.3 生成网络

生成模型的联合分布$p(\pmb{x,z};\theta)=p(\pmb{x|z};\theta)p(\pmb{z};\theta)$可以分解为两部分：隐变量$\pmb{z}$的先验分布$p(\pmb{z};\theta)$和条件概率分布$p(\pmb{x|z};\theta)$。

为简单起见，我们一般假设隐变量$𝒛$ 的先验分布为各向同性的标准高斯分布$\mathcal{N}(z|0,I)$．隐变量𝒛 的每一维之间都是独立的。

条件概率分布$p(\pmb{x|z};\theta)$可以通过**生成网络**来建模，为了简单起见，我们同样用参数化的分布族来表示条件概率分布$p(\pmb{x|z};\theta)$，这些分布族的参数可以用生成网络计算得到。如果$\pmb{x}\in \mathbb{R}^D$是D维的连续向量，可以假设$p(\pmb{x|z};\theta)$服从对角化协方差的高斯分布，即
$$
p(\pmb{x|z};\theta)=\mathcal{N}(\pmb{z;\mu_G,\sigma_G^2I})
$$
其中$\mu_G \in \mathbb{R}^D$和$\sigma_G \in \mathbb{R}^D$,同样用生成网络$f_G(\pmb{z};\theta)$来预测。生成网络的目标是找到一组网络参数$\theta^*$来最大化证据下界$ELBO(q,\pmb{x};\theta,\phi)$,即
$$
\theta^*=arg\max\limits_{\theta}[ELBO(q,\pmb{x};\theta,\phi)]
$$


##### 1.2.4 模型汇总

从以上分析，推断网络和生成网络的目标都为最大化证据下界$ELBO(q,\pmb{x};\theta,\phi)$。因此**变分自编码器的总目标**为
$$
\begin{aligned}
\max\limits_{\theta,\phi}[ELBO(q,\pmb{x};\theta,\phi)]&=
\max\limits_{\theta,\phi}\mathbb{E}_{\mathcal{z}\sim {q(\pmb{z};\phi)}}[\log \frac{p(\pmb{x,z},\theta)}{q(\pmb{z};\phi)}]\\
&=\max\limits_{\theta,\phi}\mathbb{E}_{\mathcal{z}\sim {q(\pmb{z};\phi)}}[\log \frac{p(\pmb{x|z},\theta),p(\pmb{z},\theta)}{q(\pmb{z};\phi)}]\\
&=\max\limits_{\theta,\phi}\mathbb{E}_{\mathcal{z}\sim {q(\pmb{z|x};\phi)}}[\log p(\pmb{x|z};\theta)]-KL[q(\pmb{z|x};\phi),p(\pmb{z};\theta)]


\end{aligned}
$$
从EM 算法角度来看，变分自编码器优化推断网络和生成网络的过程，可以分别看作EM 算法中的E 步和M 步．但在变分自编码器中，这两步的目标合二为一，都是最大化证据下界．此外，变分自编码器可以看作神经网络和贝叶斯网络的混合体．贝叶斯网络中的所有节点都是随机变量．在变分自编码器中，我们仅仅将隐藏编码对应的节点看成是随机变量，其他节点还是作为普通神经元．这样，编码器变成一个变分推断网络，而解码器变成一个将隐变量映射到观测变量的生成网络。对上述公式进行分析。

（1）通常情况下期望$\mathbb{E}_{\mathcal{z}\sim {q(\pmb{z|x};\phi)}}[\log p(\pmb{x|z};\theta)]$可以通过**采样**的方式近似计算。对于每个样本$\pmb{x}$，根据$q(\pmb{z|x};\phi)$采集M个$\pmb{z}^{(m)},1\leq m \leq M$,有
$$
\mathbb{E}_{\mathcal{z}\sim {q(\pmb{z|x};\phi)}}[\log p(\pmb{x|z};\theta)]\approx \frac{1}{M}\sum_{m=1}^{M}\log p(\pmb{x|z^{(m)}};\theta)
$$
期望$\mathbb{E}_{\mathcal{z}\sim {q(\pmb{z|x};\phi)}}[\log p(\pmb{x|z};\theta)]$依赖于参数$\phi$。**但是在上面的近似中，这个期望与参数$\phi$无关**。当使用梯度下降法时，期望$\mathbb{E}_{\mathcal{z}\sim {q(\pmb{z|x};\phi)}}[\log p(\pmb{x|z};\theta)]$关于参数$\phi$的梯度为0.**这种情况是由于变量$z$和参数$\phi$不是直接的确定性关系，而是一种采样关系**。这种情况一般通过两种方法解决：再参数化；梯度估计。



> #####  再参数化
>
> 再参数化是将一个函数$f(\theta)$的参数$\theta$用另外一组参数表示$\theta=g(\vartheta)$，这样函数就转换为参数为$\vartheta$的函数$\bar{f}(\vartheta)=f(g(\vartheta))$。再参数化通常用来将原始参数转换为另外一组具有特殊属性的参数．比如当𝜃 为一个很大的矩阵时，可以使用两个低秩矩阵的乘积来再参数化，从而减少参数量。
>
> 由以上分析可知，由于随机边量$z$采样自后验分布$q(\pmb{z|x};\phi)$，变量$\pmb{z}$和参数$\phi$不是直接的确定性关系，而是一种采样关系。，因此无法直接求解$\pmb{z}$关于参数$\phi$的导数。这时可以**通过再参数化的方法将$\pmb{z}$和$\phi$之间的随机性的采样关系转变为确定性函数关系**。
>
> 我们引入一个分布为$p(\epsilon)$的随机变量$\epsilon$,期望$\mathbb{E}_{\mathcal{z}\sim {q(\pmb{z|x};\phi)}}[\log p(\pmb{x|z};\theta)]$可以重写为
> $$
> \mathbb{E}_{\mathcal{z}\sim {q(\pmb{z|x};\phi)}}[\log p(\pmb{x|z};\theta)]=\mathbb{E}_{\epsilon\sim {p(\epsilon)}}[\log p(\pmb{x}|g(\phi,\epsilon);\theta)]
> $$
> 其中$\pmb{z}\triangleq g(\phi,\epsilon)$为一个确定性函数。
>
> 假设$q(\pmb{z|x};\phi)$为正态分布$\mathcal{N}(\mu_I;\sigma_I^2I)$，其中$\{ \mu_I,\sigma_I^2 \}$为推断网络$f_I(\pmb{x};\phi)$的输出，依赖于参数$\phi$，我们可以通过下面的方式来再参数化：
> $$
> \pmb{z}=\mu_I+\sigma_I\odot\epsilon
> $$
> 其中$\epsilon\sim\mathcal{N}(0,I)$，这样参数$\pmb{z}$和参数$\phi$的关系就从采样关系变为确定关系，使得$\pmb{z}\sim q(\pmb{z|x};\phi)$的随机性独立于参数$\phi$，从而可以求$\pmb{z}$关于$\phi$的导数。



（2）KL散度$KL[q(\pmb{z|x};\phi),p(\pmb{z};\theta)]$通常可以直接计算。给定D维空间的两个正态分布$\mathcal{N}(\mu_1,\Sigma_1)$以及$\mathcal{N}(\mu_2,\Sigma_2)$,其KL散度为
$$
KL(\mathcal{N}(\mu_1,\Sigma_1),\mathcal{N}(\mu_2,\Sigma_2))\\
=\frac{1}{2}(tr(\Sigma_2^{-1}\Sigma_1)+(\mu_2-\mu_1)^T\Sigma_2^{-1}(\mu_2-\mu_1)-D+log{\frac{|\Sigma_2|}{|\Sigma_1|}})
$$
这样当$p(\pmb{z};\theta)=\mathcal{N}(\pmb{z};0;I)$以及$q(\pmb{z|x};\phi)=\mathcal{N}(\pmb{z};\mu_I;\sigma_I^2I)$时，(目标是让$q(\pmb{z|x};\phi)$为标准正态分布)
$$
KL[q(\pmb{z|x};\phi),p(\pmb{z};\theta)]=\frac{1}{2}(tr(\sigma_I^2I)+\mu_I^T\mu_I-d-log(|\sigma_I^2I|))
$$


##### 1.2.5 训练

通过再参数化，变分自编码器可以通过梯度下降法来学习参数，从而提高变分自编码器的训练效率．

给定一个数据集$\mathcal{D}=\{ \pmb{x}^{(n)} \}_{n=1}^N$，对于每个样本$\pmb{x}^{(n)}$，随机采样M个变量$\epsilon^{(n,m)},1 \leq m \leq M$，并通过公式（17）计算$\pmb{z}^{(n,m)}$。变分自编码器的损失函数近似为
$$
\begin{aligned}
\mathcal{J}(\phi,\theta|\mathcal{D})
&=\sum_{n=1}^N{\huge(}\frac{1}{M}\sum_{n=1}^M\log p(\pmb{x^{(n)}|z^{(n,m)}};\theta)-KL[q(\pmb{z|x};\phi),p(\pmb{z};\theta)]{\huge)}\\
&=\sum_{n=1}^N{\huge(}\frac{1}{M}\sum_{n=1}^M\log p(\pmb{x^{(n)}|z^{(n,m)}};\theta)-KL[\mathcal{N}(\mu_I;\sigma_I^2),\mathcal{N}(0;I)]{\huge)}
\end{aligned}
$$
如果采用随机梯度方法，每次从数据集中采集一个样本𝒙 和一个对应的随机变量𝜖，并进一步假设𝑝(𝒙|𝒛; 𝜃) 服从高斯分布𝒩(𝒙|𝝁𝐺 , 𝜆𝑰)，其中𝝁𝐺 = 𝑓𝐺 (𝒛; 𝜃)是生成网络的输出，𝜆 为控制方差的超参数．则目标函数可以简化为
$$
\mathcal{J}(\phi,\theta|\pmb{x})=-\frac{1}{2}||\pmb{x}-\mu||^2+\lambda KL[\mathcal{N}(\mu_I;\sigma_I),\mathcal{N}(0;I)]
$$
其中第一项可以近似看作输入𝒙 的重构正确性，第二项可以看作正则化项，𝜆 可以看作正则化系数．这和自编码器在形式上非常类似，但它们的内在机理是完全不同的.

![](https://gitee.com/shilongshen/image-bad/raw/master/image/20200529225032.png)

......

#### 1.3 生成对抗网络

##### 1.3.1显式密度模型和隐式密度模型

​	之前介绍的深度生成模型，比如变分自编码器、深度信念网络等，都是显示地构建出<u>样本的密度函数</u>𝑝(𝒙; 𝜃)，<u>并通过最大似然估计来求解参数</u>，称为显式密度模型（Explicit Density Model）． 比如， 变分自编码器的密度函数为𝑝(𝒙, 𝒛; 𝜃) = 𝑝(𝒙|𝒛; 𝜃)𝑝(𝒛; 𝜃)．虽然使用了神经网络来估计𝑝(𝒙|𝒛; 𝜃)，但是我们依然假设𝑝(𝒙|𝒛; 𝜃) 为一个参数分布族，而神经网络只是用来预测这个参数分布族的参数．这在某种程度上限制了神经网络的能力。



> [什么是极大似然估计]: https://zhuanlan.zhihu.com/p/26614750
>
> 极大似然估计，通俗来讲，就是利用已知的样本结果信息，反推最具有可能导致这些样本结果出现的模型参数值。
>
> 在显式密度模型中，我们已知的是一组数据$\cal{D}=\lbrace\pmb{x}^{(n)}\rbrace_{n=1}^N$，它们都是独立地从相同的概率密度函数为$p_r(\pmb{x})$ 的未知分布中产生的。现在我们要使用极大似然估计，根据数据集$\cal{D}$来反推概率密度函数$p_\theta(\pmb{x})$的参数。
>
> 那么我们就知道了极大似然估计的核心关键就是对于一些情况，**样本太多，无法得出分布的参数值，可以采样小样本后，利用极大似然估计获取假设中分布的参数值。**
>
> **极大似然估计等同与最小化KL散度**
>
> ![](https://gitee.com/shilongshen/image-bad/raw/master/img/20200824084145.png)

​	

如果只是希望<u>有一个模型能生成符合数据分布𝑝𝑟(𝒙) 的样本</u>，<u>那么可以不显示地估计出数据分布的密度函数</u>．假设在低维空间𝒵 中有一个简单容易采样的分布𝑝(𝒛)，𝑝(𝒛) 通常为标准多元正态分布𝒩(0, 𝑰)．我们用神经网络构建一个映射函数𝐺 ∶ 𝒵 → 𝒳，称为生成网络．利用神经网络强大的拟合能力，使得𝐺(𝒛) 服从数据分布𝑝𝑟(𝒙)．这种模型就称为隐式密度模型（Implicit Density Model）．**所谓隐式模型就是指并不显式地建模𝑝𝑟(𝒙)，而是建模生成过程**．图13.7给出了隐式模型生成样本的过程．

![](https://gitee.com/shilongshen/image-bad/raw/master/image/20200515164107.png)

**注意**：显示密度模型使用**最大似然估计**来确定参数$\theta$使得密度函数$p_{\theta}(x)$尽可能的接近真实数据分布$p_{r}(x)$，这样就能够保证从密度函数$p_{\theta}(x)$采样（生成）的样本$G(z,\theta)$。服从真实数据分布$p_{r}(x)$.而隐式密度模型则不建立$p_{\theta}(x)$,而直接通过神经网络使用在低维空间𝒵 中的一个简单容易采样的分布𝑝(𝒛)来生成服从真实数据分布$p_{r}(x)$的样本$G(z,\theta)$。

##### 1.3.2网络分解

隐式密度模型的<u>一个关键是如何确保生成网络产生的样本一定是服从真实的数据分布</u>．既然我们不构建显式密度函数，就无法通过最大似然估计等方法来训练。生成对抗网络是通过**对抗训练**的方式来使得生成网络产生的样本服从真实数据分布.在生成对抗网络中，有两个网络进行对抗训练．一个是判别网络，目标是尽量准确地判断一个样本是来自于真实数据还是由生成网络产生；另一个是生成网络，目标是尽量生成判别网络无法区分来源的样本．这两个目标相反的网络不断地进行交替训练．当最后收敛时，如果判别网络再也无法判断出一个样本的来源，那么也就等价于生成网络可以生成符合真实数据分布的样本．生成对抗网络的流程图如图13.8所示．

![](https://gitee.com/shilongshen/image-bad/raw/master/image/20200515165846.png)

**注意**:显式密度模型使用**最大似然估计**来确定参数$\theta$使得密度函数$p_{\theta}(x)$尽可能的接近真实数据分布$p_{r}(x)$；而隐式密度模型使用**对抗训练**的方式来保证生成样本服从真实数据分布。

###### 1.3.2.1 判别网络

判别网络（Discriminator Network）$D(\pmb{x};\phi)$的目标是区分出一个样本$\pmb{x}$是来自真实分布$p_r(\pmb{x})$还是来自生成模型$p_{\theta}(\pmb{x})$，因此**判别网络实际上是一个二分类的分类器**。用标签$y=1$来表示样本来自真实分布，$y=0$表示样本来自生成模型，判别网络$D(\pmb{x};\phi)$的输出为$\pmb{x}$属于真实数据分布的概率，
$$
p(y=1|\pmb{x})=D(\pmb{x};\phi)
$$
则样本来自生成模型的概率为$p(y=0|\pmb{x})=1-D(\pmb{x};\phi)$。

给定一个样本$(\pmb{x}，y)$,$y=\{1,0\}$表示其来自于$p_r(\pmb{x})$还是$p_{\theta}(\pmb{x})$，判别网络的目标是最小化交叉熵，即
$$
\min\limits_{\phi}-(\mathbb{E}_{\pmb{x}}[ylogp(y=1|\pmb{x})+(1-y)logp(y=0|\pmb{x})])
$$
假设分布$p(\pmb{x})$是由是由分布$p_r(\pmb{x})$和分布$p_{\theta}(\pmb{x})$等比例混合而成，即$p(\pmb{x})=\frac{1}{2}(p_r(\pmb{x})+p_{\theta}(\pmb{x}))$，则上式等价为
$$
\max\limits_{\phi}(\mathbb{E}_{\pmb{x}\sim p_{r}(\pmb{x})}[logD(\pmb{x};\phi)]+
\mathbb{E}_{\pmb{x'}\sim p_{\theta}(\pmb{x'})}[log(1-D(\pmb{x'};\phi)])\\
=\max\limits_{\phi}(\mathbb{E}_{\pmb{x}\sim p_{r}(\pmb{x})}[logD(\pmb{x};\phi)]+
\mathbb{E}_{\pmb{z}\sim p(\pmb{z})}[log(1-D(G(\pmb{z};\theta);\phi))])
$$
其中$\theta$和$\phi$分别是生成网络和判别网络的参数。

> 直观的理解：当判别器的输入来自于真实分布$p_r(\pmb{x})$，则$\mathbb{E}_{\pmb{x'}\sim p_{\theta}(\pmb{x'})}=0,\mathbb{E}_{\pmb{x}\sim p_{r}(\pmb{x})}=1$，此时的优化目标为$\max\limits_{\phi}[logD(\pmb{x};\phi)]$，即要求$D(\pmb{x};\phi)$尽可能接近1,($0\leq D(\pmb{x};\phi)\leq1$)。同理，当判别器的输入来自于生成分布$p_{\theta}(\pmb{x})$，则$\mathbb{E}_{\pmb{x'}\sim p_{\theta}(\pmb{x'})}=1,\mathbb{E}_{\pmb{x}\sim p_{r}(\pmb{x})}=0$，此时的优化目标为$\max\limits_{\phi}log(1-D(G(\pmb{z};\theta);\phi))$，即要求$D(G(\pmb{z};\theta))$尽可能接近0。

###### 1.3.2.2 生成网络

生成网络的目标刚好与判别网络相反，即让判别网络将自己生成的样本判别为真实样本。
$$
\max\limits_{\theta}(\mathbb{E}_{\pmb{z}\sim p(\pmb{z})}[log(D(G(\pmb{z};\theta);\phi))])\\
=\min\limits_{\theta}(\mathbb{E}_{\pmb{z}\sim p(\pmb{z})}[log(1-D(G(\pmb{z};\theta);\phi))])
$$
上面的这两个目标函数是等价的．但是在实际训练时，一般使用前者，因为其梯度性质更好．我们知道，函数log(𝑥), 𝑥 ∈ (0, 1) 在𝑥 接近1 时的梯度要比接近0 时的梯度小很多，接近“饱和”区间．这样，当判别网络𝐷 以很高的概率认为生成网络𝐺 产生的样本是“假”样本，即(1 − 𝐷(𝐺(𝒛; 𝜃); 𝜙)) → 1，这时目标函数关于𝜃 的梯度反而很小，从而不利于优化．

##### 13.3.3 训练

采用**交替优化**的方式进行训练。

![](https://gitee.com/shilongshen/image-bad/raw/master/image/20181212153355732.PNG)



和单目标的优化任务相比，生成对抗网络的两个网络的优化目标刚好相反．因此生成对抗网络的训练比较难，往往不太稳定．一般情况下，需要平衡两个网络的能力．对于判别网络来说，一开始的判别能力不能太强，否则难以提升生成网络的能力．但是，判别网络的判别能力也不能太弱，否则针对它训练的生成网络也不会太好．在训练时需要使用一些技巧，使得**在每次迭代中，判别网络比生成网络的能力强一些，但又不能强太多**。

生成对抗网络的训练流程如算法13.1所示．每次迭代时，判别网络更新𝐾 次而生成网络更新一次，即首先要保证判别网络足够强才能开始训练生成网络．在实践中𝐾 是一个超参数，其取值一般取决于具体任务．

![](https://gitee.com/shilongshen/image-bad/raw/master/image/20200521102647.png)



####  2 概率密度估计

概率密度估计，简称密度估计，**是基于一些观测样本来估计一个随机变量的概率密度函数**

密度估计方法可以分为两类：参数密度估计和非参数密度估计。

##### 2.1 参数密度估计

参数密度估计（Parametric Density Estimation）是**根据先验知识假设随机变量服从某种分布，然后通过训练样本来估计分布的参数**．

令$\cal{D}=\lbrace \pmb{x}^{(n)}\rbrace_{n=1}^N$为从某个未知分布中独立抽取的N个训练样本，假设这些样本服从一个概率分布函数$p(\pmb{x};\theta)$，其对数似然函数为
$$
logp(\cal{D};\theta)=\sum_{n=1}^Nlogp(\pmb{x}^{(n)};\theta)
$$

------

什么是似然函数？：N个训练样本服从分布$p(\pmb{x};\theta)$。似然函数就是计算产生N个训练样本的可能性有多大

则似然函数为
$$
p(\cal{D};\theta)=p(\pmb{x}^1;\theta)p(\pmb{x}^2;\theta)...p(\pmb{x}^N;\theta)
$$
则相应的对数似然函数为：
$$
logp(\cal{D};\theta)=\sum_{n=1}^Nlogp(\pmb{x}^{(n)};\theta)
$$

------

使用**极大似然估计**来寻找参数$\theta$使得$logp(\cal{D};\theta)$最大。**这样参数估计问题就转化为最优化问题**：
$$
{\theta}^{ML}
=arg \max\limits_{\theta}\sum_{n=1}^Nlogp(\pmb{x}^{(n)};\theta)
$$

------

如何通过极大似然估计求参数$\theta$？

求似然函数关于参数$\theta $的导数，并令导数为0.即求极值。

------



##### 2.2 正态分布

假设样本$\pmb{x}\in\cal{\R}^D$服从正态分布
$$
\cal{N}(\pmb{x}|\pmb{\mu},\pmb{\Sigma})=\frac{1}{(2\pi)^{D/2}|\pmb{\Sigma}|^{1/2}}exp(-\frac{1}{2}(\pmb{x}-\pmb{\mu})^{T}\pmb{\Sigma}^{-1}(\pmb{x}-\pmb{\mu}))
$$
其中$\pmb{\mu}$和$\pmb{\Sigma}$分别为正态分布的均值和方差。

数据集$\cal{D}=\lbrace \pmb{x}^{(n)}\rbrace_{n=1}^N$的对数似然函数为
$$
logp(\cal{D}|\pmb{\mu},\pmb{\Sigma})=-\frac{N}{2}log(2\pi)^D|\pmb{\Sigma}|
-\frac{1}{2} \sum_{n=1}^N(\pmb{x}^{(n)}-\pmb{\mu})^{T}\pmb{\Sigma}^{-1}(\pmb{x}^{(n)}-\pmb{\mu})
$$
分别求上式关于$\pmb{\mu}$和$\pmb{\Sigma}$的偏导数，并令其等于0，可得
$$
\pmb{\mu}^{ML}=\frac{1}{N}\sum_{n=1}^N\pmb{x}^{(n)}\\
\pmb{\Sigma}=\frac{1}{N}\sum_{n=1}^N(\pmb{x}^{(n)}-\pmb{\mu}^{ML})\pmb{\Sigma}^{-1}(\pmb{x}^{(n)}-\pmb{\mu}^{ML})^{T}
$$
此时求得的$\pmb{\mu}$和$\pmb{\Sigma}$，即通过N个从分布$p(\pmb{x};\theta)$采样的样本推测得到的分布的参数。通俗来讲，就是利用已知的样本结果信息（N个采样样本），反推最具有可能导致这些样本结果出现的模型参数值（$\pmb{\mu}$和$\pmb{\Sigma}$）。

##### 2.3 非参数估计

...

#### 3 EM算法

概率图模型，简称**图模型**，是指一种利用图结构来描述多元随机变量之间条件独立关系的概率模型，从而给研究高维空间中的概率模型带来了很大的便捷性。

图模型的基本问题

（1） 表示问题：对于一个概率模型，如何通过图结构来描述变量之间的依赖关系。

（2） 学习问题：图模型的学习包括图结构的学习和参数的学习．在本章中，我们只关注在给定图结构时的参数学习，即参数估计问题

（3） 推断问题：在已知部分变量时，计算其他变量的条件概率分布。

图模型的学习可以分为两部分：一是**网络结构学**习，即寻找最优的网络结构；二是**网络参数估计**，即已知网络结构，估计每个条件概率分布的参数．网络结构学习比较困难，一般是由领域专家来构建．本节只讨论在给定网络结构条件下的参数估计问题．图模型的参数估计问题又分为不包含隐变量时的参数估计问题和包含隐变量时的参数估计问题。

如果图模型中包含隐变量，即有部分变量是不可观测的，就需要用EM算法进行参数估计。在一个包含隐变量的图模型中，令$\pmb{X}$定义可观测变量几何，$\pmb{Z}$定义隐变量集合，一个样本$\pmb{x}$的边际似然函数为
$$
p(\pmb{x};\theta)=\sum_{\pmb{z}}p(\pmb{x,z};\theta)
$$
其中$\theta$为模型参数。边际似然也称为**证据**。

给定$N$个训练样本$\mathcal{D}=\{ \pmb{x}^{(n)} \}_{n=1}^N$，整个训练集的对数似然函数为
$$
\begin{aligned}
\mathcal{L(D;\theta)}&=\frac{1}{N}\sum_{n=1}^Nlogp(\pmb{x}^{(n)};\theta)\\
&=\frac{1}{N}\sum_{n=1}^Nlog\sum_{\pmb{z}}p(\pmb{x^{(n)},z};\theta)
\end{aligned}
$$
通过最大化整个训练集的对数边际似然$\mathcal{L(D;\theta)}$，可以估计出最优的参数$\theta^*$．然而计算边际似然函数时涉及$𝑝(𝑥)$ 的推断问题，需要在**对数函数的内部进行求和（或积分）**．这样，当计算参数$\theta$ 的梯度时，这个求和操作依然存在．除非𝑝(𝒙, 𝒛; 𝜃) 的形式非常简单，否则这个求和难以直接计算。

为了计算$logp(\pmb{x};\theta)$，我们引入一个额外的变分函数$q(\pmb{z})$,$q(\pmb{z})$为定义在隐变量$\pmb{Z}$上的分布。样本$\pmb{x}$的对数边际似然函数为
$$
\begin{aligned}
\log p(\pmb{x};\theta)&=\log \sum_{\pmb{z}}q(\pmb{z})\frac{p(\pmb{x,z};\theta)}{q(\pmb{z})}\\
&\geq \sum_{\pmb{z}}q({\pmb{z}})\log \frac{p(\pmb{x,z};\theta)}{q(\pmb{z})}\\
&\triangleq ELBO(q,\pmb{x};\theta)
\end{aligned}
$$
其中$ELBO(q,\pmb{x};\theta)$为对数边际似然函数$\log p(\pmb{x};\theta)$的下界，称为证据下界。上述公式使用了Jensen不等式，由Jensen不等式的性质可知，仅当$q(\pmb{z})=p(\pmb{z|x};\theta)$时，$\log p(\pmb{x};\theta)=ELBO(q,\pmb{x};\theta)$。这样最大化对数边际似然函数$\log p(\pmb{x};\theta)$的过程可以分解为两个步骤：

**E步**：先找到近似分布$q(\pmb{z})=p(\pmb{z|x};\theta)$使得$\log p(\pmb{x};\theta)=ELBO(q,\pmb{x};\theta)$。

**M步**：再找到参数$\theta ^*$最大化$ELBO(q,\pmb{x};\theta)$。

> **信息论视角** 对数边际似然函数的分解
>
> 因为$p(\pmb{x,z};\theta)=p(\pmb{z|x};\theta)p(\pmb{x};\theta)$，所以有$\log p(\pmb{x,z};\theta)=\log p(\pmb{z|x};\theta)+\log p(\pmb{x};\theta)$，进而有$\log p(\pmb{x};\theta)=\log p(\pmb{x,z};\theta)-\log p(\pmb{z|x};\theta)$。因此
> $$
> \begin{aligned}
> \log p(\pmb{x};\theta)&=\sum _{\pmb{z}}q(\pmb{z})\log p(\pmb{x};\theta)\\
> &=\sum _{\pmb{z}}q(\pmb{z})(\log p(\pmb{x,z};\theta)-\log p(\pmb{z|x};\theta))\\
> &=\sum _{\pmb{z}}q(\pmb{z})\log\frac{ p(\pmb{x,z};\theta)}{q(\pmb{z})}-\sum _{\pmb{z}}q(\pmb{z})\log\frac{ p(\pmb{z|x};\theta)}{q(\pmb{z})}\\
> &=ELBO(q,\pmb{x};\theta)+KL(q(\pmb{z})||p(\pmb{z|x};\theta))
> \end{aligned}
> $$
>
> 证明：
>
> $q(z)$为概率密度函数，所以$\sum_{z}q(z)=1,\int q(z)dz=1  \quad (\sum=\int)$.
> $$
> \begin{aligned}
> \log p(x)&=\int q(z)\log p(x)dz\\
> &=\int q(z)[\log p(x,z)-\log p(z|x)]dz\\
> &=\int q(z)\log p(x,z,\theta)dz-\int q(z)\log q(z)dz-\int q(z)\log (z|x,\theta)dz+\int q(z)\log q(z)dz\\
> &=\int q(z)[\log p(x,z,\theta)-\log q(z)]dz-\int q(z)[\log p(z|x,\theta)-\log q(z)]dz\\
> &=\int q(z)\log\frac{ p(x,z,\theta)}{ q(z)}dz-\int q(z)\log \frac{p(z|x,\theta)}{\log q(z)}dz\\
> &=ELBO(q,\pmb{x};\theta)+KL(q(\pmb{z})||p(\pmb{z|x};\theta))
> \end{aligned}
> $$
> 








