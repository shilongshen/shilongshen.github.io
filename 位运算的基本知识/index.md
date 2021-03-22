# 位运算的基本知识


[参考](https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/solution/jian-zhi-offerer-shua-wei-yun-suan-java-a6nhx/)

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201230200503.png)

```
>>> 运算符会用0填充高位，这与 >> 不同，其会用符号位填充高位。不存在 <<< 运算符
```





[例1](https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/)

```java
class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        /*
        * 采用位运算的方式
        * 利用n与1进行与操作，即可判定n的最后一位是否为1，如果为1，res加1，否则不加
        * 然后将n右移一位，知道n=0
        * */
        int res=0;

        while (n!=0){
            res+=(n&1);
            n=n>>>1;
        }
        return res;
    }
}
```

[例2](https://leetcode-cn.com/problems/counting-bits/)

```java
class Solution {
    public int[] countBits(int num) {
        int[] res=new int[num+1];

        for(int i=0;i<=num;i++){
            int n=i;
            while(n!=0){
                res[i]+=(n&1);
                n=n>>>1;
            }
        }
        return res;
    }
}
```


