# 01背包问题


n件物品，它们装入背包所占的容量分别为w1、w2……wn;它们所拥有的价值分别为p1、p2 ……p	n；
有一个总容量为C的背包；

在装满背包的情况下，如何使得包内的总价值最大？

**该问题的特点是：每个物品仅有一个，可以选择放或者不放，也就是说每个物品只能使用一次。**

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210115221718.png)

首先我们定义一个变量$B(i,j)$表示当背包容量为j，前i个物品（第1个到第i个）的情况下包内的最大价值。

即有i个物品，背包的容量为j，此时的包内的最大价值。





我们对每一件物品进行编号，可以列出以下表格：

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210115221537.png)

行表示背包的容量，列表时背包的编号；为了便于操作，我们将0也加入了表格之中。

现在来分析$B(i,j)$的情况，首先明确一点，为了求出$B(i,j)$，我们可以利用之前的状态。
$$
B(i,j)=\left\{ \begin{array}{lcl}
  B(i-1,j)   	&if &w[i]>j
\\
max\{  [B(i-1,j)]; [B(i-1,j-w(i))+p(i)]  \} &if &w[i]<=j
\end{array}\right.
$$

- 对于情况$w[i]>j$表示第i个物品的重量大于背包容量，所以此时不能够放入第i个物品进背包内，只能够放弃，因此这种情况下只能够考虑前i-1个物品，即：$B(i-1,j)$
- 对于第二种情况，当前背包能够放入第i个物品。这时我们需要考虑两种情况：1.放入第i个物品；2.不放入第i个物品；因此我们需要在这两种情况中选出一个最大值；

```java
public class pack {
    public static void main(String[] args) {
        int[] w={0,2,3,4,5};//各物品体积
        int[] p={0,1,2,5,6};//各物品价值
        int c=8;//背包总容量
        int x=w.length-1;
        int[][] B=new int[x+1][c+1];

        for (int i=1;i<x+1;i++){
            for (int j=1;j<c+1;j++){
                if (w[i]>j){
                    B[i][j]=B[i-1][j];
                }
                else{
                    B[i][j]=Math.max(B[i-1][j],(B[i-1][j-w[i]])+p[i]);
                }
            }
            System.out.println(Arrays.toString(B[i]));
        }
    }
}

/*
[0, 0, 1, 1, 1, 1, 1, 1, 1]
[0, 0, 1, 2, 2, 3, 3, 3, 3]
[0, 0, 1, 2, 5, 5, 6, 7, 7]
[0, 0, 1, 2, 5, 6, 6, 7, 8]
*/
```






