# Linux相关


### Linux中tty是什么

tty：终端设备的统称。

tty一词源于Teletypes，或者teletypewriters，原来指的是电传打字机，是通过串行线用打印机键盘通过阅读和发送信息的东西，后来这东西被键盘与显示器取代，所以现在叫终端比较合适。终端是一种字符型设备，它有多种类型，通常使用tty来简称各种类型的终端设备。

**tty1～6是文本型控制台，tty7是X Window图形显示管理器。**

在本地机器上可以通过Ctrl+Alt+F1（F1-F7键）切换到对应的登录控制台。

> 所谓的窗口环境就是：文字界面加上X窗口软件！文字界面一定是存在的，只是窗口界面软件看你要不要启动而已



### 快捷键

- Tab：命令和文件名补全；
- Ctrl+C：中断正在运行的程序；
- Ctrl+D：结束键盘输入（End Of File，EOF）



### 帮助



#### 1. --help

指令的基本用法与选项介绍。



#### 2. man

man 是 manual 的缩写，将指令的具体信息显示出来。

当执行 `man date` 时，有 DATE(1) 出现，其中的数字代表指令的类型，常用的数字及其类型如下：

| 代号 | 类型                                            |
| ---- | ----------------------------------------------- |
| 1    | 用户在 shell 环境中可以操作的指令或者可执行文件 |
| 5    | 配置文件                                        |
| 8    | 系统管理员可以使用的管理指令                    |



#### 3. info

info 与 man 类似，但是 info 将文档分成一个个页面，每个页面可以跳转。



#### 4. doc

/usr/share/doc 存放着软件的一整套说明文件。

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210102100343.png)

### 关机



#### 1. who

在关机前需要先使用 who 命令查看有没有其它用户在线。



#### 2. sync

为了加快对磁盘文件的读写速度，位于内存中的文件数据不会立即同步到磁盘，因此关机之前需要先进行 sync 同步操作。

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210102102132.png)

#### 3. shutdown

```
## shutdown [-krhc] 时间 [信息]
-k ： 不会关机，只是发送警告信息，通知所有在线的用户
-r ： 将系统的服务停掉后就重新启动
-h ： 将系统的服务停掉后就立即关机
-c ： 取消已经在进行的 shutdown
```

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210102100730.png)

```bash
halt #关机
```





### PATH

可以在环境变量 PATH 中声明可执行文件的路径，路径之间用 : 分隔。

```
/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/home/dmtsai/.local/bin:/home/dmtsai/bin
```



### sudo

sudo 允许一般用户使用 root 可执行的命令，不过只有在 /etc/sudoers 配置文件中添加的用户才能使用该指令。

> ubuntu有以下方式切换到root身份
>
> sudo+命令，输入当前用户密码后以root权限执行命令，有时间限制且仅限当前命令。
>
> sudo -i，输入当前用户密码后以root权限登录shell，无时间限制。使用exit或logout退出。
>
> su，输入root账户的密码后切换到root身份，无时间限制。su 用户名切换其它用户。
>
> sudo su，效果同su，只是不需要root的密码，而需要当前用户的密码

### 包管理工具

RPM 和 DPKG 为最常见的两类软件包管理工具：

- RPM 全称为 Redhat Package Manager，最早由 Red Hat 公司制定实施，随后被 GNU 开源操作系统接受并成为许多 Linux 系统的既定软件标准。YUM 基于 RPM，具有依赖管理和软件升级功能。
- 与 RPM 竞争的是基于 Debian 操作系统的 DEB 软件包管理工具 DPKG，全称为 Debian Package，功能方面与 RPM 相似。



### 发行版

Linux 发行版是 Linux 内核及各种应用软件的集成版本。

| 基于的包管理工具 | 商业发行版 | 社区发行版      |
| ---------------- | ---------- | --------------- |
| RPM              | Red Hat    | Fedora / CentOS |
| DPKG             | Ubuntu     | Debian          |

### VIM

#### VIM 三个模式

<img src="https://gitee.com/shilongshen/image-bad/raw/master/img/20201228145339.png" style="zoom:80%;" />

- 一般指令模式（Command mode）：VIM 的默认模式，可以用于移动游标查看内容；
- 编辑模式（Insert mode）：按下 "i" 等按键之后进入，可以对文本进行编辑；
- 指令列模式（Bottom-line mode）：按下 ":" 按键之后进入，用于保存退出等操作。

在指令列模式下，有以下命令用于离开或者保存文件。

| 命令 | 作用                                                         |
| ---- | ------------------------------------------------------------ |
| :w   | 写入磁盘                                                     |
| :w!  | 当文件为只读时，强制写入磁盘。到底能不能写入，与用户对该文件的权限有关 |
| :q   | 离开                                                         |
| :q!  | 强制离开不保存                                               |
| :wq  | 写入磁盘后离开                                               |
| :wq! | 强制写入磁盘后离开                                           |

#### vi显示行号

1. 按下 冒号 ：
2. 输入set number

| 命令   | 作用                                             |
| ------ | ------------------------------------------------ |
| dd     | 删除光标所在行                                   |
| \#dd   | 例如，6dd表示删除从光标所在的该行往下数6行之文字 |
| x      | 每按一次删除光标所在位置的后面一个字符           |
| /word  | 在文件中查找内容为word的字符串（向下查找）       |
| ?word  | 在文件中查找内容为word的字符串（向上查找）       |
| Ctrl+B | 屏幕往上移动一页                                 |
| Ctrl+F | 屏幕往下移动一页                                 |



# 磁盘

## 

## 磁盘结构

- 盘面（Platter）：一个磁盘有多个盘面
- 磁道（Track）：盘面上的圆形带状区域，一个盘面可以有多个磁道
- 扇区(Track Sector)：磁道上的**一个弧段**，一个磁道可以有多个扇区，它是**最小的物理存储单位**，目前主要有512bytes和4K两种大小
- 磁头（Head）：与盘面非常接近，能够将判别上的磁场转换为电信号（读），或者将电信号转换为盘面的磁场（写）
- 制动手臂（Actuator arm）:用于在磁道中移动磁头
- 主轴（Spindle）：使得整个盘面转动。

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201210205647.png)

下面将分别介绍

磁道（Track）如下图，数据存储在磁道上

<img src="https://gitee.com/shilongshen/image-bad/raw/master/img/20201228155748.png" style="zoom: 33%;" />



扇区：

<img src="https://gitee.com/shilongshen/image-bad/raw/master/img/20201228160036.png" style="zoom:33%;" />



<img src="https://gitee.com/shilongshen/image-bad/raw/master/img/20201228160200.png" style="zoom:33%;" />

如何确定数据存储在硬盘上的哪一个位置，实现快速读写数据（这么多文件存在磁盘上该如何快速的找到需要读写的数据呢？）

有两种方式：

## 分区表

在写数据时，再将数据存储到磁道后，会将分区表中记录数据存储位置对应的扇区和簇，这能够**大致**的定位数据的位置

在读数据时，首先查找分区表，找到数据对应的存储位置并进行读取（通过移动磁头）。

磁盘的分区有两种方式：一种是限制较多的MBR分区，一种是较新且限制较少的GPT分区

### MBR分区

MBR 中，第一个扇区最重要，里面有主要开机记录（Master boot record, MBR）及分区表（partition table），其中主要开机记录占 446 bytes，分区表占 64 bytes。

- 分区表：记录整个硬盘的分区状态，仅有4个分区，每个区段记录了该区段的驱动与结束的磁柱号码

  <img src="https://gitee.com/shilongshen/image-bad/raw/master/img/20201228163759.png" style="zoom: 80%;" />

分区表只有 64 bytes，最多只能存储 4 个分区，这 4 个分区为主分区（Primary）和扩展分区（Extended）。其中<u>扩展分区只有一</u>个，它使**用其它扇区来记录额外的分区表**，因此通过扩展分区可以分出更多分区，这些分区称为逻辑分区。

<img src="https://gitee.com/shilongshen/image-bad/raw/master/img/20201228164650.png" style="zoom:80%;" />

Linux 也把分区当成文件，分区文件的命名方式为：磁盘文件名 + 编号，例如 /dev/sda1。注意，逻辑分区的编号从 5 开始。

假设上面的硬盘装置文件名为/dev/sda 时,那么这四个分区槽在 Linux 系统中的装置文件名如下所示,
**重点在于档名后面会再接一个数字**,这个数字与该分区槽所在的位置有关。

- P1：/dev/sda1
- P2: /dev/sda2
- P3: /dev/sda3
- P4: /dev/sda4

### GPT分区

扇区是磁盘的最小存储单位，旧磁盘的扇区大小通常为 512 bytes，而最新的磁盘支持 4 k。GPT 为了兼容所有磁盘，**在定义扇区上使用逻辑区块地址（Logical Block Address, LBA），LBA 默认大小为 512 bytes**。

GPT 第 1 个区块记录了主要开机记录（MBR），紧接着是 33 个区块记录分区信息，并把最后的 33 个区块用于对分区信息进行备份。这 33 个区块**第一个为 GPT 表头纪录，这个部份纪录了分区表本身的位置与大小和备份分区的位置**，同时放置了分区表的校验码  (CRC32)，操作系统可以根据这个校验码来判断 GPT 是否正确。若有错误，可以使用备份分区进行恢复。

**GPT 没有扩展分区概念，都是主分区**，每个 LBA 可以分 4 个分区，因此总共可以分 4 * 32 = 128 个分区。

MBR 不支持 2.2 TB 以上的硬盘，GPT 则最多支持到 233 TB = 8 ZB。

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201228165720.png)

## 开机检测程序

### BIOS

BIOS（Basic Input/Output System，基本输入输出系统），它是一个固件或称为韧体,（嵌入在硬件中的软件），BIOS 程序存放在断电后内容不会丢失的只读内存中。

 ![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201228170746.png)

BIOS 是开机的时候计算机执行的**第一个程序**，这个程序知道可以开机的磁盘，并读取磁盘第一个扇区的主要开机记录（MBR），由主要开机记录（MBR）执行其中的开机管理程序，这个开机管理程序会加载操作系统的核心文件。

主要开机记录（MBR）中的开机管理程序提供以下功能：选单、载入核心文件以及转交其它开机管理程序。**转交**这个功能可以用来实现**多重引导**，只需要将另一个操作系统的开机管理程序安装在其它分区的启动扇区上，在启动开机管理程序时，就可以通过选单选择启动当前的操作系统或者转交给其它开机管理程序从而启动另一个操作系统。

下图中，第一扇区的主要开机记录（MBR）中的开机管理程序提供了两个选单：M1、M2，M1 指向了 Windows 操作系统，而 M2 指向其它分区的启动扇区，里面包含了另外一个开机管理程序，提供了一个指向 Linux 的选单。

 ![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201228170920.png)



安装多重引导，最好先安装 Windows 再安装 Linux。因为安装 Windows 时会覆盖掉主要开机记录（MBR），而 Linux 可以选择将开机管理程序安装在主要开机记录（MBR）或者其它分区的启动扇区，并且可以设置开机管理程序的选单。（即双系统安装）

### UEFI

BIOS 不可以读取 GPT 分区表，而 UEFI 可以。

## 磁盘的文件名

Linux 中每个硬件都被当做一个文件，包括磁盘。磁盘以磁盘接口类型进行命名，常见磁盘的文件名如下：

- IDE 磁盘：/dev/hd[a-d]
- SATA/SCSI/SAS 磁盘：/dev/sd[a-p]

**其中文件名后面的序号的确定与系统检测到磁盘的顺序有关，而与磁盘所插入的插槽位置无关**。

---

> Linux 也把分区当成文件，分区文件的命名方式为：磁盘文件名 + 编号，例如 /dev/sda1。注意，逻辑分区的编号从 5 开始。
>
> 假设上面的硬盘装置文件名为/dev/sda 时,那么这四个分区槽在 Linux 系统中的装置文件名如下所示,
> **重点在于档名后面会再接一个数字**,这个数字与该分区槽所在的位置有关。
>
> - P1：/dev/sda1
> - P2: /dev/sda2
> - P3: /dev/sda3
> - P4: /dev/sda4

---



# 文件系统

在磁盘分区完毕后需要进行**格式化**，之后操作系统才能够使用这个文件系统。

> 为什么需要进行“格式化”？
>
> ​	因为**每种操作系统所设定的文件属性/权限并不相同**，为了存放这些文件所需的数据，因此需要将分区槽进行格式化，以成为操作系统能够利用的“**文件系统格式**”
>
> Linux的正统文件系统格式为Ext2；默认情况下windows操作系统是不会认识Linux的Ext2的。
>
> 一个分区通常只能格式化为一个文件系统，但是磁盘阵列等技术可以将一个分区格式化为多个文件系统。

## 组成

Q：文件系统是如何运作的？（以Ext2为例）

A：操作系统的文件数据除了<u>文件实际内容</u>外，通常具有非常多的<u>属性</u>，例如Linux操作系统的文件权限	  （rwx）与文件属性（拥有者，群组，时间参数等）。**文件系统通常会将这两部分的数据分别存放在不同的区块，权限与属性放置到inode中，至于实际数据则放置在data block区块中。另外还有一个超级区块（superblock）会记录整个文件系统的整体信息，包括inode与block的总量、使用量、剩余量等。**

> 文件系统一开始就将inode和block规划好了，除非重新格式化，否则inode和block固定后就不再变动。
>
> Linux文件系统Ext2就是使用这种inode为基础的文件系统

- inode: 记录权限和属性，一个文件占用一个inode，同时记录此文件的数据所在的block号码；
- block ：实际数据，若文件太大，会占用多个block
- superblock：整个文件系统的整体信息

每个inode与block都有编号。同时inode内有文件数据放置的block号码。因此，如果能够找到文件的inode的话，就可以知道这个文件所放置数据的block号码，也就可以读到该文件的实际数据了。这能够<u>提高文件读写的效率</u>。

## 文件读取

对于 Ext2 文件系统，当要读取一个文件的内容时，先在 inode 中查找文件内容所在的所有 block，然后把所有 block 的内容读出来。这种数据读取的方式我们称为**索引式文件系统**。

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210109103707.png)



而对于 FAT 文件系统，它没有 inode，每个 block 中存储着下一个 block 的编号。文件系统没有办法一下得知需要读取的block号码，因此需要一个一个将block读出后，才会知道下一个block在处。如果同一个文件写入的block过于分散，读取的效率就比较低。

> **碎片整理**：需要碎片整理的原因就是同一个文件写入的block太过于离散，此时文件读取的效率很低，这个时候就可以通过碎片整理将同一个文件的block汇整到一起，这样数据的读取就会比较容易。

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210109104026.png)



## 碎片整理

指一个文件内容所在的 block 过于分散。

**碎片整理**：需要碎片整理的原因就是同一个文件写入的block太过于离散，此时文件读取的效率很低，这个时候就可以通过碎片整理将同一个文件的block汇整到一起，这样数据的读取就会比较容易。

## block

在 Ext2 文件系统中所支持的 block 大小有 1K，2K 及 4K 三种，不同的大小限制了单个文件和文件系统的最大大小。

| 大小         | 1KB  | 2KB   | 4KB  |
| ------------ | ---- | ----- | ---- |
| 最大单一文件 | 16GB | 256GB | 2TB  |
| 最大文件系统 | 2TB  | 8TB   | 16TB |

一个 block 只能被一个文件所使用，未使用的部分直接浪费了。因此如果需要存储大量的小文件，那么最好选用比较小的 block。

## inode

inode 具体包含以下信息：

- 权限 (read/write/excute)；
- 拥有者与群组 (owner/group)；
- 容量；
- 建立或状态改变的时间 (ctime)；
- 最近读取时间 (atime)；
- 最近修改时间 (mtime)；
- 定义文件特性的旗标 (flag)，如 SetUID...；
- 该文件真正内容的指向 (pointer)。

inode 具有以下特点：

- 每个 inode 大小均固定为 128 bytes (新的 ext4 与 xfs 可设定到 256 bytes)；
- 每个文件都仅会占用一个 inode。
- 文件系统能够建立的文件数量与inode的数量相关
- 系统读取文件时需要先找到inode，并分析inode所记录的权限与用户是否相符，若符合才能够开始实际读取block的内容

inode 中记录了文件内容所在的 block 编号，但是每个 block 非常小，一个大文件随便都需要几十万的 block。而一个  inode 大小有限，无法直接引用这么多 block 编号。因此引入了间接、双间接、三间接引用。间接引用让 inode 记录的引用 block  块记录引用信息。

 <img src="https://gitee.com/shilongshen/image-bad/raw/master/img/20210109105946.png" style="zoom: 50%;" />

## superblock

superblock是记录整个文件系统相关信息的地方，其记录的主要信息有：

- block与inode的总量
- 未使用与已使用的inode/block数量
- block与inode的大小（block为1,2,4K,inode为128bytes或256bytes）
- 文件系统的挂载时间、最近一次写入数据的时间、最近一次检验磁盘的时间等相关信息
- 一个vaild bit数值，若此文件系统已被挂载，则vaild bit为0，若未被挂载则vaild bit为1.

superblock的大小为1024bytes，可以通过dumoe2fs来查看

## block bitmap

记录 block 是否被使用的位图。

## inode bitmap

记录inode 是否被使用的位图。

## 目录的权限

建立一个目录时，会分配一个 inode 与至少一个 block。block 记录的内容是目录下所有文件的 inode 编号以及文件名。

可以看到文件的 inode 本身不记录文件名，<u>文件名记录在目录中</u>，因此**新增文件、删除文件、更改文件名这些操作与目录的写权限有关**。

> 文件名不是存储在一个文件的内容中，而是存储在一个文件所在的目录中。因此，拥有文件的 w 权限并不能对文件名进行修改。
>
> 目录存储文件列表，一个目录的权限也就是对其文件列表的权限。因此，目录的 r 权限表示可以读取文件列表；w  权限表示可以修改文件列表，具体来说，就是添加删除文件，对文件名进行修改；x 权限可以让该目录成为工作目录，x 权限是 r 和 w  权限的基础，如果不能使一个目录成为工作目录，也就没办法读取文件列表以及对文件列表进行修改了。

## 日志

如果突然断电，那么文件系统会发生错误，例如断电前只修改了 block bitmap，而还没有将数据真正写入 block 中。

ext3/ext4 文件系统引入了日志功能，可以利用日志来修复文件系统。

## 挂载

将文件系统与目录树结合的动作称为挂载

- 挂载点一定是目录

挂载利用目录作为文件系统的进入点，也就是说，进入目录之后就可以读取文件系统的数据。

## 链接

通过前面的学习可以得知：

- 每个文件都会占用一个inode，文件内容有inode的记录来指向
- 想要读取该文件，必须要经过目录记录的文件名来指向到正确的inode号码

即文件名只于目录有关，文件内容则和inode有关。

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210109194023.png)

### 实体链接（hard link）

在目录下创建一个条目，记录着**文件名与 inode 编号**，这个 inode 就是源文件的 inode。

<u>删除任意一个条目，文件还是存在，</u>只要引用数量不为 0。

有以下限制：不能跨越文件系统、不能对目录进行链接。

```bash
## ln /etc/crontab .
## ll -i /etc/crontab crontab
34474855 -rw-r--r--. 2 root root 451 Jun 10 2014 crontab
34474855 -rw-r--r--. 2 root root 451 Jun 10 2014 /etc/crontab
```

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210109195450.png)

上图说明：我们可以通过inode 1或inode 2 找到对应的目录block，目录block中存储的文件名虽然不相同，但是存储的inode是相同的，所以都是指向同一个文件。

### 符号链接(symbolic link)

符号链接文件保存着源文件所在的**绝对路径**，在读取时会定位到源文件上，可以理解为 Windows 的快捷方式。

<u>当源文件被删除了，链接文件就打不开了。</u>

因为记录的是路径，所以**可以为目录建立符号链接**。

```
## ll -i /etc/crontab /root/crontab2
34474855 -rw-r--r--. 2 root root 451 Jun 10 2014 /etc/crontab
53745909 lrwxrwxrwx. 1 root root 12 Jun 23 22:31 /root/crontab2 -> /etc/crontab
```

![image-20210109195832856](/home/ssl/.config/Typora/typora-user-images/image-20210109195832856.png)



```bash
ln [-sf] 源文件 目标文件
-s ： 如果不加任何参数就进行链接，那就是hard link，而加上-s后就是symbolic link
-f ： 如果目标文件存在时，就主动将目标文件直接移除后再建立
```





# 文件权限与目录配置

## 文件属性

Linux一般将文件可存取的身份分为三个类别，分别是Owner/group/others，且这三种身份各有read/write/execute等权限。

对于文件来说：

- r(read): 可以读取此文件的实际内容
- w(write): 可以编辑、新增或修改文件内容（但是不可以删除）
- x(eXecute): 该文件具有可以被系统执行的权限

对于目录来说：

- r(read): 具有读取目录结构列表的权限；可以通过`ls`将该目录的内容列表显示出来
- w(write): 具有异变该目录结构列表的权限；
  - 创建文件与目录
  - 删除文件和目录（无论该文件的权限是什么）
  - 将文件和目录进行重命名
  - 移动该目录中的文件、目录位置
- x(eXecute): 表示用户能否**进入该目录**成为工作目录

<img src="https://gitee.com/shilongshen/image-bad/raw/master/img/20210102105023.png" style="zoom:50%;" />



> 我们以王三毛为例，王三毛这个“文件”的拥有者为王三毛，他属于王大毛这个群组，而张小猪相对于王三毛，则是一个“其他人（others）”
>
> 天神为“root”，可以到达任何想去的地方

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210102105823.png)



使用`ls -al`查看一个文件时，会显示一个文件的信息，例如 `drwxr-xr-x 3 root root 17 May 6 00:14 .config`，对这个信息的解释如下：

- **d**rwxr-xr-x：文件类型以及权限，第 1 位为**文件类型字段**，后 9 位为**文件权限字段**
- 3：链接数

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210102113651.png)

- root：文件拥有者
- root：所属群组
- 17：文件大小（默认单位为bytes）
- May 6 00:14：文件<u>最后被修改</u>的时间
- .config：文件名

常见的文件类型及其含义有：

- d：目录
- -：文件
- l：链接文件

9 位的文件权限字段中，<u>每 3 个为一组，共 3 组</u>，<u>每一组分别代表对文件拥有者、所属群组以及其它人的文件权限</u>。一组权限中的 3 位分别为 r、w、x 权限，表示可读、可写、可执行。（如果没有权限就用`-`表示）

文件时间有以下三种：

- modification time (mtime)：文件的内容更新就会更新；
- status time (ctime)：文件的状态（权限、属性）更新就会更新；
- access time (atime)：读取文件时就会更新。

再来一个例子：

```
drwxrwxrwx  6 root root 4096 1月   1 15:13 ssl

目录名：ssl
最后修改时间1月   1 15:13
文件大小4096
所属群组：root
文件拥有者：root
d表示这是个目录
rwxrwxrwx：表示文件拥有者，所属群组，其他都可对该目录进行读、写、执行操作
```

再来一个例子：

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210102114018.png)

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210102114240.png)

> 进入本目录要具备X权限



## 修改文件属性和权限

### 常用指令

- chgrp : 改变文件所在群组
- chown : 改变文件所有者
- chmod : 改变文件的权限，SUID,SGID,SBIT等特性

> 注意：chown的使用
>
> 1.如果要连目录下的所有次目录或文件同时更改文件拥有者的话，直接加上 -R 的选项即可
>
> 2.可以同时修改文件拥有者和群组，使用‘ : ’
>
> 3.可以单独修改群组，使用' . '

```bash
chown -R ssl 12

root@ssl-H310M-S2:/home/ssl/桌面/12# ll
总用量 8
drwxr-xr-x 2 ssl ssl 4096 1月   2 14:16 ./
drwxr-xr-x 4 ssl ssl 4096 1月   2 14:16 ../
-rw-rw-r-- 1 ssl ssl    0 1月   2 14:00 12.txt

chown root:root 12.txt
-rw-rw-r-- 1 root root    0 1月   2 14:00 12.txt

chown .ssl 12.txt
-rw-rw-r-- 1 root ssl    0 1月   2 14:00 12.txt
```

### 使用数字来修改文件权限

可以将一组权限用数字来表示，此时一组权限的 3 个位当做二进制数字的位，从左到右每个位的权值为 4、2、1，即每个权限对应的数字权值为 。

- r : 4
- w : 2
- x : 1

每种身份（owner/group/others）各自的三个权限(r/w/x)分数是需要累加的，例如当权限为`-rwxrwx---`分数为：

- owner = rwx = 4+2+1 =7
- group = rwx = 4+2+1 =7
- others = --- =0+0+0 = 0

所以我们更改权限时：

```bash
之前的文件属性为：-rw-r--r-- 1 root ssl    0 1月   2 14:25 123.txt

变更文件属性：
chmod 770 123.txt

变更后的文件属性：
-rwxrwx--- 1 root ssl    0 1月   2 14:25 123.txt*

```

### 使用符号来设定权限

用符号表示

- u : user --文件拥有者
- g : group --群组
- o : others --其他
- a : all --全部的身份

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210102144627.png)

例如当权限为`-rwxrw----`时：

```bash
chmod u=rwx,g=rw 123.txt

-rwxrw---- 1 root ssl    7 1月   2 14:38 123.txt*

```

```bash
#使文件的每一个人均可写入
chmod a=w 123.txt
--w--w--w- 1 root ssl    7 1月   2 14:38 123.txt

#注意到使用'='会将其他的权限改变

#将文件的权限去掉而不改变其他的权限
chmod o-w 123.txt
--w--w---- 1 root ssl    7 1月   2 14:38 123.txt

#将文件的权限加上而不改变其他的权限
chmod o+w 123.txt
--w--w--w- 1 root ssl    7 1月   2 14:38 123.txt
```



### 文件预设权限umask

```bash
umask
0007
umask -R 
u=rwx,g=rwx,o=
```

注意umask的分数是该默认值需要减掉的权限。以上面为例子，因为umask为007所以权限为u=rwx,g=rwx,o=--- 。

  创建时，文件  默认666，目录默认777，减去umask的位就是结果。



**umask用于当前用户在建立文件或目录时候的权限默认值。**

### 文件隐藏属性chattr

```bash
chattr [+-] [选项]
选项：
i:让文件无法被删除、改名、无法写入或新增数据
a:只能增加数据，不能删除页不能修改数据
```



```bash
root@ssl-H310M-S2:/home/ssl/桌面/12# chattr +i test44  #添加i隐藏属性
root@ssl-H310M-S2:/home/ssl/桌面/12# rm test44		 #root也无法删除
rm: 无法删除'test44': 不允许的操作
root@ssl-H310M-S2:/home/ssl/桌面/12# chattr -i test44   #解除i隐藏属性
root@ssl-H310M-S2:/home/ssl/桌面/12# rm test44
root@ssl-H310M-S2:/home/ssl/桌面/12#
```

lsattr（显示文件隐藏属性）

### 文件的特殊权限SUID，SGID，SBIT

SUID：

- SUID权限仅对于二进制程序有效

- 执行者对于该程序需要具有X的可执行权限
- 本权限仅在执行该程序的过程中有效
- 执行者将具有该程序拥有者的权限

```bash
(base) ssl@ssl-H310M-S2:~/PycharmProjects$ ll /usr/bin/passwd
-rwsr-xr-x 1 root root 54256 3月  27  2019 /usr/bin/passwd*
```

`当s标志在文件拥有者的x位置时为SUID，当s在群组的x时为SGID`

与SUID不同，SGID可以针对文件或目录来设定









## 目录配置

为了使不同 Linux 发行版本的目录结构保持一致性，Filesystem Hierarchy Standard (FHS) 规定了 Linux 的目录结构。最基础的三个目录如下：

（FHS的重点在于规范每个目录下应该要放置什么样的数据）

- / (root, 根目录)：与开机系统有关
- /usr (unix software resource)：所有系统默认软件都会安装到这个目录；
- /var (variable)：存放系统或程序运行过程中的数据文件。

1. 所有目录都由根目录衍生出来，同时根目录也与开机/还原/系统修复等动作有关。

   ![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210103145424.png)

# 文件和目录管理



> . 表示当前目录
>
> .. 表示上层目录
>
> 所有目录底下都会有两个目录，分别是 . 与 .. 分别表示此层目录与上层目录

> 绝对路径和相对路径
>
> 绝对路径：一定从根目录/开始写起，例如/usr/share/local
>
> 相对路径：不是有/开始写起,例如cd ../man

### 文件与目录的基本操作

#### 

#### 1. ls

列出文件或者目录的信息，目录的信息就是其中包含的文件。

```
## ls [-aAdfFhilnrRSt] file|dir
-a ：列出全部的文件
-d ：仅列出目录本身
-l ：以长数据串行列出，包含文件的属性与权限等等数据
```

#### 

#### 2. cd

更换当前目录。

```
cd [相对路径或绝对路径]
cd .. :返回上层目录
cd - :返回之前的目录
```

#### 

#### 3. mkdir

创建目录。

```
## mkdir [-mp] 目录名称
-m ：配置目录权限
-p ：递归创建目录
```

#### 

#### 4. rmdir

删除目录，目录必须为空。

```
rmdir [-p] 目录名称
-p ：递归删除目录
```

#### 

#### 5. touch

更新文件时间或者建立新文件。

```
## touch [-acdmt] filename
-a ： 更新 atime
-c ： 更新 ctime，若该文件不存在则不建立新文件
-m ： 更新 mtime
-d ： 后面可以接更新日期而不使用当前日期，也可以使用 --date="日期或时间"
-t ： 后面可以接更新时间而不使用当前时间，格式为[YYYYMMDDhhmm]
```

#### 

#### 6. cp

复制文件。如果源文件有两个以上，则**目的文件一定要是目录**才行。

```
cp [-adfilprsu] source destination
-a ：相当于 -dr --preserve=all
-d ：若来源文件为链接文件，则复制链接文件属性而非文件本身
-i ：若目标文件已经存在时，在覆盖前会先询问
-p ：连同文件的属性一起复制过去
-r ：递归复制
-u ：destination 比 source 旧才更新 destination，或 destination 不存在的情况下才复制
--preserve=all ：除了 -p 的权限相关参数外，还加入 SELinux 的属性, links, xattr 等也复制了
```

#### 

#### 7. rm

删除文件。

```
## rm [-fir] 文件或目录
-r ：递归删除
```

#### 

#### 8. mv

移动文件。或重命名

```bash
## mv [-fiu] source destination
## mv [options] source1 source2 source3 .... directory
-f ： force 强制的意思，如果目标文件已经存在，不会询问而直接覆盖

mv 12.txt 45.txt #将文件12.txt重命名为45.txt
```

#### 9.df

```bash
df -h #查看目录挂载点

(base) wag@wag-SYS-7049GP-TRT:/home$ df -h
文件系统        容量  已用  可用 已用% 挂载点
udev             63G     0   63G    0% /dev
tmpfs            13G  235M   13G    2% /run
/dev/nvme0n1p2  916G  699G  171G   81% /
tmpfs            63G  6.0G   57G   10% /dev/shm
tmpfs           5.0M  4.0K  5.0M    1% /run/lock
tmpfs            63G     0   63G    0% /sys/fs/cgroup
/dev/nvme0n1p1  511M  3.7M  508M    1% /boot/efi
tmpfs            13G  108K   13G    1% /run/user/1000
/dev/sda        3.6T  422G  3.0T   13% /home/ssl

```

```bash
df -h [目录名] #查看目录挂载点

(base) wag@wag-SYS-7049GP-TRT:/home$ df -h ./wag
文件系统        容量  已用  可用 已用% 挂载点
/dev/nvme0n1p2  916G  699G  171G   81% /
```

#### 10.du

```bash
du -sh [目录或文件] #查看目录或文件的大小

(base) wag@wag-SYS-7049GP-TRT:/home$ du -sh ./wag
du: 无法读取目录'./wag/.cache/dconf': 权限不够
237G	./wag
```





### 文件内容查阅

#### 1. cat

从第一行开始显示文件内容。

```
## cat [-AbEnTv] filename
-n ：打印出行号，连同空白行也会有行号，-b 不会
```

```bash
cat 45.txt
asdas 
asdas

#附加打印行号
cat -n 45.txt
1	asdas 
2	asdas
3	aasdas
```



#### 2. tac

是 cat 的反向操作，从最后一行开始开始显示文件内容。

#### 

#### 3. more

和 cat 不同的是它可以一页一页查看文件内容，比较适合大文件的查看。

```bash
more linux命令速查.md

# 文件和目录
~~~

cd /home    #进入 '/ home' 目录'
cd ..       #返回上一级目录
cd ../..    #返回上两级目录
cd          #进入个人的主目录
cd ~user1   #进入个人的主目录
cd -       #返回上次所在的目录
pwd        #显示工作路径

ls      #查看目录中的文件
ls -F   #查看目录中的文件
ls -l   #显示文件和目录的详细资料
ls -a   #显示隐藏文件
--更多--(4%)
#此时可以使用的几个快捷键：
1.space ： 向下翻一页
2.enter : 向下翻一行
3. q : 离开
4. b : 往回翻页
```



#### 4. less

和 more 类似，但是多了一个向前翻页的功能。

```bash
less linux命令速查.md
# 文件和目录
~~~

cd /home    #进入 '/ home' 目录'
cd ..       #返回上一级目录
cd ../..    #返回上两级目录
cd          #进入个人的主目录
cd ~user1   #进入个人的主目录
cd -       #返回上次所在的目录
pwd        #显示工作路径

ls      #查看目录中的文件
ls -F   #查看目录中的文件
ls -l   #显示文件和目录的详细资料
ls -a   #显示隐藏文件
linux命令速查.md
#此时可以使用的几个快捷键
1. space ： 向下翻一页
2. pageup : 向上翻一页
3. pagedown : 向下翻一页
4. q : 离开
```



#### 5. head

只看文件前几行。

```
## head [-n number] filename
-n ：后面接数字，代表显示几行的意思
```

```bash
head linux命令速查.md

# 文件和目录
~~~

cd /home    #进入 '/ home' 目录'
cd ..       #返回上一级目录
cd ../..    #返回上两级目录
cd          #进入个人的主目录
cd ~user1   #进入个人的主目录
cd -       #返回上次所在的目录
#默认显示前10行，但是可以通过-n来修改,例如head -2 linux命令速查.md 只显示前两行
```



#### 6. tail

是 head 的反向操作，只看文件的后几行。

#### 

#### 7. od

以字符或者十六进制的形式显示二进制文件。

#### 8.nl

显示的时候同时输出行号

```bash
nl 45.txt
1	asdas 
2	asdas
3	aasdas

```

### 指令与文件搜索

#### 1. which

指令搜索。

```
## which [-a] command
-a ：将所有指令列出，而不是只列第一个
```

```bash
root@ssl-H310M-S2:/home/ssl/桌面/12# which ls
/bin/ls
```



#### 2. whereis

文件搜索。速度比较快，因为它只搜索几个特定的目录。

whereis主要针对/bin/sbin底下的执行档，以及/usr/share/man底下的man page文件，跟几个比较特定的目录来处理。

```
## whereis [-bmsu] dirname/filename
```

```bash
root@ssl-H310M-S2:/home/ssl/桌面/12# whereis ifconfig
ifconfig: /sbin/ifconfig /usr/share/man/man8/ifconfig.8.gz
```



#### 3. locate

文件搜索。可以用关键字或者正则表达式进行搜索。

locate 使用 /var/lib/mlocate/ 这个数据库来进行搜索，它存储在内存中，并且每天更新一次，所以无法用 locate 搜索新建的文件。可以使用 updatedb 来立即更新数据库。

```
## locate [-ir] keyword
-r：正则表达式
```

```bash
root@ssl-H310M-S2:/home/ssl/桌面/12# locate -l 5 passwd
/etc/passwd
/etc/passwd-
/etc/cron.daily/passwd
/etc/init/passwd.conf
/etc/pam.d/chpasswd
```



#### 4. find

文件搜索。可以使用文件的属性和权限进行搜索。

```
## find [basedir] [option]
example: find . -name "shadow*"
```

**① 与时间有关的选项**

```
-mtime  n ：列出在 n 天前的那一天修改过内容的文件
-mtime +n ：列出在 n 天之前 (不含 n 天本身) 修改过内容的文件
-mtime -n ：列出在 n 天之内 (含 n 天本身) 修改过内容的文件
-newer file ： 列出比 file 更新的文件
```

+4、4 和 -4 的指示的时间范围如下：

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210105095230.png)



**② 与文件拥有者和所属群组有关的选项**

```
-uid n  #用户的账号ID
-gid n  #组名ID
-user name  #用户名
-group name  #组名
-nouser ：搜索拥有者不存在 /etc/passwd 的文件
-nogroup：搜索所属群组不存在于 /etc/group 的文件
```

**③ 与文件权限和名称有关的选项**

```
-name filename
-size [+-]SIZE：搜寻比 SIZE 还要大 (+) 或小 (-) 的文件。这个 SIZE 的规格有：c: 代表 byte，k: 代表 1024bytes。所以，要找比 50KB 还要大的文件，就是 -size +50k
-type TYPE
-perm mode  ：搜索权限等于 mode 的文件
-perm -mode ：搜索权限包含 mode 的文件
-perm /mode ：搜索权限包含任一 mode 的文件
```

### 观察文件类型：file

了解文件的基本类型，例如是ASCII或者是data文件，或者是binary

```bash
(base) ssl@ssl-H310M-S2:~/PycharmProjects$ file ~/.bashrc
/home/ssl/.bashrc: ASCII text
```



# 文件的压缩和打包

## 压缩文件名

Linux 底下有很多压缩文件名，常见的如下：

| 扩展名    | 压缩程序                              |
| --------- | ------------------------------------- |
| *.Z       | compress                              |
| *.zip     | zip                                   |
| *.gz      | gzip                                  |
| *.bz2     | bzip2                                 |
| *.xz      | xz                                    |
| *.tar     | tar 程序打包的数据，没有经过压缩      |
| *.tar.gz  | tar 程序打包的文件，经过 gzip 的压缩  |
| *.tar.bz2 | tar 程序打包的文件，经过 bzip2 的压缩 |
| *.tar.xz  | tar 程序打包的文件，经过 xz 的压缩    |

## 压缩指令

### 1. gzip

gzip 是 Linux 使用最广的压缩指令，可以解开 compress、zip 与 gzip 所压缩的文件。

经过 gzip 压缩过，源文件就不存在了。

有 9 个不同的压缩等级可以使用。

可以使用 zcat、zmore、zless 来读取压缩文件的内容。

```bash
$ gzip [-cdtv#] filename #
-c ：将压缩的数据输出到屏幕上
-d ：解压缩
-t ：检验压缩文件是否出错
-v ：显示压缩比等信息
-# ： # 为数字的意思，代表压缩等级，数字越大压缩比越高，默认为 6
```

### 2. bzip2

提供比 gzip 更高的压缩比。

查看命令：bzcat、bzmore、bzless、bzgrep。

```
$ bzip2 [-cdkzv#] filename
-k ：保留源文件
```

### 3. xz

提供比 bzip2 更佳的压缩比。

可以看到，gzip、bzip2、xz 的压缩比不断优化。不过要注意的是，压缩比越高，压缩的时间也越长。

查看命令：xzcat、xzmore、xzless、xzgrep。

```
$ xz [-dtlkc#] filename
```

## tar

**tar — The GNU version of the tar archiving utility**

压缩指令只能对一个文件进行压缩，而打包能够<u>将多个文件打包成一个大文件</u>。tar 不仅可以用于打包，也可以使用 gzip、bzip2、xz 将打包文件进行压缩。

```
$ tar [cv] [-z|-j|-J]  [-f 新建的 tar 文件] filename...  ==打包压缩
$ tar [tv] [-z|-j|-J]  [-f 已有的 tar 文件]              ==查看
$ tar [xv] [-z|-j|-J]  [-f 已有的 tar 文件] [-C 目录]    ==解压缩

-z ：使用 zip；
-j ：使用 bzip2；
-J ：使用 xz；
-c ：新建打包文件；常用  --create, create a new archive
           
-t ：查看打包文件里面有哪些文件；常用 --list, list the contents of an archive
           
-x ：解打包或解压缩的功能；常用 -x, --extract, --get, extract files from an archive
           
-v ：在压缩/解压缩的过程中，显示正在处理的文件名；常用
-f : filename：要处理的文件，这个参数是最后一个参数，后面只能接档案名。；常用
-C 目录 ： 在特定目录解压缩。
```

| 使用方式 | 命令                                                 |
| -------- | ---------------------------------------------------- |
| 打包压缩 | tar  cvf filename.tar.bz2 ‘要被压缩的文件或目录名称’ |
| 查 看    | tar tvf filename.tar.bz2                             |
| 解压缩   | tar xvf filename.tar.bz2                             |

[查看](https://www.cnblogs.com/luozeng/p/8674529.html)

# bash

Linux中的命令行界面称为shell （图形界面称为GUI）。

bash shell（bash）是大多数Linux系统的默认shell，还用很多其他的shell ,例如ksh和csh。

> 文本模式登录后所取得的程序被称为壳（shell），这是因为这只程序负责最外面跟使用者（我们）打交道，所以被戏称为“壳”
>
> 在Linux中的shell为bash

shell也只是一个普通的用户程序。它仅仅需要从键盘读取数据、向显示器输出数据和运行其他程序的能力。

可以将一系列shell命令放到一个文件中，然后将此文件作为shell的输入来运行。包含shell命令的文件称为**shell脚本**。

---

Linux的命令行用户界面（shell）中包含大量的标准应用程序。这些程序大致可以分为以下6类：

- 文件和目录操作命令
- 过滤器
- 程序设计工具，如编辑器和编译器
- 文档处理
- 系统管理
- 其他

## 内核结构



![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201214144601.png)



运行在硬件之上的为操作系统，其作用为控制硬件并且为其他程序提供系统调用接口。这些系统调用允许用户创立并管理进程、文件以及其他资源。

Linux具有三种不同的接口：真正的系统调用接口，库函数接口，由标准应用程序构成的接口。

## 特性

- 命令历史：记录使用过的命令，快捷键：history
- 命令与文件补全：快捷键：tab
- 命名别名：例如 ll 是 ls -al 的别名 ，快捷键：alias
- shell scripts
- 通配符：例如 ls -l /usr/bin/X* 列出 /usr/bin 下面所有以 X 开头的文件



## 变量操作

对一个变量赋值直接使用 =。

对变量取用需要在变量前加上"$"  也可以用 "​\${}" 的形式；

输出变量使用 echo 命令。

```
$ x=abc
$ echo $x
$ echo ${x}
```

变量内容如果有空格，必须使用双引号或者单引号。

- 双引号内的特殊字符可以保留原本特性，例如 x="lang is $LANG"，则 x 的值为 lang is zh_TW.UTF-8；
- 单引号内的特殊字符就是特殊字符本身，例如 x='lang is $LANG'，则 x 的值为 lang is $LANG。

可以使用 `指令` 或者 $(指令) 的方式将指令的执行结果赋值给变量。例如 version=$(uname -r)，则 version 的值为 4.15.0-22-generic。

可以使用 **export 命令将自定义变量转成环境变量**，环境变量可以在子程序中使用，所谓子程序就是由当前 Bash 而产生的子 Bash。

```bash
(base) ssl@ssl-H310M-S2:~/桌面/12$ bash
(base) ssl@ssl-H310M-S2:~/桌面/12$ exit
exit
(base) ssl@ssl-H310M-S2:~/桌面/12$
```

- 可以通过`env`查看环境变量，也可以通过`export`来查看

```bash
(base) ssl@ssl-H310M-S2:~/桌面/12$ env
XDG_VTNR=7
LC_PAPER=zh_CN.UTF-8
LC_ADDRESS=zh_CN.UTF-8
XDG_SESSION_ID=c2
XDG_GREETER_DATA_DIR=/var/lib/lightdm-data/ssl
LC_MONETARY=zh_CN.UTF-8
CLUTTER_IM_MODULE=xim
...
```

- 通过`set`观察所有变量（包括环境变量和自定义变量）
- 使用 **export 命令将自定义变量转成环境变量**

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20210109212851.png)

子程序仅会继承父程序的环境变量，子程序不会继承父程序的自定义变量。





Bash 的变量可以声明为数组和整数数字。注意数字类型没有浮点数。如果不进行声明，默认是字符串类型。变量的声明使用 declare 命令：

```
$ declare [-aixr] variable
-a ： 定义为数组类型
-i ： 定义为整数类型
-x ： 定义为环境变量
-r ： 定义为 readonly 类型
```

使用 [ ] 来对数组进行索引操作：

```
$ array[1]=a
$ array[2]=b
$ echo ${array[1]}
```

## 历史命令history

```bash
history ：查看所有历史命令
！num : 运行第[num]个命令
```

```bash
(base) ssl@ssl-H310M-S2:~/桌面/12$ history
...
 2075  cat /bin/shells
 2076  cat /etc/shells
 
(base) ssl@ssl-H310M-S2:~/桌面/12$ !2076
cat /etc/shells
# /etc/shells: valid login shells
/bin/sh
/bin/dash
/bin/bash
/bin/rbash

```



## 指令搜索顺序

- 以绝对或相对路径来执行指令，例如 /bin/ls 或者 ./ls ；
- 由别名找到该指令来执行；
- 由 Bash 内置的指令来执行；
- 按 $PATH 变量指定的搜索路径的顺序找到第一个指令来执行。



## 数据流重定向

重定向指的是使用文件代替标准输入、标准输出和标准错误输出。

| 1                     | 代码 | 运算符    |
| --------------------- | ---- | --------- |
| 标准输入 (stdin)      | 0    | < 或 <<   |
| 标准输出 (stdout)     | 1    | > 或 >>   |
| 标准错误输出 (stderr) | 2    | 2> 或 2>> |

其中，有一个箭头的表示以覆盖的方式重定向，而有两个箭头的表示以追加的方式重定向。

可以将不需要的标准输出以及标准错误输出重定向到 /dev/null，相当于扔进垃圾箱。

如果需要将标准输出以及标准错误输出同时重定向到一个文件，需要将某个输出转换为另一个输出，例如 2>&1 表示将标准错误输出转换为标准输出。

```
$ find /home -name .bashrc > list 2>&1
```

```bash
(base) ssl@ssl-H310M-S2:~/桌面/12$ cat 45link.txt   #标注输出是直接输出到屏幕上的
asdas 
asdas
aasdas

(base) ssl@ssl-H310M-S2:~/桌面/12$ cat 45link.txt > 56.txt  #通过输出重定向操作可以将输出输出到指定位置
(base) ssl@ssl-H310M-S2:~/桌面/12$ cat 56.txt 
asdas 
asdas
aasdas
```

## 双向输出重定向

输出重定向会将输出内容重定向到文件中，而   **tee**   不仅能够完成这个功能，还能保留屏幕上的输出。也就是说，使用 tee 指令，一个输出会同时传送到文件和屏幕上。

```
$ tee [-a] file
```



## 管道指令

管道是**将一个命令的标准输出作为另一个命令的标准输入**，在数据需要经过多个步骤的处理之后才能得到我们想要的内容时就可以使用管道。

在命令之间使用 | 分隔各个管道命令。

```
$ ls -al /etc | less
```

## 提取指令

cut 对数据进行切分，取出想要的部分。

切分过程一行一行地进行。

```
$ cut
-d ：分隔符
-f ：经过 -d 分隔后，使用 -f n 取出第 n 个区间
-c ：以字符为单位取出区间
```

示例 1：last 显示登入者的信息，取出用户名。

```
$ last
root pts/1 192.168.201.101 Sat Feb 7 12:35 still logged in
root pts/1 192.168.201.101 Fri Feb 6 12:13 - 18:46 (06:33)
root pts/1 192.168.201.254 Thu Feb 5 22:37 - 23:53 (01:16)

$ last | cut -d ' ' -f 1
```

示例 2：将 export 输出的信息，取出第 12 字符以后的所有字符串。

```
$ export
declare -x HISTCONTROL="ignoredups"
declare -x HISTSIZE="1000"
declare -x HOME="/home/dmtsai"
declare -x HOSTNAME="study.centos.vbird"
.....(其他省略).....

$ export | cut -c 12-
```

### 

## 排序指令

**sort**   用于排序。

```
$ sort [-fbMnrtuk] [file or stdin]
-f ：忽略大小写
-b ：忽略最前面的空格
-M ：以月份的名字来排序，例如 JAN，DEC
-n ：使用数字
-r ：反向排序
-u ：相当于 unique，重复的内容只出现一次
-t ：分隔符，默认为 tab
-k ：指定排序的区间
```

示例：/etc/passwd 文件内容以 : 来分隔，要求以第三列进行排序。

```
$ cat /etc/passwd | sort -t ':' -k 3
root:x:0:0:root:/root:/bin/bash
dmtsai:x:1000:1000:dmtsai:/home/dmtsai:/bin/bash
alex:x:1001:1002::/home/alex:/bin/bash
arod:x:1002:1003::/home/arod:/bin/bash
```

**uniq**   可以将重复的数据只取一个。

```
$ uniq [-ic]
-i ：忽略大小写
-c ：进行计数
```

示例：取得每个人的登录总次数

```
$ last | cut -d ' ' -f 1 | sort | uniq -c
1
6 (unknown
47 dmtsai
4 reboot
7 root
1 wtmp
```

### 



## 字符转换指令

**tr**   用来删除一行中的字符，或者对字符进行替换。

```
$ tr [-ds] SET1 ...
-d ： 删除行中 SET1 这个字符串
```

示例，将 last 输出的信息所有小写转换为大写。

```
$ last | tr '[a-z]' '[A-Z]'
```

**col**   将 tab 字符转为空格字符。

```
$ col [-xb]
-x ： 将 tab 键转换成对等的空格键
```

**expand**   将 tab 转换一定数量的空格，默认是 8 个。

```
$ expand [-t] file
-t ：tab 转为空格的数量
```

**join**   将有相同数据的那一行合并在一起。

```
$ join [-ti12] file1 file2
-t ：分隔符，默认为空格
-i ：忽略大小写的差异
-1 ：第一个文件所用的比较字段
-2 ：第二个文件所用的比较字段
```

**paste**   直接将两行粘贴在一起。

```
$ paste [-d] file1 file2
-d ：分隔符，默认为 tab
```

### 

## 分区指令

**split**   将一个文件划分成多个文件。

```
$ split [-bl] file PREFIX
-b ：以大小来进行分区，可加单位，例如 b, k, m 等
-l ：以行数来进行分区。
- PREFIX ：分区文件的前导名称
```



## grep

- 简单来说，正则表达式就是处理字符串的方法，他**以行为单位**来进行字符串的处理行为，可以让使用者轻易的达到“搜寻/删除/取代”某个特定字符串。

grep（globally search a regular expression and print)，**使用正则表示式进行全局查找并打印**。

```
$ grep [-acinv] [--color=auto] 搜寻字符串 filename
-c ： 统计匹配到行的个数
-i ： 忽略大小写
-n ： 输出行号
-v ： 反向选择，也就是显示出没有 搜寻字符串 内容的那一行
--color=auto ：找到的关键字加颜色显示
```

示例：把含有 the 字符串的行提取出来（注意默认会有 --color=auto 选项，因此以下内容在 Linux 中有颜色显示 the 字符串）

```
$ grep -n 'the' regular_express.txt
8:I can't finish the test.
12:the symbol '*' is represented as start.
15:You are the best is mean you are the no. 1.
16:The world Happy is the same with "glad".
18:google is the best tools for search keyword
```

示例：正则表达式 a{m,n} 用来匹配字符 a m~n 次，这里需要将 { 和 } 进行转义，因为它们在 shell 是有特殊意义的。

```
$ grep -n 'a\{2,5\}' regular_express.txt
```

### 

## printf

用于格式化输出。它不属于管道命令，在给 printf 传数据时需要使用 $( ) 形式。

```
$ printf '%10s %5i %5i %5i %8.2f \n' $(cat printf.txt)
    DmTsai    80    60    92    77.33
     VBird    75    55    80    70.00
       Ken    60    90    70    73.33
```

### 

## awk

是由 Alfred Aho，Peter Weinberger 和 Brian Kernighan 创造，awk 这个名字就是这三个创始人名字的首字母。

awk 每次处理一行，处理的最小单位是字段，每个字段的命名方式为：$n，n 为字段号，从 1 开始，$0 表示一整行。

示例：取出最近五个登录用户的用户名和 IP。首先用 last -n 5 取出用最近五个登录用户的所有信息，可以看到用户名和 IP 分别在第 1 列和第 3 列，我们用 $1 和 $3 就能取出这两个字段，然后用 print 进行打印。

```
$ last -n 5
dmtsai pts/0 192.168.1.100 Tue Jul 14 17:32 still logged in
dmtsai pts/0 192.168.1.100 Thu Jul 9 23:36 - 02:58 (03:22)
dmtsai pts/0 192.168.1.100 Thu Jul 9 17:23 - 23:36 (06:12)
dmtsai pts/0 192.168.1.100 Thu Jul 9 08:02 - 08:17 (00:14)
dmtsai tty1 Fri May 29 11:55 - 12:11 (00:15)
$ last -n 5 | awk '{print $1 "\t" $3}'
dmtsai   192.168.1.100
dmtsai   192.168.1.100
dmtsai   192.168.1.100
dmtsai   192.168.1.100
dmtsai   Fri
```

可以根据字段的某些条件进行匹配，例如匹配字段小于某个值的那一行数据。

```
$ awk '条件类型 1 {动作 1} 条件类型 2 {动作 2} ...' filename
```

示例：/etc/passwd 文件第三个字段为 UID，对 UID 小于 10 的数据进行处理。

```
$ cat /etc/passwd | awk 'BEGIN {FS=":"} $3 < 10 {print $1 "\t " $3}'
root 0
bin 1
daemon 2
```

awk 变量：

| 变量名称 | 代表意义                     |
| -------- | ---------------------------- |
| NF       | 每一行拥有的字段总数         |
| NR       | 目前所处理的是第几行数据     |
| FS       | 目前的分隔字符，默认是空格键 |

示例：显示正在处理的行号以及每一行有多少字段

```
$ last -n 5 | awk '{print $1 "\t lines: " NR "\t columns: " NF}'
dmtsai lines: 1 columns: 10
dmtsai lines: 2 columns: 10
dmtsai lines: 3 columns: 10
dmtsai lines: 4 columns: 10
dmtsai lines: 5 columns: 9
```

# 进程管理



## 查看进程

### 1. ps

查看某个时间点的进程信息。

示例：查看自己的进程

```shell
ps -l
```

示例：查看系统所有进程

```shell
ps aux
```

示例：查看含有threadx的进程 （**常用**）

```shell
ps aux | grep threadx
#或则使用
ps ef | grep thraad
```

```markdown
ps [-aefFly] [-p pid] [-u userid]

-a 与任何用户标识和终端相关的进程

-e 所有进程（包括守护进程）

-p pid 与指定PID相关的进程

-u userid 与指定用户标识userid相关的进程

-ef 显示所有用户进程，完整输出

-a 显示所有非守护进程

-t 仅显示所有守护进程
```



#### 

### 2. pstree

查看进程树。

示例：

```shell
pstree -A  #查看所有进程树
pstree -p 22772  #查看进程22772中的线程数
pstree -p 22772 | wc -l  #查看进程下的线程数量
```



#### 

### 3. top

实时显示进程信息。

示例：两秒钟刷新一次

```shell
top -d 2
top -H  #查看机器性能
```

#### 

### 4. netstat

查看占用端口的进程

示例：查看特定端口的进程

```
## netstat -anp | grep 'port'
```

- -a或--all 显示所有连线中的Socket。
- -n或--numeric 直接使用IP地址，而不通过域名服务器。
- -p或--programs 显示正在使用Socket的程序识别码和程序名称。

## 进程状态

| 状态 | 说明                                                         |
| ---- | ------------------------------------------------------------ |
| R    | running or runnable (on run queue) 正在执行或者可执行，此时进程位于执行队列中。 |
| D    | uninterruptible sleep (usually I/O) 不可中断阻塞，通常为 IO 阻塞。 |
| S    | interruptible sleep (waiting for an event to complete)   可中断阻塞，此时进程正在等待某个事件完成。 |
| Z    | zombie (terminated but not reaped by its parent) 僵死，进程已经终止但是尚未被其父进程获取信息。 |
| T    | stopped (either by a job control signal or because it is being traced)   结束，进程既可以被作业控制信号结束，也可能是正在被追踪。 |



 [![img](https://camo.githubusercontent.com/ae6622fbe9bed2d92a67e2cac0bbcb86fcf5e015a789aab89faf6b5e0fdb41e5/68747470733a2f2f63732d6e6f7465732d313235363130393739362e636f732e61702d6775616e677a686f752e6d7971636c6f75642e636f6d2f32626162343132372d336537642d343863632d393134652d3433366265383539666230352e706e67)](https://camo.githubusercontent.com/ae6622fbe9bed2d92a67e2cac0bbcb86fcf5e015a789aab89faf6b5e0fdb41e5/68747470733a2f2f63732d6e6f7465732d313235363130393739362e636f732e61702d6775616e677a686f752e6d7971636c6f75642e636f6d2f32626162343132372d336537642d343863632d393134652d3433366265383539666230352e706e67) 





### SIGCHLD

当一个子进程改变了它的状态时（停止运行，继续运行或者退出），有两件事会发生在父进程中：

- 得到 SIGCHLD 信号；
- waitpid() 或者 wait() 调用会返回。

其中子进程发送的 SIGCHLD 信号包含了子进程的信息，比如进程 ID、进程状态、进程使用 CPU 的时间等。

<u>在子进程退出时，它的进程描述符不会立即释放，这是为了让父进程得到子进程信息，父进程通过 wait() 和 waitpid() 来获得一个已经退出的子进程的信息。</u>

### wait()

```
pid_t wait(int *status)
```

父进程调用 wait() 会一直阻塞，直到收到一个子进程退出的 SIGCHLD 信号，之后 wait() 函数会销毁子进程并返回。

如果成功，返回被收集的子进程的进程 ID；如果调用进程没有子进程，调用就会失败，此时返回 -1，同时 errno 被置为 ECHILD。

参数 status 用来保存被收集的子进程退出时的一些状态，如果对这个子进程是如何死掉的毫不在意，只想把这个子进程消灭掉，可以设置这个参数为 NULL。

### 

### waitpid()

```
pid_t waitpid(pid_t pid, int *status, int options)
```

作用和 wait() 完全相同，但是多了两个可由用户控制的参数 pid 和 options。

pid 参数指示一个子进程的 ID，表示只关心这个子进程退出的 SIGCHLD 信号。如果 pid=-1 时，那么和 wait() 作用相同，都是关心所有子进程退出的 SIGCHLD 信号。

options 参数主要有 WNOHANG 和 WUNTRACED 两个选项，WNOHANG 可以使 waitpid() 调用变成非阻塞的，也就是说它会立即返回，父进程可以继续执行其它任务。

### 

## 孤儿进程

**一个父进程退出，而它的一个或多个子进程还在运行，那么这些子进程将成为孤儿进程。**

孤儿进程将被 init 进程（进程号为 1）所收养，并由 init 进程对它们完成状态收集工作。

由于孤儿进程会被 init 进程收养，所以孤儿进程不会对系统造成危害。

### 

## 僵尸进程

一个子进程的进程描述符在子进程退出时不会释放，只有当父进程通过 wait() 或 waitpid()  获取了子进程信息后才会释放。**如果子进程退出，而父进程并没有调用 wait() 或  waitpid()，那么子进程的进程描述符仍然保存在系统中，这种进程称之为僵尸进程。**

僵尸进程通过 ps 命令显示出来的状态为 Z（zombie）。

系统所能使用的进程号是有限的，如果产生大量僵尸进程，将因为没有可用的进程号而导致系统不能产生新的进程。

要消灭系统中大量的僵尸进程，只需要将其父进程杀死，此时僵尸进程就会变成孤儿进程，从而被 init 进程所收养，这样 init 进程就会释放所有的僵尸进程所占有的资源，从而结束僵尸进程。

# 账户管理

## 新建只能在控制台下登录的用户

使用此方法无法在图像界面看到自己的家目录

```bash
useradd [-u UID] [-g 初始群组] [-c 说明栏] [-d 家目录绝对路径] 使用者账号名

# -u ： UID，是一组数字
# -d : 制定某个目录成为家目录，使用绝对路径

```

```bash
psaawd [账号名称] #设定密码
```

```bash
userdel [账号名称] #删除账号
```

```bash
cat /etc/passwd #查看用户属性

test4:x:1003:1003:,,,:/home/test4:/bin/bash

用户名:[密码口令]:用户标识号:组标识号:注释性描述:用户主目录:命令解释程序
```

```bash
su test4  #切换用户

root@ssl-H310M-S2:/home# su test4
test4@ssl-H310M-S2:/home$ cd ~
test4@ssl-H310M-S2:~$ pwd
/home/test4
test4@ssl-H310M-S2:~$
```

可以看到登陆以后的用户t 录仍为“/home”;**这种方式只能在控制台中互相切换用户**，一旦重启系统，用该用户还是无法登陆（只能用原来的用户或root登陆）。

## 新建可登录图形用户界面的用户

**推荐使用这种方式**

```bash
adduser [账号名]
---
adduser [--home DIR] [--shell SHELL] [--no-create-home] [--uid ID]
[--firstuid ID] [--lastuid ID] [--gecos GECOS] [--ingroup GROUP | --gid ID]
[--disabled-password] [--disabled-login] [--encrypt-home] UserName
   添加普通用户


---
root@ssl-H310M-S2:/home# adduser --home DIR test4     //使用--home指定新用户的家目录
正在添加用户"test4"...
正在添加新组"test4" (1003)...
正在添加新用户"test4" (1003) 到组"test4"...
创建主目录"/home/test4"...
正在从"/etc/skel"复制文件...
输入新的 UNIX 密码： 
重新输入新的 UNIX 密码： 
passwd：已成功更新密码
正在改变 test4 的用户信息
请输入新值，或直接敲回车键以使用默认值
	全名 []: 
	房间号码 []: 
	工作电话 []: 
	家庭电话 []: 
	其它 []: 
这些信息是否正确？ [Y/n] y
```

两种方式最大的差别在于新建用户的命令不同，**第一种是useradd, 第二种是adduser**。相对应的，如果要删除用户，第一种的命令为userdel, 第二种是deluser.

> adduser：会自动为创建的用户指定主目录、系统shell版本，会在创建时输入用户密码。 
>  useradd：需要使用参数选项指定上述基本设置，如果不使用任何参数，则创建的用户无密码、无主目录、没有指定shell版本。

```
/etc/passwd - 使 用 者 帐 号 资 讯，可以查看用户信息
/etc/shadow - 使 用 者 帐 号 资 讯 加 密
/etc/group - 群 组 资 讯
/etc/default/useradd - 定 义 资 讯
/etc/login.defs - 系 统 广 义 设 定
/etc/skel - 内 含 定 义 档 的 目 录
```

## 常用命令

### 赋予用户root权限

[参考](https://blog.csdn.net/yzf279533105/article/details/88704233)

```shell
方法一： 修改 /etc/sudoers 文件，找到下面一行，把前面的注释（#）去掉

## Allows people in group wheel to run all commands
%wheel    ALL=(ALL)    ALL

然后修改用户，使其属于root组（wheel），命令如下：

#usermod -g root UserName

修改完毕，现在可以用UserName帐号登录，然后用命令 su – ，即可获得root权限进行操作。

方法二： 修改 /etc/sudoers 文件，找到下面一行，在root下面添加一行，如下所示：

## Allow root to run any commands anywhere
root    ALL=(ALL)     ALL
UserName   ALL=(ALL)     ALL

修改完毕，现在可以用UserName帐号登录，然后用命令 su – ，即可获得root权限进行操作。
```



### 查看系统中所有用户

```shell
grep bash /etc/passwd
```

### 用户切换

```shell
su UserName
```



# 参考

https://github.com/CyC2018/CS-Notes/blob/master/notes/Linux.md#linux

鸟 哥 的 Linux 私 房 菜 基 础 篇 第 四 版

# 常见问题

1. Linux执行ls，会引起哪些系统调用（B,C,D）

```
A.nmap
B.read
C.execve
D.fork
```

任何shell都会执行 exec 和 fork, 而  ls会执行read

2. cat -n file1file2 命令的意思是？

```
把文件file1和file2连在一起，然后输出到屏幕上。
```

3. 使用vi编辑某文件时，要将第7到10行的内容一次性删除，可以在命令模式下先将光标移到第7行，再使用（ B）命令

```
A.dd  #删除光标所在的行
B.4dd #
C.de
D.4de
```

  ndd：  删除当前行开始的连续  n  行。

  dd：删除光标所在行，

  n1,n2d：  删除n1到n2行，例如删除1到10行1,10d

   n，$d：删除从某行开始至文本末尾，例如删除第8行至末尾    8,$d  

4. linux命令执行成功后会返回

```
0
```

成功返回0，不成功返回不同的值

5. 以下哪一个命令只查找源代码、二进制文件和帮助文件，而不是所以类型的文件？此命令查找的目录是由环境变量$PATH指定的

```
A.whereis
B.whatis
C.which
D.apropos
```

  whereis 可查询二进制文件(-b)、帮助文档(-m)、源程序（-s），无选项时，返回所有结果，-u(除上述三种的其它文件) 

  which  查看可执行文件的位置 

  whatis  查询命令有什么功能 

  apropos  搜索指定关键字的命令 

6. 有一个文件ip.txt，每行一条ip记录，共若干行，已排好序，下面哪个命令可以实现“统计出现次数最多的前3个ip及其次数”？（ B ） 

```
A.uniq -c ip.txt

B.uniq -c ip.txt | sort -nr | head -n 3

C.cat ip.txt | count -n | sort -rn | head -n 3

D.cat ip.txt | count -n
```

- `uniq`命令：报告或删去**重复行**，加上`-c参数可以统计重复行出现的次数（放在每行开头）。
- `sort`命令：对文本按行进行排序，`-n`参数表示根据数字大小排序；`-r`，对应英文单词是reverse，意思是反转排序结果，`sort`**默认是从小到大排序**，加上这个参数可以实现从大到小排序。
- `head`命令：取文件的前一部分（默认输出前10行）。加`-n [数字]`可以指定到底是前几行。
  把这三个命令的作用都搞懂，再加上一点 *Linux管道符* 的知识，这道题目也就很容易解决了。最好是自己动手在命令行下实践几次，加深印象，容易记牢。

假设一个文件ip,其中的内容为

```txt
10.249
10.249
10.249
10.249
10.249
10.249
10.256
10.256
10.126
10.126
10.126
10.126
10.555
```



```bash
(base) ssl@ssl-H310M-S2:~/桌面/12$ uniq -c ip  #统计重复出现行的次数
      6 10.249
      2 10.256
      4 10.126
      1 10.555
```

```bash
(base) ssl@ssl-H310M-S2:~/桌面/12$ uniq -c ip | sort -nr #按照出现的次数降序排序
      6 10.249
      4 10.126
      2 10.256
      1 10.555
```

```bash
(base) ssl@ssl-H310M-S2:~/桌面/12$ uniq -c ip | sort -nr | head -n 3 #去除前三行
      6 10.249
      4 10.126
      2 10.256

```

7. 如何查看当前Linux系统的状态,如CPU使用,内存使用,负载情况，下列描述正确的是（A,B,C）？

```
A.可以使用top命令分析CPU使用，内存使用，负载等情况
B.可以使用free查看内存整体的使用情况
C.可以使用cat /proc/meminfo查看内存更详细的情况
```

  **top**命令：  

  Linux下常用的性能分析工具。能够**实时**显示系统中各个进程对资源的占用状况。 

  **free**命令：  

  可以显示Linux系统中空闲的、已用的物理内存及swap内存，及被内核使用的buffer。 

  **df命令：** 

  用于显示当前在Linux系统上的文件系统的磁盘使用情况的统计信息。

8. 进程之间通信都有哪些方式？(A.B.C)

```
A.共享内存
B.消息传递
C.系统管道
D.临界区
```

9. 写出完成以下功能的Linux命令: 将文件xyz中的单词AAA全部替换为BBB (C)

   ```
   A.sed 's/AAA/BBB' xyz
   B.sed 's/AAA/BBB/g' xyz
   C.replace 's/AAA/BBB/p' xyz
   D.replace 's/AAA/BBB/d' xyz
   ```

s表示替换命令，/AAA/表示匹配AAA，/BBB/表示把匹配替换成BBB，/g 表示一行上的替换所有的匹配。

  sed 's/aaa/bbb/' filea  将filea中的第一个aaa替换为bbb 

  sed 's/aaa/bbb/g' filea 将filea中的所有的aaa替换为bbb 

这里有一点需要指出的是，这条命令并不能修改源文件的内容，而只是把替换后的文件内容输出，如若想改变原来文件的内容的话，可以使用如下命令：
  sed 's/AAA/BBB/g' xyz > xyz.tmp

10. 如果系统的umask设置为244，创建一个新文件后，它的权限：（）

```
-r---w--w-
```

  umask是从权限中“拿走”相应的位,且文件创建时不能赋予执行权限.

  创建时，文件  默认666，目录默认777，减去umask的位就是结果。

11. 软件项目存储于/ftproot，允许apache用户修改所有程序，设置访问权限的指令？

```
chmod  777  /ftproot
```
