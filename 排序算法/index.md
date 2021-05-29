# 排序算法


# 排序的稳定性

**稳定性**指的是对于序列中相同元素，经过排序后， 其前后位置并没有发生改变，则为稳定排序，否则不稳定。

<img src="https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210330153655.png" style="zoom:80%;" />

# 冒泡排序

> 两两比较相邻记录的关键字，如果反序则交换，直到没有反序的记录为止。

## 初级版本

每一个位置的数字，都与比它的下标大的数字进行比较（从前往后比较）。

```java
public class bean2 {
    public static void main(String[] args) {
        int[] nums={9,1,5,8,3,7,4,6,2};
        Solution solution=new Solution();
        System.out.println(Arrays.toString(solution.sort(nums)));
    }
}

class Solution{
    public int[] sort(int[] nums){
        for (int i=0;i<nums.length;i++){
            for(int j=i+1;j< nums.length;j++){
                if (nums[i]>nums[j]){
                    swap(i,j,nums);
                }
            }

        }
        return nums;
    }

    private void swap(int a,int b,int[] nums){//交换数组下标为a,b的两个数
        int temp=nums[a];
        nums[a]=nums[b];
        nums[b]=temp;
    }
}
```

## 冒泡排序

将最小值依次从底端交换到顶端，这一过程形象的称为冒泡排序

```java
class Solution{
    public int[] sort(int[] nums){
        for (int i=0;i<nums.length;i++){
            for (int j= nums.length-2;j>=i;j--){
                if (nums[j]>nums[j+1]){
                    swap(j+1,j,nums);
                }
            }
        }
        return nums;
    }
    private void swap(int a,int b,int[] nums){
        int temp=nums[a];
        nums[a]=nums[b];
        nums[b]=temp;
    }
}
```

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210322224825.png)

## 冒泡排序的优化

如果数组为[2,1,3,4,5,6,7,8,9],那么我们只需要进行一次排序(i==0)，将1，2进行交换，就可以得到排序的数组；不需要再进行`i==1,i==2,...,i==nums.length-1`这些循环了。

为了达到这一个目的，可以设置一个flag.。

例如[2,1,3,4,5,6,7,8,9]，当`i==0`时，flag==true，会进行循环，当`i==1`,flag==true，会进行循环，但是由于没有交换，flag设置为false了，所以`i==2,i==3,...i=nums.length-1`这些都不用执行了。

```java
class Solution{
    public int[] sort(int[] nums){
        boolean flag=true;//通过设置flag，可以避免无意义的交换运算。
        for (int i=0;i<nums.length&&flag==true;i++){//只有当flag==true时才进行遍历
            flag=false;//将其设置为false
            for (int j= nums.length-2;j>=i;j--){
                if (nums[j]>nums[j+1]){
                    swap(j,j+1,nums);
                    flag=true;//如果进行交换了才将flag设为true
                }
            }
        }

        return nums;
    }
    private void swap(int a,int b,int[] nums){
        int temp=nums[a];
        nums[a]=nums[b];
        nums[b]=temp;
    }
}
```

## 冒泡排序复杂度分析

 根据优化的冒泡排序，当待排序数组的数组本身就是有序的，只需要进行n-1次比较，时间复杂度为$O(n)$;当待排序数组的数组是逆序的时候，需要进行$1+2+3+...+(n-1)$次比较，总的比较次数为$\frac{n(n-1)}{2}$，时间复杂度为$O(n^2)$。

#  简单选择排序

> 通过n-i次关键字间的比较，从n-i+1个记录中选出关键字最小的记录，并和第i个记录交换。

```java
class Solution4{
    public int[] sort(int[] nums){
        for (int i=0;i<nums.length;i++){
            int min=i;
            for (int j=i;j< nums.length;j++){
                if (nums[min]>nums[j]){
                   min=j;
                }
            }

            if (i!=min){
                swap(min,i,nums);
            }
        }
        return nums;
    }

    private void swap(int a,int b,int[] nums){
        int temp=nums[a];
        nums[a]=nums[b];
        nums[b]=temp;
    }
}
```

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210323085018.png)

## 简单选择排序复杂度分析

简单选择排序的特点是交换的次数少。分析复杂度可以发现，无论最好或最坏的情况，其比较次数都是一样多的，第i趟排序需要进行n-i次比较，因此总共需要进行$\frac{n(n-1)}{2}$次比较，时间复杂度为$O(n^2)$。

常见问题：对含有31个元素的序列采用直接选择排序算法排序，在最坏情况下需要进行多少次**移动**才能完成排序？回答：90次，一次交换，三次移动，因为交换需要借助辅助变量；最坏的情况是数组是倒序排列的，这时需要进行n-1次交换，因为每一次交换需要借助辅助变量，所以移动的次数为3(n-1)

# 直接插入排序法

> 将一个记录插入到已经排好序的有序表中，从而得到一个新的，记录数+1的有序表--维护一个有序表

```java
class Solution{
    public int[] sort(int[] nums){
        for(int i=1;i<nums.length;i++){
            if(nums[i]<nums[i-1]){//如果nums[i]小于有序表的最后一个元素，此时就需要将nums[i]插入到有序表中，即将nums[i]放在有序表的上方。
                int temp=nums[i]  //使用temp中间存储变量nums[i]
                for(int j=i;j>=0;j++){//依次将temp与有序表中的元素进行比较
                    if(j>0&&nums[j-1]>temp){//如果temp小于有序表的最后一个元素，就将有序表的最后一个元素下移一位
                        nums[j]=nums[j-1];
                    }
                    else{
                        nums[j]=temp;
                        break;
                    }
                }
            }                
        }
        return nums;          
    }
}
```

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210323094448.png)



另一种写法([参考](https://www.runoob.com/data-structures/insertion-sort.html))：

```java
class Solution6{
    public int[] sort(int[] nums){
        for(int i=1;i< nums.length;i++){//i从1开始，表示假设初始时nums[0]是一个有序表
            for (int j=i;j>0;j--){
                if (nums[j]<nums[j-1]){//如果前一个数nums[j-1]比nums[j]，就将两者进行交换
                    swap(j,j-1 ,nums);
                }
                else {
                    break;
                }
            }
        }
        return nums;
    }
    private void swap(int a,int b,int[] nums){
        int temp=nums[a];
        nums[a]=nums[b];
        nums[b]=temp;
    }
}
```



## 复杂度分析

最坏的情况，即数组为逆序，例如[9,8,7,6,5,4,3,2,1]，此时需要比较$2+3+4+...+n=\frac{(n+2)(n-1)}{2}$，时间复杂度为$O(n^2)$



# 希尔排序

[参考](https://www.runoob.com/data-structures/shell-sort.html)

希尔排序是插入排序的一种，它是针对直接插入排序算法的改进。

它通过比较相距一定间隔的元素来进行，各趟比较所用的距离随着算法的进行而减小，**直到只比较相邻元素**（这就是直接插入排序）的最后一趟排序为止。

图例：

希尔排序目的为了加快速度改进了插入排序，交换不相邻的元素对数组的局部进行排序，并最终用插入排序将局部有序的数组排序。

在此我们选择增量 **gap=length/2**，缩小增量以 **gap = gap/2** 的方式，用序列 **{n/2,(n/2)/2...1}** 来表示。

<img src="https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210323145736.png" style="zoom:80%;" />

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210323145808.png)

```java
class Solution7{
    public int[] sort(int[] nums){
        for (int gap= nums.length/2;gap>0;gap/=2){
            for (int i=gap;i< nums.length;i++){//内层就是一个直接插入排序
                 for (int j=i-gap;j>=0;j-=gap){
                     if (nums[j]>nums[j+gap]){
                         swap(j,j+gap,nums);
                     }
                 }
            }
        }
        return nums;
    }

    private void swap(int a,int b,int[] nums){
        int temp=nums[a];
        nums[a]=nums[b];
        nums[b]=temp;
    }
}
```

说明：

- 最后一个增量必须为1

## 复杂度分析

[参考](https://www.jianshu.com/p/d730ae586cf3)

**希尔排序的执行时间依赖于增量序列。    **
 希尔排序耗时的操作有：比较 + 后移赋值。

**时间复杂度情况如下：**（n指待排序序列长度）
 **1) 最好情况：**序列是正序排列，在这种情况下，需要进行的比较操作需（n-1）次。后移赋值操作为0次。即O(n)
 **2) 最坏情况：**O(nlog2n)。
 **3) 渐进时间复杂度（平均时间复杂度）：**O(nlog2n)

希尔排序是按照不同步长对元素进行插入排序，当刚开始元素很无序的时候，步长最大，所以插入排序的元素个数很少，速度很快；当元素基本有序了，步长很小，插入排序对于有序的序列效率很高。所以，**希尔排序的时间复杂度会比O(n²)好一些。**
 希尔算法的性能与所选取的增量（分组长度）序列有很大关系。只对特定的待排序记录序列，可以准确地估算比较次数和移动次数。想要弄清比较次数和记录移动次数与增量选择之间的关系，并给出完整的数学分析，至今仍然是数学难题。

希尔算法在最坏的情况下和平均情况下执行效率相差不是很多，与此同时快速排序在最坏的情况下执行的效率会非常差。希尔排序没有快速排序算法快，因此中等大小规模表现良好，对规模非常大的数据排序不是最优选择。
 （**注：**专家们提倡，几乎任何排序工作在开始时都可以用希尔排序，若在实际使用中证明它不够快，再改成快速排序这样更高级的排序算法。）

# 堆排序

[参考](https://blog.csdn.net/qq_36186690/article/details/82505569)

> 堆（优先队列）：堆是具有以下性质的完全二叉树：每个结点的值都大于或等于其左右孩子结点的值，称为大项堆；或者每个结点的值都小于或等于其左右孩子结点的值，称为小项堆。

堆的一些特性，假设大顶堆为：

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210323162651.png)

将其转换为数组形式为:

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210323162732.png)

通过数学表达式进行描述为：
$$
arr[i]>=arr[2i+1] \\ arr[i]>=arr[2i+2] 
$$

> 因为堆是一棵完全二叉树，对于一个节点i，其左孩子一定为2i+1,右孩子一定为2i+2
>
> 可以理解为：堆是建立再数组上的树结构

堆排序的思想：（假设利用大项堆）将待排序的数列构造成一个大项堆。此时，整个数列的最大值就是堆顶元素。将它移走，然后将剩余的n-1个序列重新够造成一个堆，这样就会得到n个元素中的次大元素。如此反复进行，就能够得到一个有序序列。



```java
class Solution8{

    public int[] sort(int[] nums){
        for (int i= nums.length/2-1;i>=0;i--){//将无序数组调整为大顶堆
            HeapAdjust(nums,i, nums.length);//从第一个非叶子节点开始，从下到上，从右到左进行调整;就是将每一个非叶子节点是为根节点，将其和其子树调整为大顶堆
        }
        for(int j= nums.length-1;j>0;j--){
            swap(0,j,nums);//将堆顶元素与末尾元素进行交换
            HeapAdjust(nums,0,j);//将数组中剩余元素调整为大顶堆   -->注意因为其余的子树已经分别是大顶堆了，所以子需要对根节点进行调整即可
        }

        return nums;
    }
    private void HeapAdjust(int[] nums,int s,int len){
        int temp=nums[s];
        for (int i=2*s+1;i<len;i=2*s+1){//当前节点为s，它的左孩子为2*s+1,右孩子为2*s+2;它们的孩子也是以2*s+1进行增长
            if (i+1<len&&nums[i]<nums[i+1]){//如果左孩子的值小于右孩子的值，将i指向右孩子
                i++;
            }
            if (nums[i]>temp){//如果孩子节点大于父节点，将孩子节点的值赋值给父节点
                nums[s]=nums[i];
                s=i;//将父节点的值赋值给孩子节点
            }else {//如果孩子节点小于父节点，直接退出
                break;
            }
        }
        nums[s]=temp;
    }
    private void swap(int a,int b,int[] nums){
        int temp=nums[a];
        nums[a]=nums[b];
        nums[b]=temp;
    }
}
```

## 复杂度分析

在正式排序时，第i次取堆顶元素重建堆需要用$O(\log i)$的时间，（完全二叉树的某个节点到根节点的距离为$[\log_2 i]+1$），并且需要取n-1次堆顶记录，所以堆排序的时间复杂度为$O(n\log n)$

# 归并排序

[参考](https://blog.csdn.net/engineerhe/article/details/104065282)

 原理：假设初始序列含有n个记录，则可以看成是n个有序的子序列，每个子序列的长度为1，然后两两归并，得到[n/2]个长度为2或1的有序子序列，再两两归并，如此反复，直到得到一个长度为n的有序序列为止。 

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210323204438.png)

```java
class Solution9{
    public int[] sort(int[] nums){
        if (nums.length<2) return nums;
        mergeSort(nums,0,nums.length-1);
        return nums;
    }

    private void mergeSort(int[] nums,int left, int right) {
        if (left>=right){
            return;
        }

        int mid=left+(right-left)/2; // 计算中间位置
        mergeSort(nums,left,mid); // 左侧子数组
        mergeSort(nums,mid+1,right); // 右侧子数组  
        merge(nums,left,mid,right);  // 合并子数组
    }

    private void merge(int[] nums, int left, int mid, int right) {
        int[] temp=new int[right-left+1];//定义辅助数组
        // 定义两个指针，分别指向第一个子数组和第二个子数组的开始（这里mid+1就相等于第二个子数组开始）
        int p1=left;
        int p2=mid+1;
        int index=0;// 定义辅助数组移动的下标

        // 向temp中添加数字，任何一个子数组指向末尾的时候结束此while
        while (p1<=mid&&p2<=right){
            if (nums[p1]<nums[p2]){
                temp[index]=nums[p1];
                index++;
                p1++;
            }else {
                temp[index]=nums[p2];
                index++;
                p2++;
            }
        }
        // 这里的两个while是为了把剩下的一个子数组中的数字添加到temp中
        while (p1<=mid){
            temp[index]=nums[p1];
            index++;
            p1++;
        }
        while(p2<=right){
            temp[index]=nums[p2];
            index++;
            p2++;
        }

         // 把辅助数组中的数更新到目标数组中，这里需要注意目标数组更新的位置是left开始不是0开始。
        for (int i=0;i< temp.length;i++){
            nums[left+i]=temp[i];
        }
    }
}
```

假设待排序的数组为[8,4,5,7,1,3,6,2],则具体的执行步骤如下：

```java
left=0,right=7,mid=3
    mergeSort(nums,0,3) //-->处理|8|4|5|7|
    	left=0,right=3,mid=1
    	mergeSort(nums,0,1)  //-->处理|8|4|
    		left=0,right=1,mid=0
    		mergeSort(nums,0,0)
    			return
    		mergeSort(nums,1,1)
    			return
    		merge(nums,0,0,1)
    	mergeSort(nums,2,3)  //-->处理|5|7|
    		left=2,right=3,mid=2
    		mergeSort(nums,2,2)
    			return
    		mergeSort(nums,3,3)
    			return
    		merge(nums,0,1,3)
    mergeSort(nums,4,7) //-->处理|1|2|6|2|
    	//......
    merge(nums,0,7,3)
```

## 复杂度分析

时间复杂度为 $O(n \times log(n))$，但是需要损耗空间，其空间复杂度为$ O ( n )$，即需要一个额外的数组进行对子数组进行排序；

# 快速排序

基本思想：通过一趟排序将待排记录分割成独立的两部分，其中一部分记录的关键字均比另一部分记录的关键字小，则可分别对这两部分记录进行排序，以达到整个序列有序的目的。

```java
class Solution10{
    public int[] sort(int[] nums){
        Qsort(nums,0,nums.length-1);
        return nums;
    }

    private void Qsort(int[] nums, int low, int high) {
        if (low<high){
            int pivot=Partition(nums,low,high);//求出中枢值
            Qsort(nums, low, pivot-1);//对中枢值左边的数组进行排序
            Qsort(nums, pivot+1, high);//对中枢值右边的数组进行排序
        }
    }

    private int Partition(int[] nums, int low, int high) {
        int pivotKey=nums[low];//设置中枢值pivotKey为nums[low]，目标为pivotKey左边的数都小于它，右边的数都大于它
        while(low<high){//从左右分别进行比较
            while (low<high&&nums[high]>=pivotKey){//从右边进行比较
                high--;
            }
            swap(low,high,nums);//当nums[high]<pivotKey,就将pivotKey与nums[high]交换
            while (low<high&&nums[low]<=pivotKey){//从左边进行比较
                low++;
            }
            swap(low,high,nums);//当nums[low]>pivotKey,就将pivotKey与nums[low]交换
        }
        return low;//返回最终的中枢值的位置
    }

    private void swap(int a,int b,int[] nums){
        int temp=nums[a];
        nums[a]=nums[b];
        nums[b]=temp;
    }
}
```

## 复杂度分析  

最优的时间复杂度为$O(n\log n)$，最差的时间复杂度为$O(n^2)$，,空间复杂度为$O(\log n)$

因为关键字的比较和交换是跳跃进行的，所以快速排序是不稳定的。  

# 小结

## 常用排序算法的分类

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210323222105.png)

## 复杂度比较

<img src="https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210324101629.png" style="zoom:150%;" />

> 简单排序：冒泡排序，简单选择排序，直接插入排序
>
> 改进算法：希尔排序，堆排序，归并排序，快速排序
>
> 从待排序记录的个数上来说，待排序的个数n越小，采用简单排序方法越合适。
>
> 只要是存在跳跃交换元素的排序算法都是不稳定的，例如简单选择排序，希尔排序，堆排序，快速排序   

## 动画演示

[链接](https://www.cs.usfca.edu/~galles/visualization/ComparisonSort.html)
