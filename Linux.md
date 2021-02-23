Linux

## Linux命令

查看centos的版本：

```shell
cat /etc/redhat-release 
```

查看内核版本

```bash
uname -r
```

查看网卡

```bash
mii-tool ens33
```

查看多少人在登录

```bash
who
```

查看系统磁盘

```shell
lsblk
```

`head` 显示文件的前10行

`pwd` (print working directory) 打印工作目录

`ll -d /root` -d 参数表示只查看目录，不看目录里的内容，一般用来查看一个目录的权限

`ls -lS /boot/` 参数大写S按大小（size）排序展示 ，大小以字节显示

`ls -lSh /boot/` 参数h（human-readable)，以人类读取方式显示字节大小

`复制` 选中即复制

`粘贴` 按鼠标滚轮即粘贴

`alias la="ls -a"` 设置`la`为`ls -a`的别名 （临时设置，重启之后无效) 

`永久有效的设置别名`(当前用户)：将别名设置写入在文件`/root/bashrc`下，`source /root/bashrc`使之生效

`永久有效设置别名`（所有用户都生效）：将别名设置写入在文件/etc/bashrc下

`cd -`返回切换前的目录。

`history` ：显示历史记录。

`history -c`: 清除历史记录。（只清除当前终端上的历史记录）

`vi /root/.bash_history` : 清除某个用户的历史记录(以前留下的都会记录在里面)

<font style='color:orange'>快速查找历史命令的技巧：</font>

> ctrl + r   => 输入某条命令的关键字 =>找出来对应的命令，按向右键。
>
> ！45 => !加命令历史id数字（此命令会直接执行） 
>
> `!$` 执行上一个命令的最后一个参数

`hwclock` : 硬件时钟。主板BIOS里的时钟

`date`: 系统时钟。Linux内核在启动时会读取硬件时间之后再同步。

时间的几种参数类型：

> UTC( Universal Time Coordinated)：世界标准时间
>
> GMT（Greenwich Mean Time)： 格林尼治时间
>
> CST ( China Standard Time)：中国标准时间

`date -s '2020-12-22'`将时间设置成字符串中的时间

`date "+%F"` 取时间字符串。F完整时间格式= `%Y-%m-%d'

`date -d "+1 months" +%F` : 在当时时间上的加一个月，再显示出来

`time` time用来测试一个命令运行的时间，后面直接跟上命令与参数

```shell
time ls -l /etc/
```

## shell

```bash
cat /ect/shells
```

查看当前用的 shell 类型

```bash
echo $SHELL
```

可以用`>`创建一个文件

```bash
> file
```

## 命令提示符

prompt

[root@localhost ~]#

`#` 管理员

`$` 普通用户

显示提示符的格式

​	[root@localhost ~]# echo #PS1

修改提示符的格式

```bash
PS1="\[\e[1;5;41;33;m\][\u@\h \W]\\$\[\e[0m\]"
```

​	1 高亮 

​	41-47 背景色

​	31-37 字体色

​	\e \033            \u 当前用户

​	\h 主机名的简称      \H 主机名全称

​	\w 当前工作目录		\W 当前工作目录基名

​	\t 24小时时间格式		\T 12 小时时间格式

​	\\! 命令历史数 			\\# 开机后命令历史数

将命令保存在`/etc/profile.d/`下，生成一个.sh 文件

## 开机登录

```bash
vim /etc/gdm/custom.conf
GDM configuration storage
[daemon]
AutomaticLoginEnable=true
AutomaticLogin=root
```



## 开机登录提示信息

`/etc/motd` 这个文件下的内容在每次登录后都会提示

motd: message of the day

## rpm包的安装

- 直接yum安装

- 安装`安装系统`中的安装包

  ```shell
  mount /dev/sr0 /mnt
  cd /mnt
  rpm -ivh BaseOS/Packages/xxx.rpm
  ```

## Linux目录说明

| 目录             | 说明                                                         |
| :--------------- | :----------------------------------------------------------- |
| /                | Linux文件系统的入口，所有的目录、文件、设备都在/下。         |
| /bin             | Binary的缩写。常用的二进制命令目录。比如 ls cp mkdir cut;和/usr/bin类似，一些用户级gnu工具 |
| /boot            | 存放的系统启动相关的文件，例如：kernel.grub(引导装载程序)    |
| /dev             | Device的缩写。设备文件目录，比如声卡、磁盘                   |
| /etc             | 常用系统以及二进制安装包配置文件默认路径和服务器启动命令目录，比如：<br />/etc/passwd   用户信息文件<br />/etc/shadow   用户密码文件<br />/etc/group      存储用户组信息<br />/etc/fstab       系统开机启动自动挂载分区列表<br />/etc/hosts      设定用户自己的hosts文件。（IP与主机名对应的信息） |
| /home            | 普通用户的宿主目录默认存放目录                               |
| /lib             | 库文件存放目录，函数库目录                                   |
| /mnt<br />/media | /mnt和/media一般用来临时挂载临时设备的挂载目录，比如有cdrom( sr0 )、U盘等 目录 |
| /opt             | 表示可选择的意思，有些软件包也会被安装在这里。比如：gitlab   |
| /proc            | Process的缩写。操作系统运行时，进程（正在运行中的程序）信息以及内核信息（比如cpu、硬盘分区、内在信息等）。<br />/proc目录是一个伪装的文件系统proc的挂载目录。proc并不是一个真正的文件系统。<br /> 因此，这个目录 是一个虚拟的目录，它是系统内存的映射，我们可以通过访问这个目录来获取系统信息。也就是说，这个目录的内容不在硬盘上而是在内存里。 |
| /sys             | 系统目录，存放硬件信息的相关文件                             |
| /run             | 运行目录，存放的是系统运行时报数据，比如进程的PID文件        |
| /srv             | 系统本地服务文件目录                                         |
| /sbin            | 大多数涉及系统管理的命令都存放在该目录中，它是超级权限用户root的可执行命令存放地，普通用户无权限执行这个目录下的命令，凡是目录sbin中包含的命令都是root权限才能执行的。 |
| /tmp             | 该目录用于存放临时文件，有时用户运行程序的时候，会产生一些临时文件。/tmp就是用来存放临时文件的。/var/tmp目录和该目录的作用是相似的，不能存放重要数据，系统会定期删除这个目录下的没有被使用的文件 |
| /var             | Variable的缩写。系统运行和软件运行时产生的日志信息。该目录的内容是经常变动的。存放的是一些变化的文件。比如/var下有/var/log目录用来存放系统日志的目录，还有mail、/var/spool/cron |
| /usr             | 存放应用程序和文件 <br />/usr/bin     普通用户使用的应用程序<br />/usr/sbin   管理员使用的应用程序<br />/usr/lib      库文件Glibc(32位)<br />/usr/lib64  库文件Glibc |
| /lib<br />/lib64 | 这个目录是存放着系统最基本的动态链接共享库，包含许多被/bin/和/sbin/中的程序使用的库文件，目录/usr/lib/中含有更多用于用户程序的库文件。<br />libxxx.a  是静态库。  libxxx.so 是动态库<br />简单的说： 这些库是为了让你的程序能够正常编译运行<br />几乎所有的应用程序都要用到这些共享库 |



## 文件和管理

**创建文件**：

`touch`: 创建一个文件

```shell
touch a.txt   #创建一个文件a.txt
touch file1 file2 #一次性创建两个文件 file1 和 file2
touch file{3..20} #一次性创建多个文件 file3 file4 …… file20
--------------------------------------------------------------------
[root@originMachine ~]# mkdir test
[root@originMachine ~]# ls
anaconda-ks.cfg  test
[root@originMachine ~]# cd test
[root@originMachine test]# touch file{001..100}
[root@originMachine test]# ls
file001  file014  file027  file040  file053  file066  file079  file092
file002  file015  file028  file041  file054  file067  file080  file093
file003  file016  file029  file042  file055  file068  file081  file094
file004  file017  file030  file043  file056  file069  file082  file095
file005  file018  file031  file044  file057  file070  file083  file096
file006  file019  file032  file045  file058  file071  file084  file097
file007  file020  file033  file046  file059  file072  file085  file098
file008  file021  file034  file047  file060  file073  file086  file099
file009  file022  file035  file048  file061  file074  file087  file100
file010  file023  file036  file049  file062  file075  file088
file011  file024  file037  file050  file063  file076  file089
file012  file025  file038  file051  file064  file077  file090
file013  file026  file039  file052  file065  file078  file091
```

用vim和重定向创建一个文件

```shell
vim a.txt #创建一个a.txt文件并打开
echo abc > b.txt #创建一个b.txt并向里面写入'abc'
```

**创建文件夹**

`mkdir`

```shell
mkdir test
mkdir test1 test2 test2 /test/testtmp  #创建多个目录
mkdir -p /opt/test/test1/test2 #在不存在的目录下创建子目录要加参数p
```

**复制与移动**

`cp` `mv`

```shell
cp a.txt /opt/test.txt #将a.txt的内容拷贝到/opt/test.txt中
cp a.txt /opt #将a.txt拷贝一份到/opt下
cp /etc/passwd/ /opt/ #将整个目录拷贝到/opt下
cp -r /etc/passwd/ /opt/ #将整个目录（如果里面含有子目录）拷贝到/opt下
```

**查看文件**

`cat`  `more`  `head`  `tail`

```shell
more filename  #more以分页形式显示文件内容，按 Enter 刷新一行，按 Space 刷新一屏，输入q退出
less filename  #less支持前后翻滚，可以向上翻页 （pageup） 也可以向下翻页（pagedown）
head filename  #显示文件前10行
head -n 3 filename #显示文件前3行
tail filename  #显示文件后10行
tail -n 3 filename  #显示文件后3行 -f 参数是用来动态显示，常用来看日志。
```

## 格式化一块新磁盘

### 分区

```shell
[root@Omnipotent ~]# fdisk /dev/sdc
欢迎使用 fdisk (util-linux 2.23.2)。

更改将停留在内存中，直到您决定将更改写入磁盘。
使用写入命令前请三思。


命令(输入 m 获取帮助)：m
命令操作
   a   toggle a bootable flag
   b   edit bsd disklabel
   c   toggle the dos compatibility flag
   d   delete a partition
   g   create a new empty GPT partition table
   G   create an IRIX (SGI) partition table
   l   list known partition types
   m   print this menu
   n   add a new partition
   o   create a new empty DOS partition table
   p   print the partition table
   q   quit without saving changes
   s   create a new empty Sun disklabel
   t   change a partition's system id
   u   change display/entry units
   v   verify the partition table
   w   write table to disk and exit
   x   extra functionality (experts only)

命令(输入 m 获取帮助)：p

磁盘 /dev/sdc：160.0 GB, 160041885696 字节，312581808 个扇区
Units = 扇区 of 1 * 512 = 512 bytes
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节
磁盘标签类型：dos
磁盘标识符：0x0006925e

   设备 Boot      Start         End      Blocks   Id  System

命令(输入 m 获取帮助)：n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
分区号 (1-4，默认 1)：
起始 扇区 (2048-312581807，默认为 2048)：
将使用默认值 2048
Last 扇区, +扇区 or +size{K,M,G} (2048-312581807，默认为 312581807)：
将使用默认值 312581807
分区 1 已设置为 Linux 类型，大小设为 149.1 GiB

命令(输入 m 获取帮助)：w
The partition table has been altered!

Calling ioctl() to re-read partition table.
正在同步磁盘。
[root@Omnipotent ~]# ls /dev/sdc*
/dev/sdc  /dev/sdc1
```

### 格式化分区

```shell
mkfs.xfs /dev/sdc1 #把sdc1分区格式化成xfs文件格式
```



## xfs文件系统的备份与恢复

> xfs 提供了`xfsdump` 和 `xfsrestore`工具协助备份xfs文件系统中的数据。xfsdump按inode顺序备份一个xfs文件系统。
>
> centos7 开始选择xfs格式作为默认文件系统，而不再以以前的ext，仍然支持ext4,xfs专为大数据产生，每个单个文件系统最大可以支持8eb，单个文件可以支持16tb，不仅数据量大，而且扩展性高。还可以通过xfsdump,xfsrestore来备份和恢复。
>
> 与传统的unix文件系统不同，xfs不需要在备份前被卸载；对使用中的xfs文件系统做备份就可以保证镜像的一致性。xfs的备份与恢复的过程可以被中断然后继续的，无须冻结文件系统。xfsdump甚至提供了高性能的多线程备份操作——它把一次sump拆分成多个数据流，每个数据流可以发往不同的目的地
>
> xfs的备份级别有两种，默认为0（即完全备份），1～9.（增量备份）

### 备份

1，备份整个分区。（这个功能类似虚拟机的快照。）

```shell
xfsdump -f /data/xxx /dev/sdb1  #将挂载盘/dev/sdb1 下的整个分区备份到/data/xxx中
```

2，免交互备份。

```shell
xfsdump -f /data/dump-1st /dev/sdb1 -L dump-1st -M sdb1-data #-L(lable,为备件写个标签)  -M (media,为源文件写个备注)
```

3，指定只备份分区中某个文件夹或文件

```shell
xfsdump -f /data/dump-file-xxx -s file /dev/sdb1 -L dump-file -M file-dev-sdb1 # 备份/dev/sdb1/file 到 /data/dump-file-xxx中
xfsdump -f <target directory> -s <filename> <origin directory> -L <label> -M <mediaName> # 格式 -s 后的文件名不能用绝对路径，因为它相对于后面的origin路径。
```

### 恢复

1，先将整个备份恢复到分区

```shell
xfsrestore -f /data/dump-1st /dev/sdb1 # 将备件文件dump-1st 恢复到/dev/sdb1
```

2，再恢复单个文件或目录

```shell
xfsrestore -f /data/dump-file-xxx -s file /var/ #将备件文件恢复到/var/file目录中
```



> 使用 xfsdump 时，请注意下面下面的几个限制：
> 1、xfsdump 不支持没有挂载的文件系统备份！所以只能备份已挂载的！
> 2、xfsdump 必须使用 root 的权限才能操作 (涉及文件系统的关系)
> 3、xfsdump 只能备份 XFS 文件系统
> 4、xfsdump 备份下来的数据 (档案或储存媒体) 只能让 xfsrestore 解析
> 5、xfsdump 是透过文件系统的 UUID 来分辨各个备份档的，因此不能备份两个具有相同 UUID 的文件系统

### 增量备份

> **增量备份是指在一次全备份或上一次增量备份后，以后每次的备份只需备份与前一次相比增加或者被修改的文件。这就意味着，第一次增量备份的对象是进行全备后所产生的增加和修改的文件；第二次增量备份的对象是进行第一次增量备份后所产生的增加和修改的文件，以此类推。**
>
> **优缺点**
>
> **优点：没有重复的备份数据，因此备份的数据量不大，备份所需的时间很短。**
>
> **缺点：数据恢复相对比较麻烦，它需要上一次全备份和所有增量备份的内容才能够完全恢复成功，并且它们必须沿着从全备份到依次增量备份的时间顺序逐个反推恢复，因此可能会延长的恢复时间**

操作：

​	第一次全备

```shell
xfsdump -f /opt/test-full /sdb1 -L test-full -M media0
```

​	进行一次1 级备份

```shell
xfsdump -l 1 -f /opt/test-back1 /sdb1 -L test-bak1 -M media0
```

​	进行一次2级备份

```shell
xfsdump -l 2 -f /opt/test-back2 /sdb1 -L test-bak2 -M media0
```

**要想恢复全部全部数据，包括新添加的文件的操作**

1、先恢复完全备份。   

2、情况1: 恢复最后一次增量备份（如果两次增量备份都是1级的，所以只需要恢复最后一个增量就可以了）。

3、情况2：如果做的是第一次是1级备，第二次是2级备，那么在恢复的时候就需要先恢复完全备份，然后是1级备，最后是2级备（只有按备份的顺序恢复才能得到完整的数据）

## vi和vim编辑操作

vim和vi不是同一个软件包。vim是vi的增强版，最明显的区别是vim可以语法加亮，它完全兼容vi

- vim 编辑器的4种操作模式：

  1，正常模式（Normal mode) 

  2，命令行模式(Command-line mode)

  3，插入模式 （Insert mode)

  4，可视模式 （Visual mode)

- vim 编辑两个文件

  ```shell
  vim -O filename1 filename2 # 横向同时打开两个文件
  vim -o filename1 filename2 # 纵向同时打开两个文件
  ```

  在两个打开的文档中切换：ctrl + w 两次
  
  对比两个文件： vimdiff filename1 filename2

## 正常模式

从正常模式进入插入模式的6种方法 a i o A I O

```shell
vim + 23 filename #打开文件，并进入第23行。   
```

> i 在当前字符前插入
>
> I 在行首插入
>
> a 在当前字符后插入
>
> A 在行尾插入
>
> o 下一行插入
>
> O 上一行插入
>
> x 向后删除一个字符
>
> X 向前删除一个字符

在正常模式中的操作：

> u 撤销一步，每按一次就撤销一次
>
> ctrl + r 恢复，每按一次就恢复一次
>
> r 替换

在正常模式中的移动：

> 0 和 home 都是切换到行首
>
> $ 和 end 都是切换到行尾
>
> gg快速定位到文档的首行，
>
> G快速定位到末行。
>
> /string 找到匹配的字符串，如果匹配的内容较多，可以用N、n来进行向上与向下的查找。并且vim会对匹配的内容进行高亮显示，取消高亮用:noh 或 输入一个不存在的内容。
>
> /^d  查找以a开头的字符串，正则匹配规则
>
> /f$   查找以f结尾的字符串，正则匹配规则

在正常模式中对文件进行编辑：

> yy 复制整行 ，复制多行：Nyy
>
> dd 删除整行，删除多行：Ndd
>
> p 粘贴。
>
> 剪切，就是删除  => dd

Visual Mode 可视块模式：

> V 进入列编辑模式 

清空所有内容：

> ：%d 

## 安装文件传输 vsftpd

```shell
yum install vsftpd -y
firewall-cmd --zone=public --add-port=21/tcp --permanent #永久开启21端口
firewall-cmd --reload #重启防火墙
vi /etc/vsftpd/vsftpd.conf # 将anonymous_enable=YES 改成 NO
firewall-cmd --reload #重启防火墙
useradd [ftpusername] #创建的帐户如果没有设置目录，默认在 /home下
passwd [ftpusername]
#这时可以登录，但不能上传文件 用 getsebool -a | grep ftp 查看是selinux没有开启权限
setsebool -P ftpd_full_access on && setsebool -P tftp_home_dir on #开启selinux的权限
```

## 安装mariaDB

由于centos 7.5的yum默认源中的mariadb的版本是5.5。如果我们的后端的代码里需要高版本的mariadb，我们要修改yum源。

mariadb的yum源地址配置方法： https://downloads.mariadb.org/mariadb/repositories/#distro=CentOS&distro_release=centos7-amd64--centos7&mirror=tuna&version=10.5

方法是在`/etc/yum.repos.d/`下新建一个文件`MariaDB.repo`然后把在上述地址中选择的配置文件粘贴在里面

```
# MariaDB 10.5 CentOS repository list - created 2021-01-14 18:12 UTC
# http://downloads.mariadb.org/mariadb/repositories/
[mariadb]
name = MariaDB
baseurl = http://yum.mariadb.org/10.5/centos7-amd64
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck=1
```



```shell
cd /etc/yum.repos.d/
touch MariaDB.repo
vi !$ #把内容粘贴在里面
```

```shell
yum install mariadb mariadb-server -y
systemctl start mariadb && systemctl enable mariadb
mysql_secure_installation #去除安全隐患
```

我们发现如果我们是用root登录我们的Linux的话，我们进入mariadb是不需要密码的。因为从mariadb 10.5.4版本开始就是用了Unix-Socket(大多数人或教科书把Socket翻译成`套接字`，没文化害死人啊……socket的英文本意就是`插座`的意思，汉字本来就是表意的，非得弄个似是而非的东西。这也就是我们一定要读英文文档的原因，这个原因mariadb的作者在文档里说明了：[mariadb yum repo](https://mariadb.com/kb/en/authentication-plugin-unix-socket/)，我们要在`/etc/my.cnf`这个配置文档里修改一下，修改方式有两个：

```shell
[mariadb]
unix_socket=OFF #把unix认证连接给关闭就行了，不再用linux的认证权限来认证mariadb
----------------------------
[mariadb]
disable_unix_socket #这种方式也可以。两种选一个自己喜欢的。修改完配置记得重启mariadb
systemctl restart mariadb
```

按照步骤完成安全设置后进入mysql,查看一下字符集设置，因为它用的是拉丁文我们要改成包含我们汉字的uft8或uft8mb4（它比uft8多了emoji表情）。

```mysql
MariaDB [(none)]> show variables like 'character_set%';
+--------------------------+------------------------------+
| Variable_name            | Value                        |
+--------------------------+------------------------------+
| character_set_client     | utf8                         |
| character_set_connection | utf8                         |
| character_set_database   | latin1                       |
| character_set_filesystem | binary                       |
| character_set_results    | utf8                         |
| character_set_server     | latin1                       |
| character_set_system     | utf8                         |
| character_sets_dir       | /usr/share/mariadb/charsets/ |
+--------------------------+------------------------------+
```

发现字符集设置为拉丁文(latin1)，我们要将它设置成`utf8mb4`,`utf8mb4`比`utf8`多了emoji表情字符

方法是在`/etc/my.cnf`里加入一行：

```shell
[mariadb]
character_set_server = utf8mb4
```

最后进入数据库查看就会得到心满意足的效果了：

```mariadb
MariaDB [(none)]> show variables like 'character_set%';
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | utf8                       |
| character_set_connection | utf8                       |
| character_set_database   | utf8mb4                    |
| character_set_filesystem | binary                     |
| character_set_results    | utf8                       |
| character_set_server     | utf8mb4                    |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
```

## 安装Node.js

方法1 直接用yum 源安装

```shell
curl --silent --location https://rpm.nodesource.com/setup_14.x | bash -
yum install -y nodejs
node -v
curl -sL https://dl.yarnpkg.com/rpm/yarn.repo | sudo tee /etc/yum.repos.d/yarn.repo | sudo yum install yarn -y
```

方法2 用snap 安装，自动带yarn包管理器,这种方法是安装epel源，从epel源里安装snap，然后安装Nodejs。但安装完snap后需要重启。这种方法安装后同时有yarn。这样不管是npm还是yarn两种包管理器都有了。

```shell
yum install epel-release
yum install snapd
systemctl enable --now snapd.socket
ln -s /var/lib/snapd/snap /snap
reboot
snap install node --channel=14/stable --classic
```

## 打开80端口

```shell
firewall-cmd --zone=public --add-port=80/tcp --permanent #开启80端口
firewall-cmd --zone=public --add-port=3306/tcp --permanent #开启mysql默认的3306端口 （如果在mysql配置里允许非root用户使用时要开启）
firewall-cmd --reload
```

**查看防火墙状态**

```shell
1  | systemctl status firewalld
2  | firewall-cmd --state
```

**关闭80端口**

```shell 
firewall-cmd --zone=public --remove-port=80/tcp --permanent
```

**查看是否开启80端口**

```shell
firewall-cmd --zone=public --query-port=80/tcp
```

**查看所以已经开放的端口**

```shell
firewall-cmd --list-ports
```

## 安装Docker

- ```shell
  yum remove docker docker-client docker-client-latest docker-common \
                    docker-latest \
                    docker-latest-logrotate \
                    docker-logrotate \
                    docker-engine #卸载旧版本
  yum install -y yum-utils        #安装yum工具
  yum-config-manager --add-repo \
      https://download.docker.com/linux/centos/docker-ce.repo #安装yum源，在国外，建议用阿里云的源 http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
  yum install docker-ce docker-ce-cli containerd.io -y #安装docker
  systemctl start docker #启动docker
  docker run hello-world  #运行hello-world映像来验证是否正确安装Docker,此命令下载测试镜像并在容器中运行它。容器运行时，它会打印参考消息并退出
  ```

  

## 卸载Docker

1. 卸载Docker Engine，CLI和Containerd软件包：

   ```shell
   $ sudo yum remove docker-ce docker-ce-cli containerd.io
   ```

2. 主机上的映像，容器，卷或自定义配置文件不会自动删除。要删除所有图像，容器和卷：

   ```shell
   $ sudo rm -rf /var/lib/docker
   ```



-----------------

## 要yum 安装的软件

```shell
yum install -y vsftpd tree vim nodejs yarn mariadb-server git
yarn global add pm2
```







支付宝返回结果：

```
{
  gmt_create: '2021-01-15 23:35:16',
  charset: 'utf-8',
  gmt_payment: '2021-01-15 23:35:23',
  notify_time: '2021-01-15 23:35:24',
  subject: '季会员订单',
  sign: 'TG6ZXVA2J+CYt7RYkKd8F3ZEzFvNjlFBxLpineIoQVbCj53iE4JzW6byw2o7gN1BRveAcqCAJvCnpPA4CHqDqNjrpIDbrjBfm8NbXDvzvrAbDB/Vt95Zyy2dKlEv7mkM/BNVqDyelZG9xMVRy55egg7S44S6f/t24vDBGuDyJ5Y=',
  buyer_id: '2088622955135011',
  body: '公司: test1 ',
  invoice_amount: '180.00',
  version: '1.0',
  notify_id: '2021011500222233523035010509389090',
  fund_bill_list: '[{"amount":"180.00","fundChannel":"ALIPAYACCOUNT"}]',
  notify_type: 'trade_status_sync',
  out_trade_no: 'm1_1610724907269',
  total_amount: '180.00',
  trade_status: 'TRADE_SUCCESS',
  trade_no: '2021011522001435010500901242',
  auth_app_id: '2021000116662484',
  receipt_amount: '180.00',
  point_amount: '0.00',
  app_id: '2021000116662484',
  buyer_pay_amount: '180.00',
  sign_type: 'RSA',
  seller_id: '2088621954789584'
}
```

沙箱帐号

```
kgnjmj2132@sandbox.com
```























