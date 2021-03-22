# 动态规划问题


#### 动态规划解题的4个步骤

- 定义子问题(重叠子问题)
- 写出子问题的递推关系
- 确定DP数组的计算顺序
- 空间优化（可选）



#### [打家劫舍](https://leetcode-cn.com/problems/house-robber/)

采用一维数组的动态规划

```java
// 问题转化为：给定长度为n的数组，如何取最大的和（在满足条件的情况下）
class Solution {
    public int rob(int[] nums) {
        int len=nums.length;
        if(len==0) return 0;
        if(len==1) return nums[0];

        int[] dp=new int[len];
        dp[0]=nums[0];
        dp[1]=Math.max(nums[0],nums[1]);

        for(int i=2;i<len;i++){
            dp[i]=Math.max(dp[i-1],dp[i-2]+nums[i]);
        }

        return dp[len-1];
    }
}
```

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210116152741.png)

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210116152824.png)

![image-20210116152913131](/home/ssl/.config/Typora/typora-user-images/image-20210116152913131.png)





#### [打家劫舍 II](https://leetcode-cn.com/problems/house-robber-ii/)

采用两个一维数组的动态规划

```java
class Solution {
    public int rob(int[] nums) {
        int len = nums.length;
        if (len == 0) return 0;//表示没有屋子可偷
        if (len == 1) return nums[0]; //表示只有一间屋子可偷
        if (len==2) return Math.max(nums[0],nums[1]);//只有两件屋子可以偷，直接返回大者即可


//此时房子的数量大于等于3；因为首尾不能够相接，我们将其分为两种情况：
//  情况1：不偷第一家的情况下可以得到的最大值
//  情况2：不偷最后一家的情况下可以得到的最大值
//  将情况1与情况2中的大者即为所求的答案

        int[] dp = new int[len - 1];//表示不偷最后一家时可以通到的最大值

        dp[0] = nums[0];//偷第一家
        dp[1] = Math.max(nums[0], nums[1]);//初始状态

        for (int i = 2; i < len-1; i++) {

            dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i]);


        }

//
        int[] dq = new int[len - 1];
        dq[0] = nums[1];//不偷第一家
        dq[1] = Math.max(nums[1], nums[2]);//初始状态
        for (int i = 2; i < len-1; i++) {

            dq[i] = Math.max(dq[i - 1], dq[i - 2] + nums[i+1]);


        }

        return Math.max(dp[len - 2], dq[len - 2]);


    }
}

```

#### [打家劫舍 III](https://leetcode-cn.com/problems/house-robber-iii/)

采用树型结构的动态规划





#### [零钱兑换](https://leetcode-cn.com/problems/coin-change/)



```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        /*
         * 采用动态规划的方式
         * dp[i]表示目标为i时所用的最小硬币数
         * dp[i]=min(dp[i],1+dp[i-coin] for coin in coins )
         * */
        int[] dp = new int[amount + 1];
        //新状态的值要参考的值以前计算出来的「有效」状态值。
        // 因此，不妨先假设凑不出来，因为求的是小，所以设置一个不可能的数。
        Arrays.fill(dp, amount + 1);//将数组元素全部填充为amount+1

        dp[0] = 0;
        for (int i = 1; i <= amount; i++) {
            for (int coin : coins) {
                //单枚硬币的面值首先要小于等于 ->i - coin >= 0
                if (i - coin >= 0 && dp[i - coin] != amount + 1) {//
                    dp[i] = Math.min(dp[i], 1 + dp[i - coin]);
                }
            }

        }
        if (dp[amount] == amount + 1) {
            return -1;
        }
        return dp[amount];
    }
}
```





#### [n个骰子的个数](https://leetcode-cn.com/problems/nge-tou-zi-de-dian-shu-lcof/)

设共有n个骰子，每一个骰子共有6面，则总共的次数为$6^n$，设$F(n,s)$表示n个骰子出现点数为s的次数

当n=1时，$F(1,s)=1$，其中s=1,2,3,4,5,6

当$n\geq 2 $时，$F(n,s)=F(n-1,s-1)+F(n-1,s-2)+F(n-1,s-3)+F(n-1,s-4)+F(n-1,s-5)，F(n-1,s-6)$，即当第n个骰子分别为1,2,3,4,5,6时，分别计算前n-1个骰子出现的s-1,s-2,,s-3,s-4,s-5,s-6的次数，然后进行相加。

可以看出，当计算n个骰子出现和为s的情况时，是依赖与前n-1个骰子的情况的，这就存在着重叠子问题，因此可以采用动态规划的方式进行解决。

第一步：定义dp数组：设dp [i] [s] 为i个骰子出现点数s 的次数 

第二步：dp [i] [s] = dp [i] [s-1]+ dp [i] [s-2]+ dp [i] [s-3]+ dp [i] [s-4]+ dp [i] [s-5]  ... 当s-j>=i-1时 ，j表示第i个骰子的数字；因为  

```
  dp[i-1][s-j]表示i-1个骰子出现和为s-j的次数
  i-1个骰子的最小和为i-1,最大和为(i-1)*6
  因此(s-j)>=(i-1)
  所以当(s-j)<(i-1)会出错
```



![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210125155940.png)

```java
class Solution{
    public double[] dicesProbability(int n) {
        int[][] dp =new int[n+1][6*n+1];
        for (int i=1;i<=6;i++){//初始化
            dp[1][i]=1;
        }
        for (int i =2;i<=n;i++){//表示骰子的个数
            for (int s=i;s<=6*i;s++){//可能出现的点数之和;假设骰子的个数为i,那么最小的点数之和为i(即每个骰子出现的点数都为1)，最大的点数和为6*i(即每一个骰子出现的点数都为6)
                for (int j=1;j<=6;j++){//表示当前这个骰子出现的点数
                    if (s<i-1+j){
                        //dp[i-1][s-j]表示i-1个骰子出现和为s-j的次数
                        //i-1个骰子的最小和为i-1,最大和为(i-1)*6
                        //因此(s-j)>=(i-1)
                        //所以当(s-j)<(i-1)会出错
                        break;
                    }
                    dp[i][s]+=dp[i-1][s-j];
                }
            }
        }

        double total= Math.pow((double)6,(double)n);
        double[] ans=new double[5*n+1];
        for (int i=n;i<=6*n;i++){
            ans[i-n]=((double)dp[n][i])/total;
        }
        return ans;
    }
}
```



#### [把数字翻译成字符串](https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/)

假设有i个数字$x_1,x_2,...,x_{(i-1)},x_{(i)}$

假设$f(i)$表示有i个数字时的翻译方法数的表达式为：
$$
f(i)=\left\{ \begin{array}{lcl}
f(i-1)+f(i-2)  &10\times x_{(i-1)} +x_{(i)} \in [10,25]\\
f(i-1) & else

\end{array}\right.
$$


解释如下，有两种情况：

- 最后两个数字的组合的和**在**[10,25]时，有两种翻译方法，一种为最后一个数字单独翻译，此时的f(i)=f(i-1)，另一种为最后两个数字组合在一起翻译，此时的f(i)=f(i-2)，所以这种情况的翻译方法数为$f(i)=f(i-1)+f(i-2)$

- 当最后两个数字的的组合的和**不在**[10,25]时，因为最后两个数字不能够组合在一起翻译，只能够最后一个数字单独翻译，所以这种情况的翻译数为$f(i)=f(i-1)$

通过以上的分析可以得出，这个问题是存在重叠子问题的，所以可以采用动态规划的方式进行解决

定义dp[i] 表示 有i个数字时的翻译数量

注意将dp[0]初始化为1，因为很显然当i=1式只有一种翻译方法即dp[1]=1，当i=2时有两种翻译方法,即dp[2]=dp[0]+dp[1]，由此可以反推出dp[0]应该初始化为1.

代码如下：

```java
class Solution {
    public int translateNum(int num) {
        String s=String.valueOf(num);
        int[] dp=new int[s.length()+1];
        dp[0]=1;//初始化，这里将其初始化为0
        dp[1]=1;//初始化
        for (int i=2;i<=s.length();i++){
            String temp=s.substring(i-2,i);
            if (temp.compareTo("10")>=0&&temp.compareTo("25")<=0){
                dp[i]=dp[i-1]+dp[i-2];
            }
            else {
                dp[i]=dp[i-1];
            }
        }
        return dp[s.length()];
    }
}
```











# 末尾
