# MySQL


# 基本结构

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210412102809.png)

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210412102844.png)

# 基本概念

## 事务

事务是一种机制，用来管理必须成批执行DML语句（insert,update,delete）（对表的数据进行增删改）.

利用事务处理，可以保证一组操作要么作为整体执行，要么不执行。

如果没有错误发生，则将整组语句提交给数据库表。如果发生错误，则回退以恢复数据库到某种安全的状态

管理事务处理的关键在于将SQL语句组分为逻辑块，并明确规定数据何时应该回退，何时不应该回退

常见术语：

- 事务(transaction)：一组SQL语句
- 回退(rollback)：指撤销指定SQL语句的过程；rollback只能在一个事务处理内使用(在执行一条start transaction;命令后)
- 提交(commit)：指将未存储的SQL语句结果写入数据库表；在事务中提交不会隐式执行，需要使用commit明确执行
- 保留点(savepoint)：指事务处理中的临时占位符，你可以对它发布回退

```mysql
select * from orders;  #显示该表不为空
start transaction;  #开始事务
delete from orders; #删除表中所有行
select * from orders;  #显示该表为空
rollback;  #回退到start transaction之后的所有语句  -->即delete from orders;select * from orders;这两条语句被撤销了
select * from orders;  #显示该表不为空

#最终的结果是该表不为空
```



```mysql
start transaction;#开始事务
delete from orders where order_num=200;#删除表中某一行  
delete from orders where order_num=500;#删除表中某一行
commit;#提交；只有当两个删除语句都执行成功，才会对表进行更改，如果存在一个语句执行不成功，那么所有删除语句都不会执行；
```

```mysql
savepoint delete1;#创建保留点
rollback to delete1;#回退到保留点
```

## 事务的隔离级别

[参考](https://baijiahao.baidu.com/s?id=1662096005584873447&wfr=spider&for=pc)

[参考](https://www.cnblogs.com/limuzi1994/p/9684083.html)

事务具有原子性（Atomicity）、一致性（Consistency）、隔离性（Isolation）、持久性（Durability）四个特性，简称 ACID，缺一不可。

### 1、原子性（Atomicity）

原子性是指事务包含的所有操作要么全部成功，要么全部失败回滚，因此事务的操作如果成功就必须要完全应用到数据库，如果操作失败则不能对数据库有任何影响。

### 2、一致性（Consistency）

一致性是指事务必须使数据库从一个一致性状态变换到另一个一致性状态，也就是说一个事务执行之前和执行之后都必须处于一致性状态。举例来说，假设用户A和用户B两者的钱加起来一共是1000，那么不管A和B之间如何转账、转几次账，事务结束后两个用户的钱相加起来应该还得是1000，这就是事务的一致性。

### 3、隔离性（Isolation）

**隔离性是当多个用户并发访问数据库时，比如同时操作同一张表时，数据库为每一个用户开启的事务，不能被其他事务的操作所干扰，多个并发事务之间要相互隔离**。关于事务的隔离性数据库提供了多种隔离级别，稍后会介绍到。

### 4、持久性（Durability）

持久性是指一个事务一旦被提交了，那么对数据库中的数据的改变就是永久性的，即便是在数据库系统遇到故障的情况下也不会丢失提交事务的操作。例如我们在使用JDBC操作数据库时，在提交事务方法后，提示用户事务操作完成，当我们程序执行完成直到看到提示后，就可以认定事务已经正确提交，即使这时候数据库出现了问题，也必须要将我们的事务完全执行完成。否则的话就会造成我们虽然看到提示事务处理完毕，但是数据库因为故障而没有执行事务的重大错误。这是不允许的。

> 原子性：事务所包含的操作，要么都成功，要么都不成功
>
> 一致性：事务使数据库从一个一致性状态转换到另一个一致性状态，例如A,B账户的总额使1000，无论A,B之间如何交易，总额还是1000
>
> 隔离性：一个事务对数据进行操作时，不会被其他事务所影响
>
> 持久性：事务对数据库的改变是永久的。

**概念说明**

以下几个概念是事务隔离级别要实际解决的问题，所以需要搞清楚都是什么意思。

**脏读**   -->读到了其他事务未提交的数据

脏读（Dirty Read）：A事务读取B事务尚未提交的数据并在此基础上操作，而B事务执行回滚，那么A读取到的数据就是脏数据。 
![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210411213648.png) 
解决办法：如果在第一个事务提交前，任何其他事务不可读取其修改过的值，则可以避免该问题。

**可重复读**  -->一个事务中两次读取到的数据是一样的

可重复读指的是在一个事务内，最开始读到的数据和事务结束前的任意时刻读到的同一批数据都是一致的。通常针对数据**更新（UPDATE）**操作。

> 可重复读是指，<u>事务不会读到其他事务对已有数据的修改</u>，即使其他事务已提交，也就是说，事务开始时读到的已有数据是什么，在事务提交前的任意时刻，这些数据的值都是一样的。但是，对于其他事务新**插入的数据**是可以读到的，这也就引发了幻读问题。
>
> -->其他事务修改是不可读的，但是其他事务插入数据是可读的。

**不可重复读**   -->在一个事务中，两次读取到的数据是不一样的

对比可重复读，不可重复读指的是在同一事务内，不同的时刻读到的同一批数据可能是不一样的，可能会受到其他事务的影响，比如其他事务改了这批数据并提交了。通常针对数据**更新（UPDATE）**操作。

**一个事务对同一行数据重复读取两次，但是却得到了不同的结果**。事务A读取某一数据后，事务B对其做了修改，当事务A再次读该数据时得到与前一次不同的值。 
![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210411214146.png) 
解决办法：如果只有在修改事务完全提交之后才可以读取数据，则可以避免该问题。

**幻读**   -->在一个事务提交之前，别的事务对数据进行了插入

> 幻读是针对数据**插入（INSERT）**操作来说的。所谓幻读，就是指当某个事务在读取某个范围内的数据时，另外一个事务又在该范围内插入了新的数据，当之前的事务再次读取该范围的数据时，会产生幻行。
>
> 
>
> ![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210411224542.png)

### 事务隔离级别

SQL 标准定义了四种隔离级别，MySQL 全都支持。这四种隔离级别分别是：

| 读未提交（READ UNCOMMITTED） | 事务可以读取未提交的数据，会产生脏读                         |
| ---------------------------- | ------------------------------------------------------------ |
| 读提交 （READ COMMITTED）    | 读提交就是一个事务只能读到其他事务已经提交过的数据，也就是其他事务调用 commit 命令之后的数据。会产生不可重复读 |
| 可重复读 （REPEATABLE READ） | 可重复读是指在一个事务内，多次读同一数据。在这个事务还没有结束时，另外一个事务也访问该同一数据。那么，在第一个事务中的两次读数据之间，即使第二个事务对数据进行修改，第一个事务两次读到的的数据是一样的。这样就发生了**在一个事务内两次读到的数据是一样的，因此称为是可重复读**。读取数据的事务将会禁止写事务（但允许读事务），写事务则禁止任何其他事务。这样避免了不可重复读取和脏读，但是有时可能出现幻象读。（读取数据的事务）这可以通过“共享读锁”和“排他写锁”实现。  --> |
| 串行化 （SERIALIZABLE）      | 提供严格的事务隔离。它要求事务序列化执行，**事务只能一个接着一个地执行，但不能并发执行**。如果仅仅通过“行级锁”是无法实现事务序列化的，必须通过其他机制保证新插入的数据不会被刚执行查询操作的事务访问到。序列化是最高的事务隔离级别，同时代价也花费最高，性能很低，一般很少使用，在该级别下，事务顺序执行，不仅可以避免脏读、不可重复读，还避免了幻像读。 |

从上往下，隔离强度逐渐增强，性能逐渐变差。采用哪种隔离级别要根据系统需求权衡决定，其中，**可重复读**是 MySQL 的默认级别。

**事务隔离其实就是为了解决上面提到的脏读、不可重复读、幻读这几个问题**，

只有串行化的隔离级别解决了全部这 3 个问题，其他的 3 个隔离级别都有缺陷。

## 表锁、行锁

[参考](https://www.jianshu.com/p/8845ddca3b23)

表锁：直接锁定整张表，在你锁定期间，其它进程无法对该表进行写操作。如果你是写锁，则其它进程则读也不允许
行锁：仅对指定的行进行加锁，这样其它进程还是可以对同一个表中的其它行进行操作。

页锁，表锁速度快，但冲突多，行锁冲突少，但速度慢。所以取了折衷的页锁，一次锁定相邻的一组记录。

上述三种锁的特性可大致归纳如下：
1） 表级锁：开销小，加锁快；不会出现死锁；锁定粒度大，发生锁冲突的概率最高，并发度最低。
2） 行级锁：开销大，加锁慢；会出现死锁；锁定粒度最小，发生锁冲突的概率最低，并发度也最高。
3） 页面锁：开销和加锁时间界于表锁和行锁之间；会出现死锁；锁定粒度界于表锁和行锁之间，并发度一般。

## mvcc

> MVCC，全称Multi-Version Concurrency Control，即多版本并发控制。MVCC是一种并发控制的方法，一般在数据库管理系统中，实现对数据库的并发访问，在编程语言中实现事务内存。

MVCC在MySQL InnoDB中的实现主要是为了提高数据库并发性能，用更好的方式去处理读-写冲突，**做到即使有读写冲突时，也能做到不加锁，非阻塞并发读**。

在学习MVCC多版本并发控制之前，我们必须先了解一下，什么是MySQL InnoDB下的当前读和快照读?

- 当前读
   像select lock in share mode(共享锁), select for update ; update, insert ,delete(排他锁)这些操作都是一种当前读，为什么叫当前读？就是它读取的是记录的最新版本，读取时还要保证其他并发事务不能修改当前记录，会对读取的记录进行加锁。
- 快照读
   像不加锁的select操作就是快照读，即不加锁的非阻塞读；快照读的前提是隔离级别不是串行级别，串行级别下的快照读会退化成当前读；之所以出现快照读的情况，是基于提高并发性能的考虑，**快照读的实现是基于多版本并发控制，即MVCC,可以认为MVCC是行锁的一个变种**，但它在很多情况下，避免了加锁操作，降低了开销；**既然是基于多版本，即快照读可能读到的并不一定是数据的最新版本，而有可能是之前的历史版本**

说白了**MVCC就是为了实现读-写冲突不加锁**，而这个读指的就是快照读, 而非当前读，当前读实际上是一种加锁的操作，是悲观锁的实现

#### 当前读，快照读和MVCC的关系

- 准确的说，MVCC多版本并发控制指的是 “维持一个数据的多个版本，使得读写操作没有冲突” 这么一个概念。仅仅是一个理想概念
- 而在MySQL中，实现这么一个MVCC理想概念，我们就需要MySQL提供具体的功能去实现它，而<u>快照读就是MySQL为我们实现MVCC理想模型的其中一个具体非阻塞读功能</u>。而相对而言，当前读就是悲观锁的具体功能实现
- 要说的再细致一些，快照读本身也是一个抽象概念

InnoDB的MVCC，是通过在每行记录后面保存两个隐藏的列来实现的。这两个列，一个保存了行创建的时间（创建版本号），一个保存了行过期时间（或删除时间，删除版本号）。当然存储的不是实际的时间，而是系统版本号。每开始一个新的事务，系统版本号就会自动递增。事务开始时刻的系统版本号会作为事务的版本号，用来和查询到的每行记录的版本号进行比较。在可重复读隔离级别下,MVCC的具体操作：

- **select**

  InnoDB会根据以下两个条件检查每行记录：

  - InnoDB只会查找创建版本号早于当前事务版本的数据行（也就是，行的版本号小于或等于事务的版本号），这样可以确保事务读取的行，要么是在事务开始之前就存在了，要么是事务自身插入或修改过的
  - 行的删除版本号要么未定义，要么大于当前事务版本号。这可以确保事务读取到的行，在事务开始之前未被删除。

  只有同时满足这两个条件的记录，才会作为查询结果返回

- **insert**

  InnoDB为新插入的每一行保存当前系统版本号作为行创建版本号

- **delete**

  InnoDB为删除的每一行保存当前系统版本号作为行删除版本号

- **update**

  InnoDB为更新一行数据，会将当前系统版本号作为更新前的数据的删除版本号，会将当前系统版本号作为更新后的数据的创建版本号

通过这两个额外的系统版本号，使得大多数操作都可以不用加锁。这使得数据库操作更加的简单，不足之处就在于需要额外的存储空间，需要做更多的行检查工作，以及额外的维护工作。

MVCC只在读提交和可重复读两个隔离级别下工作。

#### MVCC能解决什么问题，好处是？

###### 数据库并发场景有三种，分别为：

- 读-读：不存在任何问题，也不需要并发控制
- 读-写：有线程安全问题，可能会造成事务隔离性问题，可能遇到脏读，幻读，不可重复读
- 写-写：有线程安全问题，可能会存在更新丢失问题，比如第一类更新丢失，第二类更新丢失

###### MVCC带来的好处是？

多版本并发控制（MVCC）是一种用来解决读-写冲突的无锁并发控制，也就是为事务分配单向增长的时间戳，为每个修改保存一个版本，版本与事务时间戳关联，读操作只读该事务开始前的数据库的快照。 所以MVCC可以为数据库解决以下问题

- 在并发读写数据库时，可以做到在读操作时不用阻塞写操作，写操作也不用阻塞读操作，提高了数据库并发读写的性能
- 同时还可以解决脏读，幻读，不可重复读等事务隔离问题，但不能解决更新丢失问题

###### 小结一下

总之，MVCC就是因为大牛们，不满意只让数据库采用悲观锁这样性能不佳的形式去解决读-写冲突问题，而提出的解决方案，所以在数据库中，因为有了MVCC，所以我们可以形成两个组合：

- MVCC + 悲观锁
   MVCC解决读写冲突，悲观锁解决写写冲突
- MVCC + 乐观锁
   MVCC解决读写冲突，乐观锁解决写写冲突
   这种组合的方式就可以最大程度的提高数据库并发性能，并解决读写冲突，和写写冲突导致的问题





## 主键

唯一标识表中每行的这个列称为主键。主键用来标识特定的一行。没有主键，更新和删除表中特定行很困难。

## 外键

表的外键是另一个表的主键，用于确定与另一张表的关系。

## 索引

[参考](https://blog.csdn.net/wangfeijiu/article/details/113409719)

索引（在MySQL中称为键（key））是存储引擎快速找到记录的一种数据结构。

> 简单类比一下，数据库如同书籍，**索引如同书籍目录**，假如我们需要从书籍查找与 xx 相关的内容，我们可以直接从目录中查找，定位到 xx 内容所在页面，如果目录中没有 xx 相关字符或者没有设置目录（索引），那只能逐字逐页阅读文本查找，效率可想而知。

 在MySQL中，例如要运行以下语句:

```mysql
select first_name from actor where actor_id=5;
```

如果在`actor_id`列上建有索引，则MySQL将使用索引找到`actor_id`为5的行。也就是说，MySQL先在索引上按值进行查找，然后返回所有包含该值的数据行。

索引可以包含一个或多个列的值。如果包含多个列，那么列的顺序也十分重要，因为MySQL只能高效的使用索引的**最左前缀列**。



优点：加快数据的检索速度

- 索引大大减少了服务器需要扫描的数据量
- 索引可以帮助服务器避免排序和临时表
- 索引可以将随机IO变为顺序IO

缺点：创建和维护索引要耗费时间；索引需要占用物理空间

`索引是建立在数据库表中的某些列的上面。因此，在创建索引的时候，应该仔细考虑在哪些列上可以创建索引，在哪些列上不能创建索引。`

> - 在经常需要搜索的列上，可以加快搜索的速度
> - 在作为主键的列上，强制该列的唯一性和组织表中数据的排列结构
> - 在经常用在连接（JOIN）的列上，这些列主要是一外键，可以加快连接的速度



### 索引的数据结构

#### B+树

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210412115449.png)

> - 相同数据量，B+树比B树更加矮壮，且只在叶子节点存放数据，所以相比于B树，查询磁盘的次数更少
> - 每一次查询更会遍历到叶子节点，查询更加稳定
> - 因为B+树的叶子节点通过指针相连，**关键字（key）是有序**的，所以相比于哈希表，更加便于范围查询。(按照顺序存储数据)

##### 例子

```mysql
#创建表
create table person (
	last_name varchar(50) not_null,
    first_name varchar(50) not_null,
    dob data not_null,
    gender enum('m','f') not_null,
    key(last_name,first_name,dob)#创建索引
);
```

对于表中的每一行数据，索引中包含了`last_name`,`first_name`和`dob`列的值，下图显示了该索引是如何组织数据的存储的。

<img src="https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210516160221.png" style="zoom:80%;" />

索引中包含多个列时，对多个值进行排序的依据是根据`create table`中定义索引时列的顺序`key(last_name,first_name,dob)#创建索引`。

**可以使用B+树索引的查询类型**：

- 全值匹配

  ​	全值匹配值指和索引中的所有列进行匹配。例如可以查找姓名为`Alien Cuba`、出生于`1960-01-01`的人

- 匹配<u>最左前缀</u>

  ​	用于查找与索引中第一列的值匹配的数据；例如用于查找所有`last_name`等于`Alien`的人

- 匹配列前缀

  ​	用于只匹配某一列的值开头部分。例如可以查询所有以`J`开头的姓的人。

- 匹配范围值

  ​	例如可以查找姓在`Alien`和`Barrymore`之间的人

- 精确匹配某一列并范围匹配另外一列

  ​	可以用于查找所有姓名为`Barrymore`且名字开头为`V`的人。即对`last_name`全匹配，对`first_name`进行范围匹配

- 只访问索引的查询

  ​	即查询只需要访问索引但是无需访问数据行。

**关于B+树索引的一些限制**：

- **如果不是按照索引的最左列开始查询，则无法使用索引**。例如上例中索引无法用于查找名字为`Bill`的人，也无法查找某个特定生日的人，因为这两列都不是最左数据列。类似的，也无法查找姓名以某个字母结尾的人。

- 不能跳过索引中的列。也就是说，上例中的索引无法用于查找`last_name`为`Smith`并且在某个特定日期出生的人。如果不指定`first_name`，则MySQL只能使用索引的第一列。

- 如果查询中有某个列的范围查询，则其右边所有列都无法使用索引优化查询。例如有查询

  ```mysql
  ... where last_name = 'Smith' and first_name like 'J' and dob='1970-01-01';
  ```

  这个查询只能够使用索引的前两列，因为这里like时是一个范围查询。

综上：**索引列的顺序是非常重要的。**









#### 哈希表

哈希索引就是采用一定的哈希算法，把键值换算成新的哈希值，检索时不需要类似B+树那样从根节点到叶子节点逐级查找，只需一次哈希算法即可立刻定位到相应的位置，速度非常快。

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210412101755.png)

Hash索引仅仅能满足"=",“IN"和”<=>"查询，不能使用范围查询。也不支持任何范围查询，例如WHERE price > 100。
　　
由于Hash索引比较的是进行Hash运算之后的Hash值，所以它**只能用于等值的过滤，不能用于基于范围的过滤**，因为经过相应的Hash算法处理之后的Hash值的大小关系，并不能保证和Hash运算前完全一样。

### 索引分类

#### 聚簇索引

[参考](https://www.cnblogs.com/jiawen010/p/11805241.html)

聚簇索引并不是一种单独的索引类型，而是一种**数据存储方式**。

> InnoDB中聚簇索引实际上将数据行与索引放到了一块，找到索引也就找到了数据。
>
> InnoDB中B+树索引可以分为聚簇索引和非聚簇索引

聚簇索引中叶子节点包含了所有行数据，非叶子结点只存放索引列。因为无法将数据行存放在两个不同的地方，所以一个表只能有一个聚簇索引。

InnoDB中通过主键聚集数据，**主键索引**就是聚簇索引。主键索引就是按照每张表的主键来构造B+树，叶子节点存放的是整张表的行数据，也将叶子节点称为<u>数据页</u>。如果没有定义主键InnoDB会选择非空的唯一索引替代。如果没有这样的索引，InnoDB会隐式的定义一个主键来作为聚簇索引。

优点：

1.数据访问更快，因为聚簇索引将索引和数据保存在同一个B+树中，因此从聚簇索引中获取数据比非聚簇索引更快

2.聚簇索引对于主键的排序查找和范围查找速度非常快

缺点：

1.插入速度严重依赖于插入顺序，按照主键的**顺序插入**是最快的方式，否则将会出现页分裂，严重影响性能。因此，对于InnoDB表，我们一般都会定义一个**自增的ID列为主键**
2.**更新主键的代价很高**，因为将会导致被更新的行移动。因此，对于InnoDB表，我们一般定义主键为不可更新。
3.二级索引访问需要两次索引查找，第一次找到主键值，第二次根据主键值找到行数据。

#### 非聚簇索引

在**聚簇索引之上创建的索引称之为辅助索引**，辅助索引访问数据总是需要二次查找。辅助索引叶子节点存储的不再是行的物理位置，而是主键值。通过辅助索引首先找到的是主键值，再通过主键值找到数据行的数据页，再通过数据页中的Page Directory找到数据行。

辅助索引的存在不影响数据在聚簇索引中的组织，所以一张表可以有多个辅助索引。在innodb中有时也称辅助索引为二级索引





优点：

- 数据访问更快，因为聚簇索引将索引和数据保存在同一个B+树中，因此从聚簇索引中获取数据比非聚簇索引更快
- 聚簇索引对于主键的排序查找和范围查找速度非常快

# 存储引擎

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210411103021.png)

## InnoDB引擎与MyIASM引擎的区别 

|                                | InnoDB                                 | MyIASM         |
| ------------------------------ | -------------------------------------- | -------------- |
| 是否支持行级锁                 | 支持表级锁和行级锁，默认为行级锁       | 只有表级锁     |
| 是否支持事务和崩溃后的安全恢复 | 支持事务；具有事务、回滚和崩溃修复功能 | 不提供事务支持 |
| 是否支持外键                   | 支持                                   | 不支持         |
| 是否支持MVCC                   | 支持                                   | 不支持         |



---

# 实操题

### 1. 编写一个 SQL 查询，来删除 `Person` 表中所有重复的电子邮箱，重复的邮箱里只保留 **Id** *最小* 的那个。

[链接](https://leetcode-cn.com/problems/delete-duplicate-emails/)

| id   | email            |
| ---- | ---------------- |
| 1    | john@example.com |
| 2    | bob@example.com  |
| 3    | john@example.com |

Id 是这个表的主键。

例如，在运行你的查询语句之后，上面的 Person 表应返回以下几行:

| id   | email            |
| ---- | ---------------- |
| 1    | john@example.com |
| 2    | bob@example.com  |

我的解法1：

~~~mysql
 delete from Person where id=
 (select id from 
  (
     select max(id) as id from Person where email in
     (
         select email from Person group by email having count(email)>1
     )
  ) as a
 );
~~~

我的思路是这样的：

- ~~~mysql
   select email from Person group by email having count(email)>1)
  ~~~

  首先按照email进行分组，挑选出重复的email，即通过count(email)>1判断->**这一方法适用于挑选出表中重复的元素**

- 然后在重复的eamil中找到id最大的那一行，进行删除即可

- 注意：**MySQL每一个派生出来的表都必须有一个自己的别名**



但是该方法碰到有三个或三个以上的重复email时就会失效。因为在删除的时候值删除了重复email中最大id的那一行，而中间大的没有被删除。

那我们换一个思路：能不能之间将重复email中最小id的那一行挑选出来，进行保留，其余的删除即可。这就是解法2：



~~~mysql
delete from Person where id not in 
(select id from
 		(
            select min(id) as id  ,email from Person group by email
        ) as t
);
~~~

思路：

- ~~~mysql
  select min(id) as id  ,email from Person group by email
  ~~~

  按照email进行分组，如果email是重复的，则只保留id最小的那一行，如果email是不重复的，即id最小的那一行就是它本身。

- 如果id不在派生出来的表中，则将对应行进行删除。

> 注意
>
> ```mysql
> select id , email from person group by email;
> ```
>
> 返回的是两个group:
>
> group1:
>
> ```
> +----+--------+
> | Id | email |
> +----+--------+
> | 1 ,3 |   john@example.com  |     
> +----+--------+
> ```
>
> 
>
> group2:
>
> | 2  bob@example.com |
> | ------------------ |
> |                    |
>
> 

### 2. 编写一个 SQL 查询，查找 `Person` 表中所有重复的电子邮箱。

[链接](https://leetcode-cn.com/problems/duplicate-emails/)

~~~mysql
select Email from Person group by Email having count(id)>1;
~~~



### 3.编写一个 SQL 查询，获取 `Employee` 表中第二高的薪水（Salary） 。

[链接](https://leetcode-cn.com/problems/second-highest-salary/)

```
+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
```

例如上述 Employee 表，SQL查询应该返回 200 作为第二高的薪水。如果不存在第二高的薪水，那么查询应返回 null。

```
+---------------------+
| SecondHighestSalary |
+---------------------+
| 200                 |
+---------------------+
```





解法1

利用自查询的特性：

子查询可以在任意地方使用。相当于查询的列。在oracle中只写查询列没有查询表需要加上 from dual（伪表）。而mysql不用写

~~~mysql
select (select distinct salary from employee order by salary desc limit 1,1) as SecondHighestSalary;
~~~



解法2：

~~~mysql
select ifnull((select distinct salary from employee order by salary desc limit 1,1),null) as second;
~~~





IFNULL(expression, alt_value)
**如果第一个参数的表达式 expression 为 NULL，则返回第二个参数的备用值(此题中是返回null)**。
expression是table的时候要加括号

distinct：
去重一样的Salary

limit：限时返回的个数
offset：跳过几个
limit 1 offset 1:返回一个结果，跳过一个
例如返回第三高就是：limit 1 offset 2



### 某网站包含两个表，`Customers` 表和 `Orders` 表。编写一个 SQL 查询，找出所有从不订购任何东西的客户。

[链接](https://leetcode-cn.com/problems/customers-who-never-order/)

`Customers` 表：

~~~
+----+-------+
| Id | Name  |
+----+-------+
| 1  | Joe   |
| 2  | Henry |
| 3  | Sam   |
| 4  | Max   |
+----+-------+

~~~

`Orders` 表：	

~~~mysql
+----+------------+
| Id | CustomerId |
+----+------------+
| 1  | 3          |
| 2  | 1          |
+----+------------+
~~~

例如给定上述表格，你的查询应返回：

```
+-----------+
| Customers |
+-----------+
| Henry     |
| Max       |
+-----------+
```



~~~mysql
select Name as Customers from Customers where Id not in (select CustomerId as Id from Orders);
~~~

### 多表连接

```mysql
select 字段列表 from 表1 inner | left |right join 表2 on 条件
```

1）内联结（inner join），取两表的公共数据

2）左联结（left join），联结结果保留左表的全部数据 -> 在inner join的基础上，增加左边表有右边没有的内容 -> 

3）右联结（right join），联结结果保留右表的全部数据





![1.jpg](https://pic.leetcode-cn.com/ad3df1c4ecc7d2dbe85f92cdde8ec9a731fdd20dc4c5629ecb372b21de26c682-1.jpg)

表1: Person

```
+-------------+---------+
| 列名         | 类型     |
+-------------+---------+
| PersonId    | int     |
| FirstName   | varchar |
| LastName    | varchar |
+-------------+---------+
```

PersonId 是上表主键

表2: Address

```
+-------------+---------+
| 列名         | 类型    |
+-------------+---------+
| AddressId   | int     |
| PersonId    | int     |
| City        | varchar |
| State       | varchar |
+-------------+---------+
```

AddressId 是上表主键


编写一个 SQL 查询，满足条件：无论 person 是否有地址信息，都需要基于上述两表提供 person 的以下信息：

 

FirstName, LastName, City, State


[链接](https://leetcode-cn.com/problems/combine-two-tables/)

~~~mysql
select FirstName, LastName, City, State  from Person left join Address on Person.PersonId=Address.PersonId;
~~~





### 收入超过经理的员工

[链接](https://leetcode-cn.com/problems/employees-earning-more-than-their-managers/)

Employee 表包含所有员工，他们的经理也属于员工。每个员工都有一个 Id，此外还有一列对应员工的经理的 Id。

~~~
+----+-------+--------+-----------+
| Id | Name  | Salary | ManagerId |
+----+-------+--------+-----------+
| 1  | Joe   | 70000  | 3         |
| 2  | Henry | 80000  | 4         |
| 3  | Sam   | 60000  | NULL      |
| 4  | Max   | 90000  | NULL      |
+----+-------+--------+-----------+
~~~



给定 Employee 表，编写一个 SQL 查询，该查询可以获取收入超过他们经理的员工的姓名。在上面的表格中，Joe 是唯一一个收入超过他的经理的员工。

~~~
+----------+
| Employee |
+----------+
| Joe      |
+----------+
~~~

解法1：

~~~mysql
select name as Employee from 
(
    select * from Employee where managerid is not null #挑选出managerid不为null的行,表示员工
) as t
    where salary>  #进行比较
(
    select salary from Employee where id=t.managerid
);
~~~

解法2：

~~~mysql
SELECT
    a.Name AS Employee
FROM
    Employee AS a,
    Employee AS b
WHERE
    a.ManagerId = b.Id
        AND a.Salary > b.Salary
;
~~~





### 编写一个 SQL 查询，来查找与之前（昨天的）日期相比温度更高的所有日期的 `id` 。

返回结果 **不要求顺序** 。

我的解法：

~~~mysql
select id from Weather as w1 
where 
Temperature >(select Temperature from Weather w2 where recordDate=(w1.recordDate-1) ) #挑选出前一天日期的温度
;
~~~

但是运行的时候会出错，原因在：比较两个日期的时候不能做差，而要使用datediff函数。datediff函数返回两个日期的时间差

例如：

~~~mysql
#DATEDIFF(date1,date2)
SELECT DATEDIFF('2008-12-30','2008-12-29')
#返回1,data1-data2
~~~



改正后：

~~~mysql
select id from Weather as w1 
where 
Temperature >(select Temperature from Weather w2 where DATEDIFF(w1.recordDate,w2.recordDate)=1 ) #挑选出前一天日期的温度
;
~~~

或

~~~mysql
SELECT w2.Id
FROM Weather w1, Weather w2
WHERE DATEDIFF(w2.RecordDate, w1.RecordDate) = 1
AND w1.Temperature < w2.Temperature
~~~



### [重新格式化部门表](https://leetcode-cn.com/problems/reformat-department-table/)

原表为

~~~
+------+---------+-------+
| id   | revenue | month |
+------+---------+-------+
| 1    | 8000    | Jan   |
| 2    | 9000    | Jan   |
| 3    | 10000   | Feb   |
| 1    | 7000    | Feb   |
| 1    | 6000    | Mar   |
+------+---------+-------+
~~~

通过语句

~~~mysql
SELECT id,
IF(`month`='Jan',revenue,NULL) Jan_Revenue, #对于原表中的第一行，其month=jan,所以该位置返回8000
IF(`month`='Feb',revenue,NULL) Feb_Revenue, #因为month!=feb,返回null
IF(`month`='Mar',revenue,NULL) Mar_Revenue,
IF(`month`='Apr',revenue,NULL) Apr_Revenue,
IF(`month`='May',revenue,NULL) May_Revenue,
IF(`month`='Jun',revenue,NULL) Jun_Revenue,
IF(`month`='Jul',revenue,NULL) Jul_Revenue,
IF(`month`='Aug',revenue,NULL) Aug_Revenue,
IF(`month`='Sep',revenue,NULL) Sep_Revenue,
IF(`month`='Oct',revenue,NULL) Oct_Revenue,
IF(`month`='Nov',revenue,NULL) Nov_Revenue,
IF(`month`='Dec',revenue,NULL) Dec_Revenue
FROM Department;
~~~

或

~~~mysql
SELECT id,
CASE `month` WHEN 'Jan' THEN revenue END Jan_Revenue,
CASE `month` WHEN 'Feb' THEN revenue END Feb_Revenue,
CASE `month` WHEN 'Mar' THEN revenue END Mar_Revenue,
CASE `month` WHEN 'Apr' THEN revenue END Apr_Revenue,
CASE `month` WHEN 'May' THEN revenue END May_Revenue,
CASE `month` WHEN 'Jun' THEN revenue END Jun_Revenue,
CASE `month` WHEN 'Jul' THEN revenue END Jul_Revenue,
CASE `month` WHEN 'Aug' THEN revenue END Aug_Revenue,
CASE `month` WHEN 'Sep' THEN revenue END Sep_Revenue,
CASE `month` WHEN 'Oct' THEN revenue END Oct_Revenue,
CASE `month` WHEN 'Nov' THEN revenue END Nov_Revenue,
CASE `month` WHEN 'Dec' THEN revenue END Dec_Revenue
FROM Department;

~~~



得到虚拟表1,如下所示

~~~
+------+-------------+-------------+-------------+-----+-------------+
| id   | Jan_Revenue | Feb_Revenue | Mar_Revenue | ... | Dec_Revenue |
+------+-------------+-------------+-------------+-----+-------------+

| 1    | 8000        | null        | null        | ... | null        |
+------+-------------+-------------+-------------+-----+-------------+
| 2    | 9000        | null        | null        | ... | null        |
+------+-------------+-------------+-------------+-----+-------------+
| 3    | null        | 10000       | null        | ... | null        |
+------+-------------+-------------+-------------+-----+-------------+
| 1    | null        | null        | 6000        | ... | null        |
+------+-------------+-------------+-------------+-----+-------------+
| 1    | null        | 7000        | null        | ... | null        |
+------+-------------+-------------+-------------+-----+-------------+

~~~

通过group by id语句可以得到得到一个虚拟表2，如下所示。

~~~
+------+-------------+-------------+-------------+-----+-------------+
| id   | Jan_Revenue | Feb_Revenue | Mar_Revenue | ... | Dec_Revenue |
+------+-------------+-------------+-------------+-----+-------------+
|      | 8000        | null        | null        | ... | null        |
|  1   | null        | null        | 6000        | ... | null        |
|      | null        | 7000        | null        | ... | null        |
-------+-------------+-------------+-------------------+--------------
| 2    | 9000        | null        | null        | ... | null        |
+------+-------------+-------------+-------------+-----+-------------+
| 3    | null        | 10000       | null        | ... | null        |
+------+-------------+-------------+-------------+-----+-------------+
~~~

该虚拟表按id分组，其中对于id=1的组中，Jan_Revenue，Feb_Revenue，...，Dec_Revenue等列中有多个数值，因此需要一个聚合函数将多个数值进行整合，输出为一个数值。例如sum或max函数。



因此总的语句为：

~~~mysql
SELECT id,
SUM(CASE `month` WHEN 'Jan' THEN revenue END) Jan_Revenue,
SUM(CASE `month` WHEN 'Feb' THEN revenue END) Feb_Revenue,
SUM(CASE `month` WHEN 'Mar' THEN revenue END) Mar_Revenue,
SUM(CASE `month` WHEN 'Apr' THEN revenue END) Apr_Revenue,
SUM(CASE `month` WHEN 'May' THEN revenue END) May_Revenue,
SUM(CASE `month` WHEN 'Jun' THEN revenue END) Jun_Revenue,
SUM(CASE `month` WHEN 'Jul' THEN revenue END) Jul_Revenue,
SUM(CASE `month` WHEN 'Aug' THEN revenue END) Aug_Revenue,
SUM(CASE `month` WHEN 'Sep' THEN revenue END) Sep_Revenue,
SUM(CASE `month` WHEN 'Oct' THEN revenue END) Oct_Revenue,
SUM(CASE `month` WHEN 'Nov' THEN revenue END) Nov_Revenue,
SUM(CASE `month` WHEN 'Dec' THEN revenue END) Dec_Revenue
FROM Department
GROUP BY id;

~~~

最终的结果：

~~~
+------+-------------+-------------+-------------+-----+-------------+
| id   | Jan_Revenue | Feb_Revenue | Mar_Revenue | ... | Dec_Revenue |
+------+-------------+-------------+-------------+-----+-------------+
|  1   | 8000        | 7000        | 6000        | ... | null        |
-------+-------------+-------------+-------------------+--------------
|  2   | 9000        | null        | null        | ... | null        |
+------+-------------+-------------+-------------+-----+-------------+
|  3   | null        | 10000       | null        | ... | null        |
+------+-------------+-------------+-------------+-----+-------------+
~~~





### if语句

 在mysql中if()函数的用法类似于java中的三目表达式，其用处也比较多，具体语法如下：

IF(expr1,expr2,expr3)，如果expr1的值为true，则返回expr2的值，如果expr1的值为false，

则返回expr3的值。

### group by语句

[参考](https://blog.csdn.net/u014717572/article/details/80687042)

group_by语句实际上把**同一组内多行纪录合并成一行**

### 对表进行修改的方法

drop: 删除整个表 `delete table 表名`  -->清空表的数据以及表的结构

truncate: 清空表数据 `truncate table 表名`  -->清空表的数据但是不清空表的结构

alter: 对表的一列进行操作 `alter table 表名 add 列名 数据类型 ` -->给表添加一列 ； `alter table 表名 drop 列名`-->将表的一列删除

🧅以上是对表的结构进行了修改;



update： 将表中某一行数据进行更新 `update 表名 set 列名=值 where 指定的行`

delete：删除表中的某一行 `delete from 表名 where 指定某一行`

insert: 插入一行 `insert into 表名（列名，列名...） values (值，值...)`

🍕以上只是对表的数据进行了修改 
