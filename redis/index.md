# Redis


#### NoSQL数据库的四大分类

- KV键值对
- 文档类数据库
- 列存储数据库
- 图关系数据库

#### Redis简要介绍

Redis : **RE**mote **DI**ctionary **S**erver（远程字典服务器），是一个高性能的（key/value）分布式内存数据库，基于内存运行，是当下最热门的NoSQL数据库之一，也被人们称为数据结构服务器。

#### Redis的三大特点

- Redis支持数据的持久化，可以将内存中的数据保存在磁盘中，重启的时候可以再次加载进行使用
- Redis不仅仅支持简单的key-value类型的数据，同时好提供list,set,zset,hash等数据结构的存储
- Redis支持数据的备份，即master-slave模式的数据备份

#### Redis的使用场景

- 内存存储和持久化；Redis支持异步将内存中的数据存放写到磁盘上，同时不影响继续服务

- 取最新N个数据的操作，如：可以将最新10条评论的ID放在Redis的list集合模拟类似HttpSession这种需要设定过期时间的功能

- 发布、订阅消息系统

- 定时器、计数器

  可以对String进行自增自减运算，从而实现计数器功能。

  redis这种内存型数据库的读写性能非常高，很适合存储频繁读写的计数量

- 缓存

  将热点数据放入内存中，设置内存的最大使用量以及淘汰策略来保证缓存的命中率

- 会话缓存

  可以使用redis来统一存储多台应用服务器的回话信息

  当应用服务器不再存储用户的会话信息，也就不在具有状态，一个用户可以请求任意一个应用服务器，从而实现高可用性以及可伸缩性。

#### redis与memcached

两者都是非关系型内存键值数据库，主要有以下不同

**数据类型**

memcached仅支持字符串类型，而redis支持五种不同的数据类型，可以更灵活地解决问题

**数据持久化**

redis支持两种持久化策略：RDB快照和AOF日志，而memcached不支持持久化

**分布式**

memcached不支持分布式，只能通过再客户端使用一致性哈希来实现分布式存储

redis实现了分布式的支持

**内存管理机制**

- 在redis中，并不是所有数据都一直存储在内存中，可以将一些很久没用的value交换到磁盘，而memcached的数据则一直在内存中。
- memmcached将内存分割为特定长度的块来存储数据，以完全解决内存碎片的问题，但是这种方式会使得内存的利用率不高，例如块的大小为128bytes，只存储100bytes的数据，那么剩下的28bytes就浪费掉了



#### Redis相关知识

- 单进程
- 默认16个数据库，类似数组下标从零开始，初始默认使用0号
- Select命令切换数据库
- Dbsize查看当前数据库的key数量，`keys *`: 查看所有的key，`keys k?`：可以支持条件选择
- flushall:清空所有的数据库（慎用）
- flushdb：清空当前库
- 同意密码管理，16个库都是同样的密码
- redis索引都是从零开始
- 默认端口是6379

#### Redis的数据类型

Redis键（key）

- keys *
- exists key的名字，判断某个key是否存在
- move key db 当前的库就没有了，被移除了
- expire key 几秒钟：为给定的key设置过期时间
- ttl key 查看还有多少秒过期，-1表示永不过期，-2表示已经过期
- del key：删除key
- type  key 查看你的key是什么类型

1.**Redis字符串（String）**

string 是redis最基本的数据类型，一个key对应一个value

string类型是二进制安全的。这意味着redis的string可以包含任何数据。比如jpg图片或者序列化的对象

string类型是redis最基本的数据类型，一个redis中字符串value最多可以是512M

常用命令：

set/get/append/strlen

incr/decr/incrby/decrby,一定要是数字才能够进行加减

getrange/setrange

setex(set with expire)键秒值/setnx(set if not exist)

mset/mget/msetnx

getset(先get后set)

 2.**Redis列表（List）**

redis列表是简单的字符串列表，按照插入顺序排序。可以在头部或者尾部插入元素。底层是一个双向链表。（元素可以重复）

3.**Redis集合（Set）**

redis的set是string类型的无序集合（元素不可重复）。它是通过HashTable实现的

4.**Redis哈希（Hash)**

redis hash是一个键值对集合

redis hash是一个string类型的filed和value的映射表，hash特别适合用于存储对象

5.**Redis有序集合（Zset, sorted set)**

zset和set一样也是string类型元素的集合，且不允许重复的成员

不同是每一个元素都会关联一个double类型的分数

redis正式通过分数来为集合中的成员进行**从小到大**的排序。**zset的成员是唯一的，但是分数（score）确实可以重复的。**



#### 解析配置文件

解析配置文件redis.conf或者是redis.windows.conf

```markdown
常见redis.conf配置说明：

#是否在后台运行；no：不是后台运行
daemonize yes
 
#是否开启保护模式，默认开启。要是配置里没有指定bind和密码。开启该参数后，redis只会本地进行访问，拒绝外部访问。
protected-mode yes
 
#redis的进程文件
pidfile /var/run/redis/redis-server.pid
 
#redis监听的端口号。
port 6379
 
#此参数确定了TCP连接中已完成队列(完成三次握手之后)的长度， 当然此值必须不大于Linux系统定义的/proc/sys/net/core/somaxconn值，默认是511，而Linux的默认参数值是128。当系统并发量大并且客户端速度缓慢的时候，可以将这二个参数一起参考设定。该内核参数默认值一般是128，对于负载很大的服务程序来说大大的不够。一般会将它修改为2048或者更大。在/etc/sysctl.conf中添加:net.core.somaxconn = 2048，然后在终端中执行sysctl -p。
tcp-backlog 511
 
#指定 redis 只接收来自于该 IP 地址的请求，如果不进行设置，那么将处理所有请求
#bind 127.0.0.1
#bind 0.0.0.0
 
#配置unix socket来让redis支持监听本地连接。
# unixsocket /var/run/redis/redis.sock
 
#配置unix socket使用文件的权限
# unixsocketperm 700
 
# 此参数为设置客户端空闲超过timeout，服务端会断开连接，为0则服务端不会主动断开连接，不能小于0。
timeout 0
 
#tcp keepalive参数。如果设置不为0，就使用配置tcp的SO_KEEPALIVE值，使用keepalive有两个好处:检测挂掉的对端。降低中间设备出问题而导致网络看似连接却已经与对端端口的问题。在Linux内核中，设置了keepalive，redis会定时给对端发送ack。检测到对端关闭需要两倍的设置值。
tcp-keepalive 0
 
#指定了服务端日志的级别。级别包括：debug（很多信息，方便开发、测试），verbose（许多有用的信息，但是没有debug级别信息多），notice（适当的日志级别，适合生产环境），warn（只有非常重要的信息）
loglevel notice
 
#指定了记录日志的文件。空字符串的话，日志会打印到标准输出设备。后台运行的redis标准输出是/dev/null。
logfile /var/log/redis/redis-server.log
 
#是否打开记录syslog功能
# syslog-enabled no
 
#syslog的标识符。
# syslog-ident redis
 
#日志的来源、设备
# syslog-facility local0
 
#数据库的数量，默认使用的数据库是DB 0。可以通过SELECT命令选择一个db
databases 16
 
# redis是基于内存的数据库，可以通过设置该值定期写入磁盘。
# 注释掉“save”这一行配置项就可以让保存数据库功能失效
# 900秒（15分钟）内至少1个key值改变（则进行数据库保存--持久化） 
# 300秒（5分钟）内至少10个key值改变（则进行数据库保存--持久化） 
# 60秒（1分钟）内至少10000个key值改变（则进行数据库保存--持久化）
save 900 1
save 300 10
save 60 10000
 
#当RDB持久化出现错误后，是否依然进行继续进行工作，yes：不能进行工作，no：可以继续进行工作，可以通过info中的rdb_last_bgsave_status了解RDB持久化是否有错误
stop-writes-on-bgsave-error yes
 
#使用压缩rdb文件，rdb文件压缩使用LZF压缩算法，yes：压缩，但是需要一些cpu的消耗。no：不压缩，需要更多的磁盘空间
rdbcompression yes
 
#是否校验rdb文件。从rdb格式的第五个版本开始，在rdb文件的末尾会带上CRC64的校验和。这跟有利于文件的容错性，但是在保存rdb文件的时候，会有大概10%的性能损耗，所以如果你追求高性能，可以关闭该配置。
rdbchecksum yes
 
#rdb文件的名称
dbfilename dump.rdb
 
#数据目录，数据库的写入会在这个目录。rdb、aof文件也会写在这个目录
dir /data
```

```markdown
缓存过期清除策略：

# MAXMEMORY POLICY: how Redis will select what to remove when maxmemory
# is reached. You can select among five behaviors:
#
# volatile-lru -> remove the key with an expire set using an LRU algorithm  -->使用lru算法移除key,只设置了过期时间的键
# allkeys-lru -> remove any key according to the LRU algorithm -->使用lru算法移除key
# volatile-random -> remove a random key with an expire set -->在过期集合中移除随机而的key，只设置了过期时间的键
# allkeys-random -> remove a random key, any key -->随机移除key
# volatile-ttl -> remove the key with the nearest expire time (minor TTL) -->移除那些ttl值最小的key，即那些最近要过期的key
# noeviction -> don't expire at all, just return an error on write operations -->不进行移除。针对写操作，只是返回错误信息
#

The default is:
#
# maxmemory-policy noeviction

lru：最近最少使用
random：随机
ttl：有限时间内
noeviction:永不过期，在实际生产中基本不使用这个
```

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210210203043.png)

```markdown
SNAPSHOTTING快照
################################ SNAPSHOTTING  ################################
#
# Save the DB on disk:
#
#   save <seconds> <changes>

save 秒钟 写操作次数

# redis是基于内存的数据库，可以通过设置该值定期写入磁盘。
# 注释掉“save”这一行配置项就可以让保存数据库功能失效
# 900秒（15分钟）内至少1个key值改变（则进行数据库保存--持久化） 
# 300秒（5分钟）内至少10个key值改变（则进行数据库保存--持久化） 
# 60秒（1分钟）内至少10000个key值改变（则进行数据库保存--持久化）
save 900 1  -->15分钟内改变了1次
save 300 10 -->5分钟改变了10次
save 60 10000 -->1分钟改变了1万次

如果想要禁用RDB持久化策略，只要不设置任何save指令，或者给save传入一个空字符串
127.0.0.1:6379> set k1 v1
OK
127.0.0.1:6379> save
OK


# If the background saving process will start working again Redis will
# automatically allow writes again.
#
# However if you have setup your proper monitoring of the Redis server
# and persistence, you may want to disable this feature so that Redis will
# continue to work as usual even if there are problems with disk,
# permissions, and so forth.
stop-writes-on-bgsave-error yes
如果配置成no，表示你不在乎数据不一一致或者有其他手段发现和控制


# Compress string objects using LZF when dump .rdb databases?
# For default that's set to 'yes' as it's almost always a win.
# If you want to save some CPU in the saving child set it to 'no' but
# the dataset will likely be bigger if you have compressible values or keys.
rdbcompression yes

是否将rdb文件进行压缩


# Since version 5 of RDB a CRC64 checksum is placed at the end of the file.
# This makes the format more resistant to corruption but there is a performance
# hit to pay (around 10%) when saving and loading RDB files, so you can disable it
# for maximum performances.
#
# RDB files created with checksum disabled have a checksum of zero that will
# tell the loading code to skip the check.
rdbchecksum yes

在存储快照后，还可以让redis使用CRC64算法进行数据校验，但是这样会增加大约10%的性能损耗

```





#### redis的持久化

持久化就是将内存中的数据自动备份到硬盘中

redis是内存型数据库，为了保证数据在断电后不会丢失，需要将内存中的数据持久化到硬盘上。

##### RDB：redis database

是什么： <u>在指定的时间间隔内将内存中的数据集写入磁盘</u>，-->（写） 也就是snapshot快照，它<u>恢复时是将快照文件直接读入内存中</u>  <--（读）。

> 将某个时间点的所有数据都存放在硬盘上。
>
> 可以将快照复制到其他服务器从而创建具有相同数据的服务器副本。
>
> 如果系统发生故障，将会丢失最后一次创建快照之后的数据。
>
> 如果数据量很大，保存快照的时间会很长

优点和缺点：redis会单独创建（fork）一个子进程来进行持久化，会将数据写入到一个临时文件中，待持久化过程都结束了，再用这个临时文件替换上次持久化好的文件。整个过程中，主进程是不进行任何IO操作的，这就确保了极高的性能。如果需要进行大规模数据的恢复，且对数据恢复的完整性不是非常敏感，那RDB方式要比AOF方式更加的高效。RDB的缺点是最后一次持久化后的数据可能丢失。

fork：复制一个与当前进程一样的进程。新进程的所有数据都和原进程一致，但是是一个全新的进程，并作为原进程的子进程



reb保存的是dump.rdb文件

配置位置：

```markdown
################################ SNAPSHOTTING  ################################
#
# Save the DB on disk:
#
#   save <seconds> <changes>
#
#   Will save the DB if both the given number of seconds and the given
#   number of write operations against the DB occurred.
#
#   In the example below the behaviour will be to save:
#   after 900 sec (15 min) if at least 1 key changed
#   after 300 sec (5 min) if at least 10 keys changed
#   after 60 sec if at least 10000 keys changed
#
#   Note: you can disable saving completely by commenting out all "save" lines.
#
#   It is also possible to remove all the previously configured save
#   points by adding a save directive with a single empty string argument
#   like in the following example:
#
#   save ""

# redis是基于内存的数据库，可以通过设置该值定期写入磁盘。-->触发快照的默认配置
# 注释掉“save”这一行配置项就可以让保存数据库功能失效
# 900秒（15分钟）内至少1个key值改变（则进行数据库保存--持久化） 
# 300秒（5分钟）内至少10个key值改变（则进行数据库保存--持久化） 
# 60秒（1分钟）内至少10000个key值改变（则进行数据库保存--持久化）

save 900 1
save 300 10
save 60 10000

# By default Redis will stop accepting writes if RDB snapshots are enabled
# (at least one save point) and the latest background save failed.
# This will make the user aware (in a hard way) that data is not persisting
# on disk properly, otherwise chances are that no one will notice and some
# disaster will happen.
#
# If the background saving process will start working again Redis will
# automatically allow writes again.
#
# However if you have setup your proper monitoring of the Redis server
# and persistence, you may want to disable this feature so that Redis will
# continue to work as usual even if there are problems with disk,
# permissions, and so forth.
stop-writes-on-bgsave-error yes

# Compress string objects using LZF when dump .rdb databases?
# For default that's set to 'yes' as it's almost always a win.
# If you want to save some CPU in the saving child set it to 'no' but
# the dataset will likely be bigger if you have compressible values or keys.
rdbcompression yes

# Since version 5 of RDB a CRC64 checksum is placed at the end of the file.
# This makes the format more resistant to corruption but there is a performance
# hit to pay (around 10%) when saving and loading RDB files, so you can disable it
# for maximum performances.
#
# RDB files created with checksum disabled have a checksum of zero that will
# tell the loading code to skip the check.
rdbchecksum yes

# The filename where to dump the DB
dbfilename dump.rdb

# The working directory.
#
# The DB will be written inside this directory, with the filename specified
# above using the 'dbfilename' configuration directive.
#
# The Append Only File will also be created inside this directory.
#
# Note that you must specify a directory here, not a file name.
dir ./
```

如何触发RDB快照

配置文件的默认配置（在某一个条件下触发），将生成的rdb文件备份到其他机器

save或bgsave，立刻生成rdb文件；save:全部阻塞，bgsave:在后台异步进行快照操作

执行flushall命令，但是产生的是空的rdb文件，无意义



如何恢复

将备份文件(dump.rdb)移动到啊redis安装目录并启动服务即可。（config get dir 获取redis安装目录）



优点：适合大规模的数据恢复，对数据完整性和一致性要求不高

缺点：在一定间隔时间做一次备份，所以如果redis意外关闭的话，就会丢失最后一次快照之后的所有修改；fork的时候，内存中的数据被克隆了一份，大致有2倍的膨胀性要求考虑



小总结

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210208204850.png)





##### AOF：append only file

是什么：**以日志的形式来记录每个写操作**，将redis执行过的所有写指令记录下来（读操作不记录），只追加文件但不可以改写文件，redis启动之初会读取该文件重新构建数据，换言之，redis重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作。

> 将写命令添加到AOF文件的末尾。
>
> 使用AOF持久化需要设置同步选项，从而确保写命令同步到磁盘文件上的时机。这是因为对文件进行写入并不会马上将内容同步到磁盘上，而是先存储到缓冲区，然后由操作系统决定什么时候同步到磁盘。有以下同步选项：
>
> ![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210210203908.png)

**简要的说就是每秒钟将写操作进行备份**

AOF保存的是appendonly.aof文件



配置位置

```markdown
############################## APPEND ONLY MODE ###############################

# By default Redis asynchronously dumps the dataset on disk. This mode is
# good enough in many applications, but an issue with the Redis process or
# a power outage may result into a few minutes of writes lost (depending on
# the configured save points).
#
# The Append Only File is an alternative persistence mode that provides
# much better durability. For instance using the default data fsync policy
# (see later in the config file) Redis can lose just one second of writes in a
# dramatic event like a server power outage, or a single write if something
# wrong with the Redis process itself happens, but the operating system is
# still running correctly.
#
# AOF and RDB persistence can be enabled at the same time without problems.
# If the AOF is enabled on startup Redis will load the AOF, that is the file
# with the better durability guarantees.
#
# Please check http://redis.io/topics/persistence for more information.

appendonly no

# The name of the append only file (default: "appendonly.aof")
appendfilename "appendonly.aof"

# The fsync() call tells the Operating System to actually write data on disk
# instead of waiting for more data in the output buffer. Some OS will really flush
# data on disk, some other OS will just try to do it ASAP.
#
# Redis supports three different modes:
#
# no: don't fsync, just let the OS flush the data when it wants. Faster.
# always: fsync after every write to the append only log. Slow, Safest.
# everysec: fsync only one time every second. Compromise.
#
# The default is "everysec", as that's usually the right compromise between
# speed and data safety. It's up to you to understand if you can relax this to
# "no" that will let the operating system flush the output buffer when
# it wants, for better performances (but if you can live with the idea of
# some data loss consider the default persistence mode that's snapshotting),
# or on the contrary, use "always" that's very slow but a bit safer than
# everysec.
#
# More details please check the following article:
# http://antirez.com/post/redis-persistence-demystified.html
#
# If unsure, use "everysec".

always:同步持久化，每次数据变更会立刻记录到磁盘，性能较差但是数据完整性较好
everysec:异步操作，每秒记录一次。

# appendfsync always
appendfsync everysec
# appendfsync no

# When the AOF fsync policy is set to always or everysec, and a background
# saving process (a background save or AOF log background rewriting) is
# performing a lot of I/O against the disk, in some Linux configurations
# Redis may block too long on the fsync() call. Note that there is no fix for
# this currently, as even performing fsync in a different thread will block
# our synchronous write(2) call.
#
# In order to mitigate this problem it's possible to use the following option
# that will prevent fsync() from being called in the main process while a
# BGSAVE or BGREWRITEAOF is in progress.
#
# This means that while another child is saving, the durability of Redis is
# the same as "appendfsync none". In practical terms, this means that it is
# possible to lose up to 30 seconds of log in the worst scenario (with the
# default Linux settings).
#
# If you have latency problems turn this to "yes". Otherwise leave it as
# "no" that is the safest pick from the point of view of durability.
重写时是否可以运行appendfsync,用默认no即可，保证数据安全性

no-appendfsync-on-rewrite no

# Automatic rewrite of the append only file.
# Redis is able to automatically rewrite the log file implicitly calling
# BGREWRITEAOF when the AOF log size grows by the specified percentage.
#
# This is how it works: Redis remembers the size of the AOF file after the
# latest rewrite (if no rewrite has happened since the restart, the size of
# the AOF at startup is used).
#
# This base size is compared to the current size. If the current size is
# bigger than the specified percentage, the rewrite is triggered. Also
# you need to specify a minimal size for the AOF file to be rewritten, this
# is useful to avoid rewriting the AOF file even if the percentage increase
# is reached but it is still pretty small.
#
# Specify a percentage of zero in order to disable the automatic AOF
# rewrite feature.
auto-aof-rewrite-percentage : 设置重写的基准值
auto-aof-rewrite-min-size ： 设置重写的基准值


auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb

# An AOF file may be found to be truncated at the end during the Redis
# startup process, when the AOF data gets loaded back into memory.
# This may happen when the system where Redis is running
# crashes, especially when an ext4 filesystem is mounted without the
# data=ordered option (however this can't happen when Redis itself
# crashes or aborts but the operating system still works correctly).
#
# Redis can either exit with an error when this happens, or load as much
# data as possible (the default now) and start if the AOF file is found
# to be truncated at the end. The following option controls this behavior.
#
# If aof-load-truncated is set to yes, a truncated AOF file is loaded and
# the Redis server starts emitting a log to inform the user of the event.
# Otherwise if the option is set to no, the server aborts with an error
# and refuses to start. When the option is set to no, the user requires
# to fix the AOF file using the "redis-check-aof" utility before to restart
# the server.
#
# Note that if the AOF file will be found to be corrupted in the middle
# the server will still exit with an error. This option only applies when
# Redis will try to read more data from the AOF file but not enough bytes
# will be found.
aof-load-truncated yes
```



**aof和rdb可以共存，当两者共存时先找的是aof**



aof启动

将appendonly设置为yes

将有数据的aof文件复制一份保存到对应目录

恢复：重启redis然后重新加载（会将所有写操作重新执行一遍）

修复：redis-check-aof --fix进行修复，即将aof文件中的不符合规范的语句进行删除



rewrite

是什么：AOF采用文件追加的方式，文件会越来越大，为避免这种情况，新增重写机制，当AOF文件的大小超过所设定的阈值时，redis就会重新启动AOF文件的内容压缩，只保留可以恢复数据的最小指令集，可以使用命令bgrewriteaof

重写原理：aof文件持续增长而过大时，会fork出一条新进程来将文件重写（也就是先写临时文件最后再rename），遍历新进程的内存中数据，每条记录有一条set语句。重写aof文件的操作，并没有读取旧的aof文件，而是将整个内存中的数据库内容用命令的方式重写一个新的aof文件，这一点和快照有一点类似。

触发机制：redis会记录上一次重写时的aof大小，默认配置是当aof文件大小是上次rewrite后大小的一倍且文件大于64M时触发。

优点：

每秒同步

缺点：

相同数据集而言，aof文件要远大于rdb文件，恢复速度鳗鱼rdb

aof运行效率要慢于rdb，每秒同步策略效率较好，不同步效率和rdb相同

小结

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210208221917.png)



##### 选择哪一个

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210208222525.png)

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210208222650.png)



#### 事务

1. 是什么：可以一次执行多个命令，本质是一组命令的集合。一个事务中的所有命令都会序列化，**按顺序串行化执行而不会被其他命令插入，不许加塞**。

2. 能干嘛：一个队列中，一次性、顺序性、排他性的**执行一系列命令**。对强一致性要求严格。要么一起成功，要么一起失败

3. 常用命令：

discard：取消事务，放弃执行事务块内的所有命令

exec：执行所有事务块内的命令

multi：标记一个事务块的开始

unwatch：取消watch命令对所有key的监视

watch key：监视一个（或多个）key，如果在事务执行之前这个（或多个）key被其他命令所改动，那么事务将被打断。

```sql
#case1:正常执行；一次性执行多条语句
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> set k3 v3
QUEUED
127.0.0.1:6379> get k2
QUEUED
127.0.0.1:6379> exec
1) OK
```

```sql
#case2：放弃执行，不执行事务块中的语句
127.0.0.1:6379> multi
127.0.0.1:6379> set k2 22
QUEUED
127.0.0.1:6379> set k3 33
QUEUED
127.0.0.1:6379> discard
OK
127.0.0.1:6379> get k2
"v2"
127.0.0.1:6379> get k3
"v3"
```

```sql
#全体连坐，当事务块中的一条语句出错了，则事务块中的语句都不会被执行，即要么都执行，要么都不执行；注意这是在加入队列时就已经报错了
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> set k3 v3
QUEUED
127.0.0.1:6379> setget k4 v4
(error) ERR unknown command `setget`, with args beginning with: `k4`, `v4`,
127.0.0.1:6379> set k5 v5
QUEUED
127.0.0.1:6379> exec
(error) EXECABORT Transaction discarded because of previous errors.
```



```sql
#case4：这与case3的区别在于，在加入队列时不报错而是在执行exec时对出错的语句进行报错
127.0.0.1:6379> keys *
1) "k1"
2) "k2"
3) "k3"
127.0.0.1:6379> multi
OK
127.0.0.1:6379> incr k1
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> set k3 v3
QUEUED
127.0.0.1:6379> get k4
QUEUED
127.0.0.1:6379> exec
1) (error) ERR value is not an integer or out of range
2) OK
3) OK
4) (nil)
```



```sql
#case5:watch监控
```

- 悲观锁/乐观锁/CAS(check and set)

  - 悲观锁：每次去拿数据都认为别人会修改，所以<u>每次在拿数据的时候都会上锁</u>，这样别人想拿这个数据就会block直到它拿到锁。传统的关系型数据库就用到了很多这种锁机制，比如行锁，表锁，读锁，写锁等，都是**在做操作之前先上锁**。
  - 乐观锁：每次拿数据的时候都认为别人不会修改，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，可以使用版本号等机制。乐观锁适用于多读的应用类型，这样可以提高吞吐量。**乐观锁策略：提交版本必须大于记录当前版本才能执行更新**。
  - CAS：

- 初始化信用卡可用余额和欠额

- 无加塞篡改，先监控再开启multi，保证两笔金额变动在同一事务内

- 有加塞篡改

- unwatch

- 一旦执行exec，之前加的监控锁都会被取消掉

- 小结

  通过watch命令在事务执行之前监控了多个keys，如果在watch之后有任何key的值发生了改变，exec命令执行的事务都将被放弃，同时返回nil应答以通知调用者事务执行失败

```sql
#无加塞篡改
# 初始化信用卡可用余额balance和欠额debt
127.0.0.1:6379> set balance 100
OK
127.0.0.1:6379> set debt 0
OK
127.0.0.1:6379> keys *
1) "debt"
2) "balance"
#先监控watch
127.0.0.1:6379> watch balance
OK
#再开启multi，保证两笔金额变动在同一事务内
127.0.0.1:6379> multi
OK
127.0.0.1:6379> decrby balance 20
QUEUED
127.0.0.1:6379> incrby debt 20
QUEUED
127.0.0.1:6379> exec
1) (integer) 80
2) (integer) 20
```

```sql
#有加塞篡改
# 初始化信用卡可用余额balance和欠额debt
127.0.0.1:6379> set balance 100
OK
127.0.0.1:6379> set debt 0
OK
#先监控watch
127.0.0.1:6379> watch balance
OK

####
#此时修改
127.0.0.1:6379> set balance 800
OK
####

#再进行multi
127.0.0.1:6379> multi
OK
127.0.0.1:6379>
127.0.0.1:6379> decrby balance 20
QUEUED
127.0.0.1:6379> incrby debt 20
QUEUED

#此时执行事务失败，返回nil
127.0.0.1:6379> exec
(nil)
```

通过watch监控`balance`，如果`balance`在中间阶段被别人修改了，我们就不能够对其进行操作。通过这样的操作才能够保证`balance`变量的安全性。

4. 三阶段

- 开启：以multi开始一个事务
- 入队：将多个命令入队到事务中，接到这些命令并不会立即执行，而是放到等待执行的事务队列里面
- 执行：由exec命令触发事务

5. 三特性

- 单独的隔离操作：事务中的所有命令都会序列化、按顺序执行。事务在执行过程中，不会被其他客户端发送来的命令请求打断。
- 没有隔离级别的概念：队列中的命令没有提交之前都不会实际的被执行。因为事务提交前任何指令都不会被实际执行，也就不存在”事务内的查询要看到事务里的更新，在事务外查询不能看到“这个问题
- 不保证原子性：redis同一个事务如果有一天命令执行失败，其后的命令仍然会被执行，没有回滚。

#### redis发布订阅

是什么：进程间的通信模式：发送者（pub）发送消息，订阅者(sub)接收消息

`注意在实际的工作中不会使用redis进行消息的发布和订阅`

订阅/发布消息图

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210209163501.png)

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210209163550.png)



命令：

psubscribe pattern:订阅一个或多个符合给定模式的频道

pubsub subcommand：查看订阅与发布系统状态

publish channel message：将消息发送到指定的频道

punsubscribe pattern：退订所有给定模式的频道

subscribe channel ：订阅给定的一个或多个频道的信息

unsubscribe channel：退订给定的频道



案例：

**先订阅后发布**才能收到消息，

1.可以一次订阅多个：subscribe c1 c2 c3

```sql
127.0.0.1:6379> subscribe c1 c2 c3
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "c1"
3) (integer) 1
1) "subscribe"
2) "c2"
3) (integer) 2
1) "subscribe"
2) "c3"
3) (integer) 3
```



2.消息发布，publish c2 hello-redis

```sql
127.0.0.1:6379> publish c2 hello-redis
(integer) 1
```

此时

```sql
127.0.0.1:6379> subscribe c1 c2 c3
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "c1"
3) (integer) 1
1) "subscribe"
2) "c2"
3) (integer) 2
1) "subscribe"
2) "c3"
3) (integer) 3
1) "message"
2) "c2"
3) "hello-redis"
```

====================================================

3.订阅多个,使用通配符 **，psubscribe new *

```sql
127.0.0.1:6379> psubscribe new*
Reading messages... (press Ctrl-C to quit)
1) "psubscribe"
2) "new*"
3) (integer) 1
```



4.收取信息，publish new1 redis2015

```sql
127.0.0.1:6379> publish new1 redis2015
(integer) 1
```

此时

```sql
127.0.0.1:6379> psubscribe new*
Reading messages... (press Ctrl-C to quit)
1) "psubscribe"
2) "new*"
3) (integer) 1
1) "pmessage"
2) "new*"
3) "new1"
4) "redis2015"
```





#### redis复制(master/slave)

是什么：就是我们所说的主从复制，主机数据更新后根据配置和策略，自动同步到备机的master/slaver机制，master以写为主，slave以读为主

能干嘛：读写分离；容灾恢复

#### redis分片

分片是将数据划分为多个部分的方法 ，可以将数据存储在多台机器中，这种方法在解决某些问题时可以获得线性级别的性能提升。

![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210210204832.png)
























