# 广度优先问题问题


#### [机器人的运动范围](https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/)

**### 解题思路**

采用广度优先搜索的方式

广度优先通常采用队列的方式，以一种平铺的方式进行计算

移动方向：我们可以只定义向下和向右进行移动

```java
class Solution {
    public int movingCount(int m, int n, int k) {
        Deque<int[]> deque = new ArrayDeque<>();//定义一个队列，由于添加删除状态
        int[] dx = {1, 0};//右移一位
        int[] dy = {0, 1};//下移一位
        boolean[][] vis = new boolean[m][n];//定义一个boolean二维数组用于确定该坐标是否被添加
        vis[0][0] = true;//坐标[0][0]一定会被使用的，因此将其初始化为true
        deque.offer(new int[]{0, 0});//添加坐标[0][0]
        int ans = 1;
        while (!deque.isEmpty()) {
            int[] c = deque.poll();//将对头的元素poll
            int cx = c[0];//获得其x坐标
            int cy = c[1];//获得其y坐标
            for (int i = 0; i < 2; i++) {//分别向右和向下进行查询
                int tx = dx[i] + cx;//下一步的x坐标
                int ty = dy[i] + cy;//下一步的y坐标
                if (tx < 0 || tx >= m || ty < 0 || ty >= n || vis[tx][ty] == true || getsum(tx) + getsum(ty) > k) {
//  特例情况的判断，包括了越界：tx<0||tx>m||ty<0||ty>n
//                    该坐标已经被添加了
//                    该坐标的数位和大于k
                    continue;
                }
                deque.offer(new int[]{tx, ty});
                vis[tx][ty] = true;
                ans++;
            }
        }
        return ans;
    }

    private int getsum(int s) {//求数字位数和,例45 --> 位数和为4+5=9
        int res = 0;
        while (s != 0) {
            res += s % 10;//求个位数
            s = s / 10;//将数字右移一位
        }
        return res;
    }
}
```


