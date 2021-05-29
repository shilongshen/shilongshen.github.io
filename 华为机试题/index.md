# 



说明：

- 有两个示例题，然后是正式编程题
- 可以在本地IDE编辑，然后复制粘贴。 `除编程题外的其他题型做题时禁止跳出页面，只能在考试页面进行作答，跳出三次以上将后台记录。`
- 答案提交后无法修改，然后再点击交卷
- 做完的题可以再返回修改，注意写完每一题要点击保存并调试

```
https://www.nowcoder.com/discuss/8050
oj的java输入hasNext和hasNextLine区别

采用has xxxx的话，后面也要用next xxxx。比如前面用hasNextLine，那么后面要用 nextLine 来处理输入。
```



题目来自[牛客网](https://www.nowcoder.com/ta/huawei)

### 字符串最后一个单词的长度

[链接](https://www.nowcoder.com/practice/8c949ea5f36f422594b306a2300315da?tpId=37&tqId=21224&rp=1&ru=%2Fta%2Fhuawei&qru=%2Fta%2Fhuawei%2Fquestion-ranking&tab=answerKey)

```java
import java.util.Scanner;

public class Main{
    public static void main(String[] args){
        Scanner in=new Scanner(System.in);
        String s=in.nextLine();
        int len=s.length();
        if(len==0) {
            System.out.println(0);
        }
        else if(len==1) {
            System.out.println(1);
        }
        else{
            int p=len-1;
            int count=0;
//         System.out.println(p);
            while(s.charAt(p)!=' '&&p>=0){
                p--;
                count++;
                if(p<0){
                    break;
                }
            }
            System.out.println(count);
        
        }
       
        
    }
}
```



### 计算某字母出现的次数

[链接](https://www.nowcoder.com/practice/a35ce98431874e3a820dbe4b2d0508b1?tpId=37&tqId=21225&rp=1&ru=%2Fta%2Fhuawei&qru=%2Fta%2Fhuawei%2Fquestion-ranking&tab=answerKey)

```java
import java.util.Scanner;

public class Main{
    public static void main(String[] args){
        Scanner in=new Scanner(System.in);
        String s1=in.nextLine();
        String s2=in.nextLine();
        int count=0;
        int p=0;
//         System.out.println('A'-0);
        while(p<s1.length()){
            if((s1.charAt(p)==s2.charAt(0)||s1.charAt(p)==(s2.charAt(0)+32))||(s1.charAt(p)==s2.charAt(0)||s1.charAt(p)==(s2.charAt(0)-32))){
                count++;
                p++;
                continue;
            }
            p++;
        }
        System.out.println(count);
    }
}
```

### 字符串分割

[链接](https://www.nowcoder.com/practice/d9162298cb5a437aad722fccccaae8a7?tpId=37&tqId=21227&rp=1&ru=%2Fta%2Fhuawei&qru=%2Fta%2Fhuawei%2Fquestion-ranking&tab=answerKey)

```java
import java.util.Scanner;

public class Main{
    public static void main(String[] args){
        Scanner in=new Scanner(System.in);
        
        while(in.hasNext()){
            String s=in.nextLine();
            if(s.length()==8){
                System.out.println((s));
            }
            else if(s.length()<8){
                StringBuilder builder=new StringBuilder();
                int p=0;
                while(p<s.length()){
                    builder.append(s.charAt(p));
                    p++;
                }
                while(p<=7){
                    builder.append('0');
                    p++;
                }

                System.out.println(builder.toString());
            }
            else if(s.length()>8){
                int len=s.length();
                if(len%8==0){
                    for(int i=1;i<=len/8;i++){
                        System.out.println(s.substring(8*(i-1),8*i));
                    }
                }
                else{
                    for(int i=1;i<=len/8;i++){
                        System.out.println(s.substring(8*(i-1),8*i));
                    }
                    
                    int indexLen=len%8+((len/8)*8-1);
                    int index=1+((len/8)*8-1);
//                     System.out.printf("index=%d",index);
//                      System.out.printf("indexLen=%d",indexLen);
                    StringBuilder builder=new StringBuilder();
                    
                    while(index<=indexLen){
                        builder.append(s.charAt(index));
                        index++;
                    }
                    
                    while(index<=(len/8+1)*8-1){
                        builder.append('0');
                        index++;
                    }
                    System.out.println(builder.toString());
                    
                }      
            }
        }
    }
}
```



### 在字符串中找出最长连续字符串

[链接](https://www.nowcoder.com/practice/2c81f88ecd5a4cc395b5308a99afbbec?tpId=37&tqId=21315&rp=1&ru=%2Fta%2Fhuawei&qru=%2Fta%2Fhuawei%2Fquestion-ranking&tab=answerKey)

```java
import java.util.LinkedList;
import java.util.Scanner;

public class Main {
    public static void main(String[] args){
        Scanner in = new Scanner(System.in);
        while(in.hasNext()) {
            String s = in.nextLine();
            s += "e";//在最后加一个字符，用作终止条件的判断
            LinkedList<String> list = new LinkedList<>();
            int curLen = 0;
            int max = 0;
            StringBuilder sb = new StringBuilder();
            for (int p1 = 0; p1 < s.length(); p1++) {//遍历字符
                char cc = s.charAt(p1);
                if (isNumber(cc)) {//如果为数字
                    curLen++;//sb的长度
                    sb.append(s.charAt(p1));//添加进sb中

                } else if (!isNumber(s.charAt(p1)) && curLen != 0) {//如果不是数字且sb不为空-->执行到这说明已经找到了一个连续数字串了
                    if (curLen == max) {//如果当前字符的长度等于max，就添加进list中
                        list.add(sb.toString());
                    } else if (curLen > max) {//如果大于max，则将list中原来的字符清空，再添加新的字符
                        list.clear();
                        list.add(sb.toString());
                        max = curLen;//将max更新为curlen
                    }
                    //如果小于max，list中不进行改变
                    curLen = 0;//将curlen设为0
                    sb = new StringBuilder();//将sb清空
                    //继续寻找下一个连续数字串
                }

            }
            String output = "";
            for (int i = 0; i < list.size(); i++) {
                output += list.get(i);
            }
            output += ",";
            output += max;
            System.out.println(output);
        }
    }
    private static boolean isNumber(char c){
        return c >= '0' && c <= '9';
    }
}

```

### 质数因子

[链接](https://www.nowcoder.com/practice/196534628ca6490ebce2e336b47b3607?tpId=37&tqId=21229&rp=1&ru=%2Fta%2Fhuawei&qru=%2Fta%2Fhuawei%2Fquestion-ranking&tab=answerKey)

```java
import java.util.Scanner;

public class Main{
    public static void main(String[] args){
        Scanner in =new Scanner(System.in);
        long s=in.nextLong();
       long sqrt=(long)Math.sqrt(s);
        for(int i=2;i<=sqrt;i++){
            while(s%i==0){
                System.out.print(i+" ");
                s=s/i;
            }
        }
        //如果此时数字还没有除数，则可判定其本身是一个质数，没有再除下去的必要了，直接打印其本身即可
        System.out.println(s == 1 ? "": s+" ");//
    }
}
```

### 1

输入三行：

- 字符数组
- 需匹配的字符串
- 游标起始位置

求最少步数在字符数组中找到需匹配的字符串；在最左边再向左移动一步可以到达最右边，在最右边再向右移动一步可以到达最左边

例如

输入为：

```
aemoyn   //字符数组
amo      //需匹配的字符串
0        //游标起始位置
```

输出为：3

解释：

```
游标起始位置为0，对应为a,向右移动两步为m，再向右移动一步为o，所以总的步数为3
```

### 猜帽子数量

把数都读取进来，然后排序，从小到大匹配，如[1,1]就能匹配2个人，[2,2,2]匹配3个人。落单的说明信息缺失。

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210401101350.png)



### 足球比赛排名

踢球，胜负平各记3、0、1分；每次比赛分主客场；输入每次比赛的两个球队的进球数，输出按积分从大到小进行排序

例如

输入为：

```
b-c 4:3   //注意字母b，c表示球队名称，4，3表示各自进球数；注意输入的格式
a-b 3:0
a-c 2:1
b-a 1:1
c-a 0:1
c-b 2:2
```

输出为：

```
a 10,b 5,c 1   //注意输出的格式
```

解释：

```
b-c 4:3  -->b胜，b加3分，c加0分
a-b 3:0  -->a胜，a加3分，b加0分
a-c 2:1	 -->a胜，a加3分，c加0分
b-a 1:1  -->a、b平，a加1分，b加1分
c-a 0:1  -->a胜，a加3分，c加0分
c-b 2:2  -->b、c平，b加1分，c加1分

*****
统计可得：a得10分,b得5分,c得1分
按照积分进行输出：a 10,b 5,c 1
```

参考代码：

```java
import java.util.*;

public class Main {
    public static  void main(String[] args) {
        Scanner in=new Scanner(System.in);
        HashMap<Character,Integer> hashMap=new HashMap<>();
    
        while (in.hasNext()){//统计分数保存在hashMap中
           String s=in.nextLine();
           char[] chars1=s.toCharArray();
            if (((int) chars1[4]-'0')>((int) chars1[6]-'0')){//chars1[0]加3分，chars1[2]加0分
                hashMap.put(chars1[0], hashMap.getOrDefault(chars1[0],0)+3);
                hashMap.put(chars1[2], hashMap.getOrDefault(chars1[2], 0));
            }else if (((int) chars1[4]-'0')==((int) chars1[6]-'0')){//chars1[0]加1分，chars1[2]加1分
                hashMap.put(chars1[0], hashMap.getOrDefault(chars1[0],0)+1);
                hashMap.put(chars1[2], hashMap.getOrDefault(chars1[2], 0)+1);
            }else if (((int) chars1[4]-'0')<((int) chars1[6]-'0')){//chars1[0]加0分，chars1[2]加3分
                hashMap.put(chars1[0], hashMap.getOrDefault(chars1[0],0));
                hashMap.put(chars1[2], hashMap.getOrDefault(chars1[2], 0)+3);
        }
        
        //对hashMap按照value进行排序
        Set<Map.Entry<Character, Integer>> entrySet = hashMap.entrySet();
			//将entrySet转换为list
        List<Map.Entry<Character, Integer>> entryList=new ArrayList<>(entrySet);
			//list对value部分进行排序
        entryList.sort(new Comparator<Map.Entry<Character, Integer>>() {
            @Override
            public int compare(Map.Entry<Character, Integer> o1, Map.Entry<Character, Integer> o2) {
                return o2.getValue().compareTo(o1.getValue());//重写比较规则
            }
        }); 
            
        //输出
        StringBuilder stringBuilder = new StringBuilder();
        for (Map.Entry<Character, Integer> s : entryList) {
            stringBuilder.append(s.getKey());
            stringBuilder.append(" ");
            stringBuilder.append(s.getValue());
            stringBuilder.append(",");
        }
        String toString = stringBuilder.toString();
        System.out.println(toString.substring(0, toString.length() - 1));
        
    }
}
```




