# 链表的复制


在这里，复制的意思是指 深拷贝（Deep Copy），类似我们常用的“复制粘贴”，事实上，与此对应的还有 浅拷贝，它们的区别是：

    浅拷贝只复制指向某个对象的指针，而不复制对象本身，新旧对象还是共享同一块内存。但深拷贝会另外创造一个一模一样的对象，新对象跟原对象不共享内存，修改新对象不会改到原对象

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210102171443.png)



1.普通单向链表的定义

```
// Definition for a Node.
class Node {
    int val;
    Node next;
    public Node(int val) {
        this.val = val;
        this.next = null;
    }
}

```

给定链表的头结点`head`，复制链表需要遍历链表，每一轮建立新节点+构建前驱结点`pre`和当前结点`node`的引用指向即可

```java
class Solution{
    public Node copy(Node head){
        Node cur=head;
        Node dum=new Node(0);//随意初始化一个结点
        Node pre=dum;
        while(cur!=null){
            Node node=new Node(cur.val);//复制当前结点，进行深拷贝
            pre.next=node;//前驱结点
            cur=cur.next;//遍历下一个结点
            pre=node;//保存当前结点
        }
        return dum.next//dum的下一个结点即为头结点。
    }
}
```

2.复制一个复杂的链表

https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/

复杂链表的定义：

```Java
/*
// Definition for a Node.
class Node {
    int val;
    Node next;
    Node random;

    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}
*/
```

解法1：利用哈希表的特性，构建原链表结点和新链表结点的对应关系，再遍历构建新链表各节点的`next`和`random`



```java
class Solution{
    public Node copy(Node head){
        if (head==null) return null;
        
        Node cur=head;
        Map<Node,Node> map=new HashMap<>();
        
        //复制结点，并建立原结点和新节点的键值对
        while(cur!=null){
            map.put(cur,new Node(cur.val));
            cur=cur.next;
        }
        
        cur=head;
        //构建链表的next和random指向
        while(cur!=null){
            map.get(cur).next=map.get(cur.next);//map.get(cur)对应value，即新链表,新链表的next等于map.get(cur.next)，即原链表的next
            map.get(cur).random=map.get(cur.random);
            cur=cur.next;            
        }
        
        return map.get(head);//返回新链表的头结点
        
    }
}
```




























































