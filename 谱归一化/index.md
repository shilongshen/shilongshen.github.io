# 谱归一化




# 1. Lipschitz定义

![img](https://img-blog.csdnimg.cn/20190923152718681.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI2MDA0Mzg3,size_16,color_FFFFFF,t_70)

# 2. Lipschitz常数



![img](https://img-blog.csdnimg.cn/20190923152819226.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI2MDA0Mzg3,size_16,color_FFFFFF,t_70)

# 3.深度学习中的Lipschitz约束：泛化与生成模型



## L约束与泛化

**扰动敏感**

记输入为 x，输出为 y，模型为 f，模型参数为 w，记为：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci9kYTlhMTlhOS05YWJhLTRjYTAtYjMzYi1hODg2MjExY2QzYzMvMTUzOTY4NjM3MTM1NS5wbmc?x-oss-process=image/format,png)

很多时候，我们希望得到一个“稳健”的模型。何为稳健？一般来说有两种含义，**一是对于参数扰动的稳定性**，比如模型变成了 fw+Δw(x) 后是否还能达到相近的效果？如果在动力学系统中，还要考虑模型最终是否能恢复到 fw(x)；**二是对于输入扰动的稳定性**，比如输入从 x 变成了 x+Δx 后，fw(x+Δx) 是否能给出相近的预测结果。

读者或许已经听说过深度学习模型存在“对抗攻击样本”，比如图片只改变一个像素就给出完全不一样的分类结果，这就是模型对输入过于敏感的案例。

**L约束**

所以，大多数时候我们都希望模型对输入扰动是不敏感的，这通常能提高模型的泛化性能。也就是说，我们希望 ||x1−x2|| 很小时：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci9kNGNhNTQ2Yy1hNGExLTQwM2UtODY0YS02NTFmZjQ0ZDEzNTIvMTUzOTY4NjM3MTc4MS5wbmc?x-oss-process=image/format,png)

也尽可能地小。当然，“尽可能”究竟是怎样，谁也说不准。**于是 Lipschitz 提出了一个更具体的约束，那就是存在某个常数 C（它只与参数有关，与输入无关），使得下式恒成立**

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci82YzEzMTcxNy1jYTc3LTRjMmUtOGNjNi1jZDE0YmU1OTZiODkvMTUzOTY4NjM3MTU1OS5wbmc?x-oss-process=image/format,png)

也就是说，希望整个模型被一个线性函数“控制”住。**这便是 L 约束了。**

**换言之，在这里我们认为满足 L 约束的模型才是一个好模型。**并且对于具体的模型，我们希望估算出 C(w) 的表达式，并且希望 C(w) 越小越好，越小意味着它对输入扰动越不敏感，泛化性越好。

## 神经网络

在这里我们对具体的神经网络进行分析，以观察神经网络在什么时候会满足 L 约束。

简单而言，我们考虑单层的全连接 f(Wx+b)，这里的 f 是激活函数，而 W,b 则是参数矩阵/向量，这时候 (3) 变为：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci9hYTliODI0NC1hY2U5LTRhZjAtYjdkYS0yNDkyOTc4M2EzM2IvMTUzOTY4NjM3MTUwNy5wbmc?x-oss-process=image/format,png)

让 x1,x2 充分接近，那么就可以将左边用一阶项近似，得到：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci82OTZkNzFhNy1mODViLTRkYTQtODE1Yy1jOTM1MWRmYjNlZDQvMTUzOTY4NjM3MTY1My5wbmc?x-oss-process=image/format,png)

显然，要希望左边不超过右边，**∂f/∂x 这一项（每个元素）的绝对值必须不超过某个常数。这就要求我们要使用“导数有上下界”的激活函数，不过我们目前常用的激活函数，比如sigmoid、tanh、relu等，都满足这个条件。**假定激活函数的梯度已经有界，尤其是我们常用的 relu 激活函数来说这个界还是 1，因此 ∂f/∂x 这一项只带来一个常数，我们暂时忽略它，剩下来我们只需要考虑 ||W(x1−x2)||。

多层的神经网络可以逐步递归分析，从而最终还是单层的神经网络问题，而 CNN、RNN 等结构本质上还是特殊的全连接，所以照样可以用全连接的结果。因此，对于神经网络来说，问题变成了：**如果下式恒成立，那么 C 的值可以是多少？**

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci82YjVkZGQ3My03ZDA5LTRhY2EtYmNiOS01MzBiMDRlOGY2ZDgvMTUzOTY4NjM3MTQ0NS5wbmc?x-oss-process=image/format,png)

**找出 C 的表达式后，我们就可以希望 C 尽可能小**，从而给参数带来一个正则化项。

## 矩阵范数

**定义**

其实到这里，我们已经将问题转化为了一个矩阵范数问题（矩阵范数的作用相当于向量的模长），它定义为：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci9iYjc0N2Q2My0zMzgxLTQ2NmUtYjEyOS03Y2UyN2ZmNmQ5YzMvMTUzOTY4NjM3MjM1MC5wbmc?x-oss-process=image/format,png)

如果 W 是一个方阵，那么该范数又称为“谱范数”、“谱半径”等，在本文中就算它不是方阵我们也叫它“谱范数（Spectral  Norm）”好了。注意 ||Wx|| 和 ||x||  都是指向量的范数，就是普通的向量模长。而左边的矩阵的范数我们本来没有明确定义的，但通过右边的向量模型的极限定义出来的，所以这类矩阵范数称为“由向量范数诱导出来的矩阵范数”。 

好了，文绉绉的概念就不多说了，有了向量范数的概念之后，我们就有：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci8xNjJiOWUzZi03NzRiLTQxNGYtYjY1Zi1hYTMwOTM5ODgxNjYvMTUzOTY4NjM3MTgzMi5wbmc?x-oss-process=image/format,png)

呃，其实也没做啥，就换了个记号而已，||W||2 等于多少我们还是没有搞出来。

**Frobenius范数**

其实谱范数 ||W||2 的准确概念和计算方法还是要用到比较多的线性代数的概念，我们暂时不研究它，而是先研究一个更加简单的范数：**Frobenius 范数，简称 F 范数。**

这名字让人看着慌，其实定义特别简单，它就是：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci8zYWM5ZWZkNy1jZGE1LTRhYmYtOTc1NS1jYzc3MTQ4YmM4M2UvMTUzOTY4NjM3MTg4Ni5wbmc?x-oss-process=image/format,png)

说白了，它就是直接把矩阵当成一个向量，然后求向量的欧氏模长。

简单通过柯西不等式，我们就能证明：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci9iODA0ZDYyZS1jM2RjLTQ0YjAtYWY4NS00NTRiNWZiMDlhZjgvMTUzOTY4NjM3NjcxNi5wbmc?x-oss-process=image/format,png)

很明显 ||W||F 提供了 ||W||2 的一个上界，也就是说，你可以理解为 ||W||2 是式 (6) 中最准确的 C（所有满足式  (6) 的 C 中最小的那个），但如果你不大关心精准度，你直接可以取 C=||W||F，也能使得 (6) 成立，毕竟 ||W||F 容易计算。

**l2正则项**

前面已经说过，为了使神经网络尽可能好地满足L约束，我们应当希望 C=||W||2 尽可能小，我们可以把 C2  作为一个正则项加入到损失函数中。当然，我们还没有算出谱范数 ||W||2，但我们算出了一个更大的上界 ||W||F，那就先用着它吧，即 loss 为：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci9iYTJjZmNjOS1kYzJiLTQwNjAtYWQ3Mi0zNmI2OWE0MTllN2EvMTUzOTY4NjM3MjU0NS5wbmc?x-oss-process=image/format,png)

其中第一部分是指模型原来的 loss。我们再来回顾一下 ||W||F 的表达式，我们发现加入的正则项是：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci85ZjQ3ZTM4Ny1lY2NiLTQ0ZGItOTM0OS0xNWNlOTExYzAyNWEvMTUzOTY4NjM3MjQ5My5wbmc?x-oss-process=image/format,png)

这不就是 l2 正则化吗？ 

终于，捣鼓了一番，我们得到了一点回报：**我们揭示了 l2 正则化（也称为 weight decay）与 L 约束的联系，表明 l2 正则化能使得模型更好地满足 L 约束，从而降低模型对输入扰动的敏感性，增强模型的泛化性能。**

### 谱范数

**主特征根**

这部分我们来正式面对谱范数 ||W||2，这是线性代数的内容，比较理论化。

事实上，**谱范数 ||W||2 等于![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci8zNzYzODIzMS05NDExLTQ3NDYtYjI1ZC1iODY2NGI5ZTdiMDcvMTUzOTY4NjM3Mjc2OS5wbmc?x-oss-process=image/format,png)的最大特征根（主特征根）的平方根**，如果 W是方阵，那么||W||2 等于 W 的最大的特征根绝对值。

对于感兴趣理论证明的读者，这里提供一下证明的大概思路。根据定义 (7) 我们有：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci81MTJiMDRlMi0wNzgyLTQ2YTYtYjc1ZC1hMmMyNzg3ODViN2QvMTUzOTY4NjM3MzM2Mi5wbmc?x-oss-process=image/format,png)

假设**![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci80ZTkxYTNjMC1mZmIyLTRmNzQtOTZmOC02NDg3NDJmODlmY2EvMTUzOTY4NjM3MjcyOC5wbmc?x-oss-process=image/format,png)**对角化为diag(λ1,…,λn)，即![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci9mMTYyODEwYS03MTUzLTRlZWQtOTA4Yy02ZTY4NzhmNjNlNjMvMTUzOTY4NjM3MjY3OS5wbmc?x-oss-process=image/format,png)，其中 λi 都是它的特征根，而且非负，而 U 是正交矩阵，由于正交矩阵与单位向量的积还是单位向量，那么：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci82ZjZmYzNkMi04MGU2LTRjNzUtYjZiMC01NjUzZTJkNDYzZWYvMTUzOTY4NjM3MzYzNC5wbmc?x-oss-process=image/format,png)

所以![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci82NDFjYzdlNy1iNWUxLTRhOTAtODAxMS02OTdhZjVkYWEyZjAvMTUzOTY4NjM3MzY3OC5wbmc?x-oss-process=image/format,png)等于**![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci84NjZhODYwOC05OTcyLTQwYTMtODdlZC1hYjQwZWJkZDRjNDAvMTUzOTY4NjM3MjgwOS5wbmc?x-oss-process=image/format,png)**的最大特征根。

**幂迭代**

也许有读者开始不耐烦了：鬼愿意知道你是不是等于特征根呀，我关心的是怎么算这个鬼范数！

事实上，前面的内容虽然看起来茫然，但却是求 ‖W‖2 的基础。前一节告诉我们![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci84ZjYwOTEyMC1hNGZiLTQzZGMtYWRhNC00ZDA5ZDVhZjI5M2YvMTUzOTY4NjM3MzczOC5wbmc?x-oss-process=image/format,png)就是**![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci8xMWM2ZGFkMS0xNWI4LTQ0MDgtOTc2Ni1iY2QzNDc4M2EwNTkvMTUzOTY4NjM3Mjg0OC5wbmc?x-oss-process=image/format,png)**的最大特征根，所以问题变成了求**![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci9hNTU0YTI5ZS1kY2ZjLTRmZGQtOWVlNS0zMzM0Y2ViMTFlMmIvMTUzOTY4NjM3Mjg5My5wbmc?x-oss-process=image/format,png)**的最大特征根，这可以通过**“幂迭代”法** [3] 来解决。

所谓“幂迭代”，就是通过下面的迭代格式：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci84OGRlMDYxOC0wMzMxLTQyOTYtOWU0Mi1lNzUyNDYzODc1OTcvMTUzOTY4NjM3MzU0MC5wbmc?x-oss-process=image/format,png)

迭代若干次后，最后通过：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci82NWQ1ZGI0Zi04ZGM4LTQ3ZDEtYTE0Ny03ZmE1YTgyNDAzODYvMTUzOTY4NjM3NDQyNC5wbmc?x-oss-process=image/format,png)

得到范数（也就是得到最大的特征根的近似值）。也可以等价改写为：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci9jNjY2YzgzMi1lYzIxLTRhYjctYjBlMy1kM2E5ZGQyOTkxMzIvMTUzOTY4NjM3NDIzMS5wbmc?x-oss-process=image/format,png)

这样，初始化 u,v 后（可以用全 1 向量初始化），就可以迭代若干次得到 u,v，然后代入![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci9mYjkyZDY4ZS1lYzRkLTQ0YTItYjYyNC1iYzBkZjM3NzE4MmMvMTUzOTY4NjM3NDQ3MC5wbmc?x-oss-process=image/format,png)算得 ‖W‖2 的近似值。

对证明感兴趣的读者，这里照样提供一个简单的证明表明为什么这样的迭代会有效。

记，初始化为![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci84ZjYzZTYyMi00YzM0LTQxNzgtYmMwNC1lZDE3ZjBiMDUyNjgvMTUzOTY4NjM3NTAwNC5wbmc?x-oss-process=image/format,png)，同样假设 A 可对角化，并且假设 A 的各个特征根 λ1,…,λn 中，最大的特征根严格大于其余的特征根（不满足这个条件意味着最大的特征根是重根，讨论起来有点复杂，需要请读者查找专业证明，这里仅仅抛砖引玉。

当然，从数值计算的角度，几乎没有两个人是完全相等的，因此可以认为重根的情况在实验中不会出现。），那么 A 的各个特征向量 η1,…,ηn 构成完备的基底，所以我们可以设：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci9hZWE5ZjdkOC05Y2ZjLTQ5YjMtYmViZS0xYThmMWFkMjVlMjYvMTUzOTY4NjM3NTE4Mi5wbmc?x-oss-process=image/format,png)每次的迭代是 Au/‖Au‖，其中分母只改变模长，我们留到最后再执行，只看 A 的重复作用：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci9kYmQ4OTc3NS1iOTM1LTQ0ZWItOThlZC00ZTQyNDBjZjAzNWIvMTUzOTY4NjM3NTMyNC5wbmc?x-oss-process=image/format,png)

注意对于特征向量有 Aη=λη，从而：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci9iYThjNGMxZS0zODk5LTQ1Y2EtOTRjNC0yNjg4YWI5YzExNDgvMTUzOTY4NjM3NDgxMC5wbmc?x-oss-process=image/format,png)不失一般性设 λ1 为最大的特征值，那么：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci80OGE3OWZjNS0wN2ZhLTRiZTUtOWM5Yy0zYjEyMjA0YjNhYTMvMTUzOTY4NjM3NTQxNy5wbmc?x-oss-process=image/format,png)

根据假设 λ2/λ1,…,λn/λ1 都小于 1，所以 r→∞ 时它们都趋于零，或者说当 r 足够大时它们可以忽略，那么就有：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci9kNzMwMmRhNy05ZGY3LTQ4NmYtODM2Ny1mYTcwMzAxOTY1M2QvMTUzOTY4NjM3NTY4OS5wbmc?x-oss-process=image/format,png)先不管模长，这个结果表明当 r 足够大时，![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci85NzQ2OTk0OC05YzMwLTQ2NTQtODY1Mi1iNWY4YWEyODE2MjQvMTUzOTY4NjM3NTQ2NS5wbmc?x-oss-process=image/format,png)提供了最大的特征根对应的特征向量的近似方向，其实每一步的归一化只是为了防止溢出而已。这样一来![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci8yZGNjMzkwYy1kODg2LTQxZTQtOGQ0Yi0zNmEzNWIyNDc3OGMvMTUzOTY4NjM3NTY0NC5wbmc?x-oss-process=image/format,png)就是对应的单位特征向量，即：![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci85ZWE3NGIxMy0zODNmLTQ0NmUtYjk5Ni1mZWM4ZTBmMjk2ZjgvMTUzOTY4NjM3NTg4Ny5wbmc?x-oss-process=image/format,png)

因此：![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci84MWQ4NjRiOC00NjRlLTRkYzgtYjAxMy1mYTViZjVjYWNmZTIvMTUzOTY4NjM3NTkzMS5wbmc?x-oss-process=image/format,png)，这就求出了谱范数的平方。

**谱正则化**

前面我们已经表明了 Frobenius 范数与 l2 正则化的关系，而我们已经说明了  Frobenius 范数是一个更强（更粗糙）的条件，更准确的范数应该是谱范数。虽然谱范数没有  Frobenius 范数那么容易计算，但依然可以通过式 (15) 迭代几步来做近似。 

所以，我们可以提出**“谱正则化（Spectral Norm Regularization）”**的概念，即把谱范数的平方作为额外的正则项，取代简单的 l2 正则项。即式 (11) 变为：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZS5qaXFpemhpeGluLmNvbS91cGxvYWRzL2VkaXRvci8wZjYwMTc2NC0wZTgzLTQ1OWYtYTg3Yy1jNTk5ODc0OTk2M2EvMTUzOTY4NjM3NjAwNC5wbmc?x-oss-process=image/format,png)

***Spectral Norm Regularization for Improving the Generalizability of Deep Learning*** [1]一文已经做了多个实验，表明“谱正则化”在多个任务上都能提升模型性能



### 生成模型

#### **WGAN**

如果说在普通的监督训练模型中，L 约束只是起到了“锦上添花”的作用，那么在 WGAN 的判别器中，L 约束就是必不可少的关键一步了。因为 WGAN 的判别器的优化目标是：

![img](https://image.jiqizhixin.com/uploads/editor/55d86993-84ed-4085-bc00-07f4d621149a/1539686376251.png)

这里的 Pr,Pg 分别是真实分布和生成分布，|f|L=1 指的就是要满足特定的 L 约束 |f(x1)−f(x2)|≤‖x1−x2‖（那个 C=1）。所以上述目标的意思是，在所有满足这个L约束的函数中，挑出使得![img](https://image.jiqizhixin.com/uploads/editor/1aa2020c-a8ac-4c20-92f3-5537ffba4dff/1539686376341.png)最大的那个 f，就是最理想的判别器。写成 loss 的形式就是：

![img](https://image.jiqizhixin.com/uploads/editor/b391a4e5-3c5d-46c3-8771-298624b8dda7/1539686376837.png)

#### **梯度惩罚**

目前比较有效的一种方案就是梯度惩罚，**即 ‖f′(x)‖=1 是 |f|L=1 的一个充分条件**，那么我把这一项加入到判别器的 loss 中作为惩罚项，即：

![img](https://image.jiqizhixin.com/uploads/editor/ac119fe8-57f1-4044-b256-be0c7dbe8137/1539686376434.png)

事实上我觉得加个 relu(x)=max(x,0) 会更好：

![img](https://image.jiqizhixin.com/uploads/editor/fc29cccc-9bb0-4d86-82f0-218fb2c7d7db/1539686376895.png)

其中![img](https://image.jiqizhixin.com/uploads/editor/0dac7e7f-db0e-4e5a-a233-97d72b9bc4e0/1539686376777.png)采用随机插值的方式：

![img](https://image.jiqizhixin.com/uploads/editor/88b4a286-b6cb-4097-8431-4abe7cc13e76/1539686377231.png)

梯度惩罚不能保证 ‖f′(x)‖=1，但是直觉上它会在 1 附近浮动，所以 |f|L 理论上也在 1 附近浮动，从而近似达到 L 约束。

这种方案在很多情况下都已经 work 得比较好了，但是在真实样本的类别数比较多的时候却比较差（尤其是条件生成）。

**问题就出在随机插值上：**原则上来说，L  约束要在整个空间满足才行，但是通过线性插值的梯度惩罚只能保证在一小块空间满足。如果这一小块空间刚好差不多就是真实样本和生成样本之间的空间，那勉勉强强也就够用了，但是如果类别数比较多，不同的类别进行插值，往往不知道插到哪里去了，导致该满足 L 条件的地方不满足，因此判别器就失灵了。

思考：梯度惩罚能不能直接用作有监督的模型的正则项呢？有兴趣的读者可以试验一下。

#### **谱归一化**

梯度惩罚的问题在于它只是一个惩罚，只能在局部生效。真正妙的方案是构造法：构建特殊的 f，使得不管 f 里边的参数是什么，f 都满足 L 约束。

事实上，WGAN 首次提出时用的是参数裁剪——将所有参数的绝对值裁剪到不超过某个常数，这样一来参数的 Frobenius 范数不会超过某个常数，从而 |f|L 不会超过某个常数，虽然没有准确地实现 |f|L=1，但这只会让 loss 放大常数倍，因此不影响优化结果。参数裁剪就是一种构造法，这不过这种构造法对优化并不友好。

简单来看，这种裁剪的方案优化空间有很大，比如改为将所有参数的 Frobenius 范数裁剪到不超过某个常数，这样模型的灵活性比直接参数裁剪要好。如果觉得裁剪太粗暴，换成参数惩罚也是可以的，即对所有范数超过 Frobenius 范数的参数施加一个大惩罚，我也试验过，基本有效，但是收敛速度比较慢。

然而，上面这些方案都只是某种近似，现在我们已经有了谱范数，那么可以用最精准的方案了：**将 f 中所有的参数都替换为 w/‖w‖2**。这就是谱归一化（Spectral Normalization），在***Spectral Normalization for Generative Adversarial Networks*** [2] 一文中被提出并实验。

这样一来，如果 f 所用的激活函数的导数绝对值都不超过 1，那么我们就有 |f|L≤1，从而用最精准的方案实现了所需要的 L 约束。

注：“激活函数的导数绝对值都不超过 1”，这个通常都能满足，但是如果判别模型使用了残差结构，则激活函数相当于是 x+relu(Wx+b)，这时候它的导数就不一定不超过 1 了。但不管怎样，它会不超过一个常数，因此不影响优化结果。

我自己尝试过在 WGAN 中使用谱归一化（不加梯度惩罚，参考代码见后面），**发现最终的收敛速度（达到同样效果所需要的 epoch）比 WGAN-GP 还要快，效果还要更好一些。**而且，还有一个影响速度的原因：就是每个 epoch 的运行时间，梯度惩罚会比用谱归一化要长，因为用了梯度惩罚后，在梯度下降的时候相当于要算二次梯度了，要执行整个前向过程两次，所以速度比较慢。
