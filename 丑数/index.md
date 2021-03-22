# 丑数


date: 2021-02-04T09:17:30+08:00

[参考](https://leetcode-cn.com/problems/chou-shu-lcof/solution/chou-shu-ii-qing-xi-de-tui-dao-si-lu-by-mrsate/)

可以采用动态规划的想法！

第n个丑数$x_n$，它必然为一下三种情况之一:
$$
x_n=
\left\{ \begin{array}{rcl}
x_a \times 2 \\
x_b \times 3 \\
x_c \times 5\\
\end{array}\right.
$$
其中$x_a , x_b ,x_c $是未知数。

为了保证不会遗漏某个丑数，$x_n$必然等于：
$$
x_n=min\{x_a \times 2,x_b \times 3,x_c \times 5 \}
$$

> 这里我们需要明确的是一个丑数乘上2或3或5还是一个丑数

这里因为我们要在$\{x_a \times 2,x_b \times 3, x_c \times 5\}$中挑选出最小的一个丑数，我们可以这样考虑，假设存在三个数组，分别用第一个丑数乘上2,3,5

```
nums2={1*2,2*2,3*2,4*2,5*2,6*2,8*2...}
nums3={1*3,2*3,3*3,4*3,5*3,6*3,8*3...}
nums5={1*5,2*5,3*5,4*5,5*5,6*5,8*5...}

# 注意 7 不是丑数. 
# 2, 3, 5 这前 3 个丑数一定要乘以其它的丑数， 所得的结果才是新的丑数， 所以上例中没有出现 7*2, 7*3, 7*5

```

那么， 最终的丑数序列实际上就是这 3 个有序序列对的合并结果， 计**算丑数序列也就是相当于 合并 3 个有序序列**。
合并 3 个有序序列

> 合并 3 个有序序列， 最简单的方法就是<u>每一个序列都各自维护一个指针</u>， <u>然后比较指针指向的元素的值</u>， **将最小的放入最终的合并数组中， 并将相应指针向后移动一个元素**。 这也就是：
>

```java
class Solution {
public:
	int nthUglyNumber(int n) {
		// ....
		dp[i] = min(min(dp[p2] * 2, dp[p3] * 3), dp[p5] * 5);//将最小的放入最终的合并数组中
		if (dp[i] == dp[p2] * 2) 
			p2++;
		if (dp[i] == dp[p3] * 3) 
			p3++;
		if (dp[i] == dp[p5] * 5) 
			p5++;
                  // ......
};
```

合并过程中重复解的处理

nums2, nums3, nums5 中是存在重复的解的， 例如 nums2[2] == 3*2, nums3[1] == 2*3 都计算出了 6 这个结果， 所以在合并 3 个有序数组的过程中， 还需要跳过相同的结果， 这也就是为什么在比较的时候， **需要使用 3 个并列的 if... if... if...** 而不是 if... else if... else 这种结构的原因。 当比较到元素 6 时， if (dp[i] == dp[p2] * 2)...if (dp[i] == dp[p3] * 3)... 可以同时指向 nums2, nums3 中 元素 6 的下一个元素。





最终的代码：

```java
class Solution {
    public int nthUglyNumber(int n) {
        int[] dp=new int[n];//设定一个动态数组
        dp[0]=1;//初始化
        int p2=0,p3=0,p5=0;
        for(int i=1;i<n;i++){
            dp[i]=Math.min(Math.min(dp[p2]*2,dp[p3]*3),dp[p5]*5);

            if(dp[i]==dp[p2]*2){
                p2++;
            }
            if(dp[i]==dp[p3]*3){
                p3++;
            }
            if(dp[i]==dp[p5]*5){
                p5++;
            }
        }
        return dp[n-1];
    }
}
```


