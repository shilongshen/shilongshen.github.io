# 

在向量[微积分](https://baike.baidu.com/item/微积分/6065)中，雅可比矩阵是一阶[偏导数](https://baike.baidu.com/item/偏导数/5536984)以一定方式排列成的[矩阵](https://baike.baidu.com/item/矩阵/18069)，其行列式称为[雅可比行列式](https://baike.baidu.com/item/雅可比行列式/4709261)。雅可比矩阵的重要性在于它体现了一个[可微](https://baike.baidu.com/item/可微/7267373)[方程](https://baike.baidu.com/item/方程/6306)与给出点的最优线性逼近。

假设矩阵方程为：
$$
\left[\begin{array}{ccc}
y_1{(x_1...x_n)} \\
y_2{(x_1...x_n)}\\
\vdots  \\
y_n{(x_1...x_n)}
\end{array}\right]
$$
则雅可比矩阵为：


$$
\left[\begin{array}{ccc}
\frac{\partial y_{1}}{\partial x_{1}} & \cdots & \frac{\partial y_{1}}{\partial x_{n}} \\
\vdots & \ddots & \vdots \\
\frac{\partial y_{m}}{\partial x_{1}} & \cdots & \frac{\partial y_{m}}{\partial x_{n}}
\end{array}\right]
$$
记为$J_{F}\left(x_{1}, \ldots, x_{n}\right)$



如果p是 $\mathbb{R}^{n}$ 中的一点, F在 p点可微分，根据高等微积分， $J_{F}(p)$ 是在这点的导数。在此情况下， $J_{F}(p)$ 这个线性映射即 $F$ 在点p附近的最优线性逼近，也就是说当X足够靠近点p时，我们有
$$
F(x) \approx F(p)+J_{F}(p) \cdot(x-p)
$$
