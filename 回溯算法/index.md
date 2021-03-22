# 回溯算法


[参考](https://leetcode-cn.com/problems/permutations/solution/hui-su-suan-fa-python-dai-ma-java-dai-ma-by-liweiw/)

> 「回溯算法」与「深度优先遍历」都有「不撞南墙不回头」的意思。我个人的理解是：「回溯算法」强调了「深度优先遍历」思想的用途，用一个 **不断变化** 的变量，在尝试各种可能的过程中，搜索需要的结果。强调了 回退 操作对于搜索的合理性。
>
> - **递归之后需要做和递归之前相同的逆向操作**







# [问题1](https://leetcode-cn.com/problems/permutations/)

给定一个 **没有重复** 数字的序列，返回其所有可能的全排列。



我们尝试在纸上写 333 个数字、444 个数字、555 个数字的全排列，相信不难找到这样的方法。以数组 [1, 2, 3] 的全排列为例。

    先写以 111 开头的全排列，它们是：[1, 2, 3], [1, 3, 2]，即 1 + [2, 3] 的全排列（注意：递归结构体现在这里）；
    再写以 222 开头的全排列，它们是：[2, 1, 3], [2, 3, 1]，即 2 + [1, 3] 的全排列；
    最后写以 333 开头的全排列，它们是：[3, 1, 2], [3, 2, 1]，即 3 + [1, 2] 的全排列。

总结搜索的方法：按顺序枚举每一位可能出现的情况，已经选择的数字在 当前 要选择的数字中不能出现。按照这种策略搜索就能够做到 不重不漏。这样的思路，可以用一个树形结构表示。

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210114093647.png)

- 每一个结点表示了求解全排列问题的不同的阶段，这些阶段通过**变量**的「不同的值」体现，这些变量的不同的值，称之为「状态」；
  使用深度优先遍历有「回头」的过程，在「回头」以后， 状态变量需要设置成为和先前一样 ，因此在回到上一层结点的过程中，需要撤销上一次的选择，这个操作称之为「状态重置」；

- 深度优先遍历，借助系统栈空间，保存所需要的状态变量，在编码中只需要注意遍历到相应的结点的时候，状态变量的值是正确的，具体的做法是：<u>往下走一层的时候，path 变量在尾部追加，而往回走的时候，需要撤销上一次的选择，也是在尾部操作，因此 path 变量是一个栈；</u>

- 深度优先遍历通过「回溯」操作，实现了**全局使用一份状态变量的效果**。




**设计状态变量**(重要，必须明确状态变量)

    首先这棵树除了根结点和叶子结点以外，每一个结点做的事情其实是一样的，即：在已经选择了一些数的前提下，在剩下的还没有选择的数中，依次选择一个数，这显然是一个 递归 结构；
    
    递归的终止条件是： 一个排列中的数字已经选够了 ，因此我们需要一个变量来表示当前程序递归到第几层，我们把这个变量叫做 depth，或者命名为 index ，表示当前要确定的是某个全排列中下标为 index 的那个数是多少；
    
    布尔数组 used，初始化的时候都为 false 表示这些数还没有被选择，当我们选定一个数的时候，就将这个数组的相应位置设置为 true ，这样在考虑下一个位置的时候，就能够以 O(1)的时间复杂度判断这个数是否被选择过，这是一种「以空间换时间」的思想。

这些变量称为「状态变量」，它们表示了在求解一个问题的时候所处的阶段。需要根据问题的场景设计合适的状态变量。

```
状态变量：表示求解一个问题所处的阶段
depth:表示当前递归到了第几层,用于判断递归是否可以终止
used:表示哪几个数被使用过；初始化都为false,表示这些数都没有被选择过，当选定一个数的时候就将这个数组的相应位置设置为true
path:使用栈，往下走一层的时候，在末尾添加一个数，往回走的时候，撤销上一次的操作。
```





```java
class  Solution {
    public List<List<Integer>>  permute(int[] nums){
        int len= nums.length;
        List<List<Integer>> res=new ArrayList<>();
        if (len==0) return res;

        Deque<Integer> path=new ArrayDeque<>(len);
        boolean[] used=new boolean[len];//boolean数组初始化为false
        dfs(nums,len,0,path,used,res);
        return res;
    }
    /*
    状态变量：表示求解一个问题所处的阶段
    * depth:表示当前递归到了第几层,用于判断递归是否可以终止
    * used:表示哪几个数被使用过；初始化都为false,表示这些数都没有被选择过，当选定一个数的时候就将这个数组的相应位置设置为true
    * path:使用栈，往下走一层的时候，在末尾添加一个数，往回走的时候，撤销上一次的操作。
    * */

    private void dfs(int[] nums, int len, int depth, Deque<Integer> path, boolean[] used, List<List<Integer>> res) {
        if (depth==len) {//当depth==len表示此时递归可以终止，将路径添加当res中然后返回。
            res.add(new ArrayList<>(path));//注意变量 path 所指向的列表 在深度优先遍历的过程中只有一份
            //在 Java 中，参数传递是 值传递，对象类型变量在传参的过程中，复制的是变量的地址。这些地址被添加到 res 变量，但实际上指向的是同一块内存地址，因此我们会看到 6空的列表对象
            return;
        }

//        在非叶子节点处产生不同的分支；即在还未选择的数中选择一个元素作为下一个元素
        for (int i=0;i<len;i++){
            if (!used[i]){//如果当前数字还未被选择
                path.add(nums[i]);//则将该数字添加入路径中,即添加在栈的尾部
                used[i]=true;//将该数字的状态设置为true，表示该数字已经被使用过了

                dfs(nums, len, depth+1, path, used, res);//往下一层进行搜索，直到depth==len


                used[i]=false;//状态[回溯]，将尾部的数字设置为未被使用           //这一部分与往尾部添加元素的形式是对称的
                path.removeLast();//将尾部数字移除
            }
        }
    }
}
```



# [问题2](https://leetcode-cn.com/problems/permutations-ii/)

[参考](https://leetcode-cn.com/problems/permutations-ii/solution/hui-su-suan-fa-python-dai-ma-java-dai-ma-by-liwe-2/)

给定一个**可包含重复数字**的序列 `nums` ，按任意顺序 返回所有不重复的全排列。

`注意这里与问题1的不同之处在于，给定的数组是可能包含重复数字的。因此还需要考虑如何将重复的排列消除`

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210114112506.png)

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210114112549.png)

```java
class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        int len= nums.length;
        List<List<Integer>> res=new ArrayList<>();//保存最终结果的变量
        if (len==0) return res;

        Deque<Integer> path=new ArrayDeque<>();//保存路径的状态变量
        boolean[] used=new boolean[len];//表示该数字是否被使用过

        Arrays.sort(nums);//先对数组进行排序，以便判断是否重复的排列
        dfs(nums,len,0,path,used,res);
        return res;
    }

    private void dfs(int[] nums, int len, int depth, Deque<Integer> path, boolean[] used, List<List<Integer>> res) {
        //终止条件
        if (depth==len){
            res.add(new ArrayList<>(path));
            return;
        }

        for (int i=0;i<len;i++){
            if (!used[i]){
                if (i>0&&nums[i]==nums[i-1]&&!used[i-1]){//用于排除重复排序
                    continue;
                }
                used[i]=true;
                path.add(nums[i]);

                dfs(nums, len, depth+1, path, used, res);

                path.removeLast();
                used[i]=false;
            }
        }
    }
}
```



# [问题3](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)

输入一棵二叉树和一个整数，打印出二叉树中节点值的和为输入整数的所有路径。从树的根节点开始往下一直到叶节点所经过的节点形成一条路径。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    LinkedList<List<Integer>> result = new LinkedList<>();
    Deque<Integer> path=new ArrayDeque<>();//path,使用栈结构保存状态变量

    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        recur(root, sum);
        return result;
    }

    void recur(TreeNode root, int target) {
//采用先序遍历的方式计算->根左右
//采用减法的方式，每一次减去root.val，
        if (root == null) return;
        path.add(root.val);
        target = target - root.val;
        
        //使用target与root共同判断递归是否结束
        if (target == 0 && root.left == null && root.right == null) {//如果target==0，并且该节点为叶子结点，就添加入路径中
            result.add(new LinkedList<>(path));//要保证已添加的路径不被破坏，所以需要重新构造一个List
        }
        recur(root.left,target);//采用递归的方式，遍历左子树
        recur(root.right,target);
        
        //#################
        //回溯：递归后需要做与递归前对称的操作
        
        path.removeLast();//如果该节点是叶子节点，并且此时target！=0，则将该结点删除，以便于更新结点
        
        //为什么tar不需要在返回上一层之前恢复一下 比如我在这层减掉了root.val ， 为什么在最后不把它加回来？
        //值类型如基本数据类型传递给方法的是值的拷贝而不是实际的存储值变量的地址；而引用类型如对象引用传递给方法的是引用的拷贝。 这里int值是深拷贝 list是浅拷贝 这样最后要new 一下那里也解释通了
        

    }
}
```



# 参考
