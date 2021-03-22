# 背包问题


# 0-1背包



![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210302081610.png)

背包问题的关键因素：

- 背包的重量
- 物品的重量以及物品的价值
- 如果物品不可以重复放入则是0-1背包问题     -->背包进行倒序遍历
- 每个物品只有两种状态：放入背包/不放入背包   -->max(dp[i-1][j],dp[i-1][j-weight[i]]+value[i])

使用二维数组进行解决

```java

package Dynamic;

import java.util.Arrays;

public class bean12 {
    public static void main(String[] args) {
        int[] weight={1,3,4};
        int[] value={15,20,30};
        int bagweight=4;

        int[][] dp=new int[weight.length][bagweight+1];

        //第0列初始化为0，默认就是0
//        for (int i=0;i<=weight.length;i++){
//            dp[i][0]=0;//将dp[i][0]初始化为0
//        }

        for (int j=bagweight;j>=weight[0];j--){
            dp[0][j]=dp[0][j-weight[0]]+value[0];
        }

        System.out.println(Arrays.toString(dp[0]));

        for (int i=1;i<weight.length;i++){//先遍历物品
            for (int j=1;j<=bagweight;j++){//遍历背包--->取1
                if (j<weight[i]) {
                    dp[i][j]=dp[i-1][j];
                }else {
                    dp[i][j]=Math.max(dp[i-1][j],dp[i-1][j-weight[i]]+value[i]);//对应着该该物品的两种状态:放入背包/不放入背包
                }
            }

            System.out.println(Arrays.toString(dp[i]));
        }
    }
}

```

使用一维数组进行优化

**注意一定是先遍历物品再遍历背包，且遍历背包是进行倒序遍历。**

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210301195208.png)

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210302095026.png)

```java
package Dynamic;

public class bean13 {
    public static void main(String[] args) {
        int[] weight={1,3,4};
        int[] value={15,20,30};
        int bagweight=4;
        int[] dp=new int[bagweight+1];
        dp[0]=0;
        for (int i=0;i<weight.length;i++){
            for (int j=bagweight;j>=weight[i];j--){
                dp[j]=Math.max(dp[j],dp[j-weight[i]]+value[i]);
            }
        }
        System.out.println(dp[bagweight]);
    }
}
```



### 0-1背包问题可以分为以下几类

- 使得背包内物品的价值最大-->`dp[j] = max { dp[j]  ,  dp[ j - weight[i] ]+ value[i] }`
- 装满背包有几种方式 -->`dp[j] = dp[j] + dp[ j - weight[i] ]`

关于第二种情况，请[参考](https://leetcode-cn.com/problems/target-sum/solution/mu-biao-he-by-shilongshen-uz0y/)

# 完全背包

**完全背包和01背包问题唯一不同的地方就是，每种物品有无限件**。这体现在遍历顺序上。

0-1背包的核心代码：

```java
for(int i = 0; i < weight.size(); i++) { // 遍历物品
    for(int j = bagWeight; j >= weight[i]; j--) { // 遍历背包容量
        dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
    }
}
```

我们知道01背包内嵌的循环是从大到小遍历(先遍历物品再遍历背包容量)，为了保证每个物品仅被添加一次。

而完全背包的物品是可以添加多次的，**所以要从小到大去遍历**，即：

```java
// 先遍历物品，再遍历背包
for(int i = 0; i < weight.size(); i++) { // 遍历物品
    for(int j = weight[i]; j < bagWeight ; j++) { // 遍历背包容量，注意起始点为j = weight[i]，这是为了保证背包容量大于物品容量
        dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);

    }
}
```

```java
//可以写成
for(int i = 0; i < weight.size(); i++) { // 遍历物品
    for(int j =0; j < bagWeight ; j++) { // 遍历背包容量
        if(j>= weight[i]){
            dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
        }
    }
}
```



另外一个重要的不同之处在于

- 在0-1背包中，一维数组的解法必然是先遍历物品再遍历背包  -->为了保证一个背包中可以放入多个物品
- 在完全背包中，一维数组的解法既可以先遍历物品再遍历背包，也可以先遍历背包再遍历物品。

所以还可以写成

```java
for(int j=0;j<badweight;j++){
    for(int i=0;i<weight.size();i++){
        if(j>=weight(i)){
         	dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);	   
        }
    }
}
```



### 关于完全背包求方法数的排列和组合问题

完全背包问题求**组合数**：求方法数

```
1.先遍历物品再遍历背包，这是为了保证所求结果为组合数
2.背包的遍历顺序为正序遍历，这是王权·保证完全背包满足，即物品是无限的
3.求方法数的递推表达式为dp[j]=dp[j]+dp[j-nums[i]]
```

完全背包问题求**排列数**：求方法数

```
1.先遍历背包再遍历物品，这是为了保证所求结果为排列数 -->注意
2.背包的遍历顺序为正序遍历，这是保证完全背包满足，即物品是无限的
3.求方法数的递推表达式为dp[j]=dp[j]+dp[j-nums[i]]
```



# 小结

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210304103443.png)

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210304103550.png)

# 参考

[链接](https://mp.weixin.qq.com/s/akwyxlJ4TLvKcw26KB9uJw)

[链接](https://shilongshen.github.io/01%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98/)

[链接](https://mp.weixin.qq.com/s/FwIiPPmR18_AJO5eiidT6w)

[链接](https://leetcode-cn.com/problems/partition-equal-subset-sum/solution/0-1-bei-bao-wen-ti-xiang-jie-zhen-dui-ben-ti-de-yo/)
