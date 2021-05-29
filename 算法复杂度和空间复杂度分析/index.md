# 算法复杂度分析


执行时间:   时间复杂度

内存消耗：空间复杂度



一般更关注于时间复杂度，(所以常用空间换时间)

# 时间复杂度

大$O$记号

$O(g(N))$：代表了一个复杂的表达式，是关于输入规模$N$的函数的上界

$g(N)$：关于输入规模$N$的简单表达式，$N$可以理解为输入数据的个数

> 随着输入规模的增加，程序的运行时间将以什么样的速度增加。

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210322110117.png)



## 时间复杂度的计算规则

如果一个算法执行的操作数于输入数据的规模$N$的表达式为多项式：
$$
aN^2+bN+c
$$

- 时间复杂度忽略加法常数项
- 只保留最高次幂项
- 并且最高次幂项的乘法系数视为1

所以上式可看为
$$
N^2
$$


所以时间复杂度就为$O(N^2)$

注意：时间复杂度只有当输入规模特别大时才有意义

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210322111247.png)



## 时间复杂度的数学表达式

$O(g(n))={f(n):存在正常量c和n_0，使得对所有n\geq n_0,有0 \leq f(n) \leq cg(n)}$

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210322213854.png)

$O(g(n))$是$f(n)$的上界，等价于:$\lim\limits_{n \rightarrow \infty }\frac{f(n)}{g(n)} <= c$

# 空间复杂度

空间复杂度表达了在处理过程中，使用的额外空间，也是用大$O$记号。

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210322214248.png)

# 参考

[链接](https://leetcode-cn.com/leetbook/read/learning-algorithms-with-leetcode/553v4h/)
