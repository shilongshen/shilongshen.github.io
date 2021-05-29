# 剪绳子

算术几何均值不等式：
$$
\frac{n_1+n_2+...+n_a}{a}\geq a\sqrt{n_1n_2...n_a}
$$
当且仅当$n_1=n_2=...=n_a$时等号成立,$\sqrt{n_1n_2...n_a}$取最大值。

根据上述不等式可以得知，当每段绳子的相等时，乘积最大。设绳子的总长度为n，每段绳子的长度为x，共有a段绳子，即$ax=n$，则最大的乘积为$x^a$,即
$$
x^a=x^{\frac{n}{x}}=(x^{\frac{1}{x}})^{n}
$$
设$y=x^\frac{1}{x}$，问题转换为求$y$的最大值，对$y$进行分析
$$
\begin{align}
y&=x^\frac{1}{x}\\
\ln y&=\frac{1}{x} \ln x
\end{align}
$$
两端求导
$$
\begin{align}
\frac{1}{y}\dot{y}&=\frac{-1}{x^2} \ln x+\frac{1}{x^2}
\end{align}
$$
整理得
$$
\begin{align}
\dot{y}&=y(\frac{-1}{x^2} \ln x+\frac{1}{x^2})\\
\dot{y}&=x^\frac{1}{x}(\frac{1- \ln x}{x^2})
\end{align}
$$
令$\dot{y}=0$得$x=e\approx2.7$，又有当$x>e$时，$\dot{y}$小于0，当$x<e$时，$\dot{y}$大于0，所以$x=e$为最大值点。但是绳子的长度$x$必须为整数，比较
$$
y(3)=3^{\frac{1}{3}}\approx1.44
y(3)=2^{\frac{1}{2}}\approx1.41
$$
所以当$x=3$时，可以取得乘积得最大值。



切分规则：

**当将绳子尽可能多以长度为3进行切分时，乘积最大**，当存在余数得时候，存在3种情况

- 余数为0，最大乘积为$3^a$
- 余数为1，因为$2\times 2 >1\times 3$,所以最大乘积为$3^{a-1}\times 4$
- 当余数为2时，最大乘积为$3^a*2$






