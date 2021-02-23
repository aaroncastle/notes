# Terminal

- mkdir ('make directory') 当前文件夹中创建新的文件夹，（文件夹就是路径）

  ``` mkdir photos```

  `mkdir -p /test/good` -p可以创建多级目录

- pwd (print working directory) 当前全路径信息

- cd (change directory) 改变路径 

- ls ( list ) ls -a ( all ) 展示所有文件（包含隐藏文件） ls -l ( longer ) 代表展示更多文件的信息 

- touch 在当前路径下创建一个文件类型 如果文件存在，表示修改当前文件时间

- mv ( move ) 两个作用：移动；重命名

  ``` mv oldName newName ```

  ``` mv fileName ./directory ```

- rm ( remove ) 删除文件，直接删除不会出现在垃圾站

  ``` rm fileName ```

  rm -r ( recursive 递归) 删除文件夹 或 rm -f ( force )强制删除文件夹

  rmdir directory/  只能删除空的文件夹

- `cp` (copy) 复件文件

  `cp test.txt /root/home` 将text.txt复制到/root/home一份

- useradd 创建用户，`useradd Aaron ` 创建一个Aaron 的用户，`userdel aaron` 删除aaron这个用户

- groupadd 创建用户组，` groupadd work` 创建一个work的用户组，` groupdel play` 删除play这个用户组

- find 查找文件或目录，用法  `find /home -name 'test.txt'` `find /home -name '*.txt'` 

- cat 查看一个文件的内容

- vi 用vim编辑器修改某个文件

- echo 打印输出。 echo ok >test.txt 把ok覆盖掉test.txt的内容。echo ok >> text.txt 向test.txt文件追加ok内容。

- chown （change own） 更改某个文件的所有者或所属的组 ` chown -R arron:Aaron text.txt` 

- chmod (change mode) 修改文件权限 

  ` - 表示文件`

  `d 表示目录`

  `r 读的权限（4）`

  `w 写的权限（2）`

  `x 执行的权限（1）`

  ` chmod u=rw g=r o=r test.txt` 将test.txt文件的权限修改为用户可读写，用户组可读，其他用户可读

- `ifup eth0` 启动网卡

  ```shell	
  cd /etc/sysconfig/network-scripts/
  vi ifcfg-eth0
   DEVICE=eth0
   HWADDR=00:0C:29:12:3D:30
   TYPE=Ethernet
   UUID=1214d43b-a483-43d2-9ee4-af7fc84ce41b
   ONBOOT=no #开机或重启网络默认开启状态
   NM_COMTROLLED=yes
   BOOTPROTO=dhcp #动态ip 可以修改成static如果换成静态要配置以下配置
   
   IPADDR=192.168.0.xxx
   NETMASK=255.255.255.0
   GATEWAY=192.168.0.1
   
  /etc/init.d/network restart #重启服务
   vi /etc/resolv.conf #配置DNS
   nameserver 202.106.0.20 #网通DNS google DNS 8.8.8.8
  ```

  

- 如果没有`ifconfig`命令，需要安装net-tools，但没有网卡无法yum安装，所以要先配置

  `vi /etc/sysconfig/network-scripts/ifcfg-enp0s3` 将

  ```shell 
  ONBOOT=NO#改成yes
  # service network restart 重启网络服务,此时已经可以上网，可以ping www.taobao.com测试一下
  #然后 sudo yum installl net-tools
  ```

  -----

- 开启sshd服务 ` vi /etc/ssh/sshd_config/` 

- 重启sshd服务 ` systemctl restart sshd`

- 生成SSH-KEY

  ```shell
  ssh-keygen -t rsa -C 'your@email.com'
  ```

- 添加认证key

  ```shell
  cd ~
  cd .ssh
  touch authorized_keys
  ```

- 复制你电脑上的`~/.ssh/id_rsa.pub`里的内容到 authorized_keys里。

  -----

- 查看防火墙状态

  `firewall-cmd --state`

- 关闭防火墙

  `systemctl stop firewalld.service`

- 安装vsftp

  `yum install vsftpd* -y`

  重启vsftp服务

  `/etc/init.d/vsftpd restart` 此时用户可以匿名登录ftp服务器

  为防止匿名登录可以加权限：

  1，在系统下开一个用户，给密码，用户用系统开的用户帐号登录ftp（记得关闭防火墙与SElinux）

  ```shell 
  useradd xxx
  passwd xxx
  #查看防火墙状态 firewall-cmd --state
  #关闭防火墙 systemctl stop firewalld.service
  ```

  2，用虚拟用户(vsftpd) virtual user ftp daemon登录，这样用户就无法使用系统用户的权限

  ```shell 
  #安装虚拟用户软件与认证模块
  yum install pam* db4* --skip-broken -y
  #创建并生成vsftpd数据库文件内容 vi /etc/vsftpd/ftpusers.txt 
  [username]
  [passwd]
  [username]
  [passwd]
  ...
  #生成数据库
  db_load -T -t hash -f /etc/vsftpd/ftpusers.txt /etc/vsftpd/vsftpd_login.db
  #修改文件权限
  chmod 700 /etc/vsftpd/vsftpd_login.db
  #配置PAM验证文件 vi /etc/pam.d/vsftpd
  #在配置文件行首加入认证：（如果是32位，lib64改成lib，如果是RedHat,加入的语句不同）
  auth sufficient /lib64/security/pam_userdb.so db=/etc/vsftpd/vsftpd_login
  account sufficient /lib64/security/pam_userdb.so db=/etc/vsftpd/vsftpd_login
  #创建vsftpd映射本地用户
  #所有的FTP虚拟用户都需要使用一个系统用户，这个系统用户不需要密码，也不需要登录，只用来须知虚拟用户映射使用
  useradd -d/home/ftpuser -s/sbin/nologin ftpuser
  #修改/etc/vsftpd/vsftpd.conf配置文件：
  anonymous_enable=NO
  local_enable=YES
  write_enable=YES
  local_umask=022
  dirmessage_enable=YES
  xferlog_enable=YES
  connect_from_port_20=YES
  xferlog_file=/var/log/vsftpd.log
  xferlog_std_format=YES
  ascii_upload_enable=YES
  ascii_download_enable=YES
  listen=YES
  
  guest_enable=YES
  guest_username=ftpuser
  pam_service_name=vsftpd
  user_config_dir=/etc/vsftpd/vsfptd_user_conf
  virtual_use_local_privs=YES
  ```

- 查看占用的80端口

- ```shell
  netstat -ntulp | grep 80
  ```

  







# Linux 系统目录（centOS)

- / 

  - **/bin 存放必要的命令**
  - **/boot 内核以及启动所需的文件**
  - **/dev 设备文件**
  - **/etc 系统配置文件**
  - **/home 普通用户的宿主目录**
  - **/lib 必要的运行库**
  - **/mnt 临时的映射文件系统，通常用来存放挂载使用**
  - **/opt **
  - **/proc 存放存储进程和系统信息**
  - **/root 超级用户主目录**
  - **/sbin 系统管理程序**
  - **/srv **
  - **/sys **
  - **/tmp 临时文件**
  - **/usr 应用程序，命令程序文件、程序库、手册和其他文档**
  - **/var 系统默认日志存放目录**

  

### 命令

- 查看CPU

  > cat /proc/cpuinfo

- 查看内存

  > cat /proc/meminfo

- 安装node

  > 因为开发时的Node版本都很高，14.x以上而centos最高可以用yum安装node的版本是8，所以要先安装高版本的node，为了更方便的安装高于8.0版本的Node，我们用snap商店来安装。所以我们要先安装snap。在安装snap之前要先安装EPEL存储库
  >
  > ` sudo yum install epel-release`
  >
  > 然后就可以安装snap：
  >
  >  `sudo yum install snapd` 
  >
  > `sudo systemctl enable --now snapd.socket`
  >
  > `sudo ln -s /var/lib/snapd/snap /snap`
  >
  > 现在重启 `reboot`
  >
  > 现在可以安装Node了，
  >
  > `sudo snap install node --channel=14/stable --classic` 更改channel的值来安装不同版本的node（只支持整数），安装的都是稳定版本。
  >
  > 现在就有Node了，并且同时拥有了npm 和 yarn 包管理器。

- 安装mysql

  > 1，安装mysql之前要先安装mysql源，我们先下载源安装包。源安装包在`https:/mysql.com/downloads/repo/yum/`获取。
  >
  > 上面对应了不同的系统不同的版本的源地址，我们下载8.0版本的mysql
  >
  > `wget https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm`
  >
  > --------
  >
  > (如果要安装5.7版本的mysql就要下载对应的源，一般大厂都用的5.7版本的多。地址是` wget https://dev.mysql.com/get.mysql57-community-release-el7-11.noarch.rpm`)
  >
  > ------
  >
  > 2，安装mysql源到本地
  >
  > `yum localinstall mysql80-community-release-el7-3.noarch.rpm`
  >
  > ​	我们可以检查一下mysql源的安装情况，以便下面安装mysql:
  >
  > `yum repolist enabled | grep 'mysql.*-community.*'`
  >
  > 3，安装mysql：
  >
  > `yum install mysql-community-server -y`
  >
  > 4，启动mysql:
  >
  > `systemctl start mysqld`
  >
  > 5，查看mysql状态:
  >
  > `systemctl status mysqld`
  >
  > 6，设置开机启动：
  >
  > `systemctl enable mysqld`  #设置开机启动
  >
  > `systemctl daemon-reload` #设置守护进程（掉线后自动再启动）
  >
  > 7，配置mysql (密码以及默认编码)
  >
  > 因为我们是安装的服务器版本的mysql，和客户端的安装不同，客户端的安装会让输入一个密码。服务端是给一个临时密码，然后自动去改。
  >
  > mysql的密码策略(等级)有三种：low，medium，strong
  >
  > 我们可以登录mysql后用sql命令`show variables like '%password%'`查看。
  >
  > ​    我们可以在 `/etc/my.cnf`文件中来修改。具体修改方式：
  >
  > ```vim
  > # 选择 0（LOW）, 1（MEDIUM）,2（STRONG）。如果选择2需要提供密码字典文件
  > validate_password_policy = 0
  > 
  > #如果不需要密码策略（这是我的选择）,添加配置禁用策略：
  > validate_password = off
  > ```
  >
  > 重新启动mysql服务使配置生效：
  >
  > `systemctl restart mysqld`
  >
  > 使用root和默认密码登录mysql,我们需要先知道默认密码。
  >
  > ```shell
  > grep 'temporary password' /var/log/mysqld.log
  > ```
  >
  > 显示的信息最后`root@localhost:`后面的字符串就是默认密码。
  >
  > 用root帐户和默认密码登录mysql：
  >
  > ```shell
  > mysql -u root -p
  > ```
  >
  > 然后输入密码就进入了mysql  会有提示符`mysql>`
  >
  > 8，修改root密码：
  >
  > 这个临时密码必须要修改，不然不可以进行操作。可以通过以下两种方式修改：
  >
  > ```mysql
  > mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY '[你自己的密码]';  # mysql5.7的写法
  > mysql> alter user 'root'@'localhost' identified with mysql_native_password by '[你的密码]'; # myslq 8.0的写法 
  > ```
  >
  > ```mysql
  > mysql> set password for 'root'@'localhost' = password('你自己的密码');
  > ```
  >
  > 注意：mysql语句中必须以英文分号`;`结尾。
  >
  > 如果你无法修改，那是因为密码策略造成的。mysql 5.7版的在第7步 `/etc/my.cnf`文件中关闭密码策略或将策略设成0（low）级别。这样就可以把密码设置的不那么复杂了。当时如果不改配置，可以把你的密码修改成大小写加特殊符号的高强度密码（符合对应密码策略的级别的密码）也可以。
  >
  > 重点说下8.0版：
  >
  > 8.0版在mysql中更改策略：
  >
  > ```mysql
  > mysql> set global validate_password.policy = 0;
  > mysql> set global validate_password.length = 4;
  > mysql> exit
  > ```
  >
  > 退出mysql后，执行：
  >
  > ```shell
  > mysql_secure_installation
  > ```
  >
  > 效果：
  >
  > ```shell
  > Securing the MySQL server deployment.  #执行mysql服务安全部署
  > 
  > Enter password for user root:   #输入你刚才的那个临时密码
  > 
  > The existing password for the user account root has expired. Please set a new password.  #用户root存在的密码已过期，请输入一个新密码
  > 
  > New password:  #输入你要设置的新密码
  > 
  > Re-enter new password: #再输入一次
  > 
  > The 'validate_password' component is installed on the server.
  > The subsequent steps will run with the existing configuration of the component.
  > Using existing password for root.
  > 
  > Estimated strength of the password: 100
  > Change the password for root?(press y | Y for Yes, any other key for No): y
  > 
  > New password: 
  > 
  > Re-enter new password: 
  > 
  > Estimated strength of the password: 50
  > Do you wish to continue with the password provided? (Press y | Y for Yes, any other key for No): y
  > 
  > By default, a MySQL installation has an anonymous user,allowing anyone to log into MySQL without having to have a user account created for them. This is intended only for testing, and to make the installation go a bit smoother.You should remove them before moving into a production environment.
  > 
  > Remove anonymous users? (Press y|Y for Yes, any other key for No) : y
  > 
  > Success.
  > 
  > Normally, root should only be allowed to connect from 'localhost'. This ensures that someone cannot guess at the root password from the network.
  > 
  > Disallow root login remotely? (Press y|Y for Yes, any other key for No) : n
  > 
  > 
  >  ... skipping.
  > 
  > By default, MySQL comes with a database named 'test' that anyone can access. This is also intended only for testing,and should be removed before moving into a production environment.
  > 
  > Remove test database and access to it? (Press y|Y for Yes, any other key for No) : y
  > 
  >  - Dropping test database...
  > 
  > Success.
  > 
  > 
  > 
  >  - Removing privileges on test database...
  > 
  > Success.
  > 
  > 
  > 
  > Reloading the privilege tables will ensure that all changes
  > 
  > made so far will take effect immediately.
  > 
  > 
  > 
  > Reload privilege tables now? (Press y|Y for Yes, any other key for No) : y
  > 
  > Success.
  > 
  > 
  > 
  > All done! 
  > 
  > )
  > ```
  >
  > 
  >
  > 9，配置默认编码：
  >
  > 8.0版本的mysql默认编码就是utf8mb4,所以这一步可以省略了。
  >
  > 5.7版本的mysql的编码是拉丁文。我们要改成utf8mb4(至于为什么是utf8m4而不是utf8，是因为汉字的有些字占的字符不是两位，同时也加入了emoj表情)
  >
  > 登录mysql后用命令查看默认编码格式：
  >
  > ```mysql
  > mysql> show varialbes like '%character%';
  > ```
  >
  > 如果是拉丁就要修改。通过修改`/etc/my.cnf`文件下的`[myslqd]`编码配置，如下：
  >
  > > [mysqld]
  > >
  > > character_set_server = utf8mb4
  > >
  > > character_set_system = utf8mb4
  >
  > 重启服务使之生效：
  >
  > ```shell
  > systemctl restart mysqld
  > ```
  >
  > ----
  >
  > 完结撒花～～！

  

- 使用node开启网站

  经过一番折腾，终于可以用node开启自己的网站了。从github上拉取你的代码，用node启动吧，别忘了启动前要先安装依赖，`npm install` 或 `yarn install` (如果是用yarn管理器的话，只输入`yarn`就会自动运行yarn install )

  ```shell
  yum install git -y                   # 下载安装git
  cd ~                                 # 进入宿主文件夹，我们就把代码克隆到这里吧，用nginx反射代理的同学放在nginx配置的路径下就可以了
  git clone https://github.com/aaroncastle/xxxx.git server #把代码克隆进server文件夹内，如果不加server，会克隆出和你的项目一样的文件夹
  cd server                            # 进入server
  yarn                                 # 安装依赖，可以用npm install 或 yarn install(直接输入yarn就会自动运行yarn install)
  node index.js &                      # 运行你的入口文件，最后一个参数 & 的意思是在后台运行，不占用你的shell窗口了。
  ```

  如果是用一台电脑自己装个centos或虚拟机运行的话，后面的不用看了。如果你买了阿里的云服务器，你可能会发现当你关闭shell连接后你的网站会挂掉。意不意外？惊不惊喜？如果这时候你正在给仰慕你的同学花哨（骚气）地展示你开启服务器的潇洒时，你可能就尴尬了……往下看↓

  > 阿里云的服务器有个连接时间。如果长时候不操作，会退出22端口的连接而不会影响其他进程。所以你不必对shell做任何操作，到了时间你也无法操作shell，这时候你可以输入任意字符，如果发现无法输入，关闭shell即可。
  >
  > 如果一定要自动关闭shell窗口才能完成你收剑入鞘的潇洒流程，那在上一步后台开启node后，回车。进入shell提示符后，输入`exit` 退出ssh连接就可以了。
  >
  > 在虚拟机可物理机装centos的用户去打开项目地址后，会发现被服务器拒绝访问。那是因为防火墙没有开启80端口或443端口。(阿里云在安全策略组中开启80和443端口)
  >
  > 两种方法：
  >
  > 1，**关闭防火墙**（不推荐）
  >
  > ```shell
  > systemctl stop firewalld.service  #关闭防火墙
  > systemctl disable firewalld.service  #禁止防火墙开机启动
  > ```
  >
  > 2，**打开80端口**（强烈推荐）
  >
  > ```shell
  > firewall-cmd --zone=public --add-port=80/tcp --permanent #开启80端口
  > firewall-cmd --zone=public --add-port=3306/tcp --permanent #开启mysql默认的3306端口 （如果在mysql配置里允许非root用户使用时要开启）
  > ```
  >
  > 3，命令含义：
  >
  > > –zone #作用域
  > > –add-port=80/tcp #添加端口，格式为：端口/通讯协议
  > > –permanent #永久生效，没有此参数重启后失效
  >
  > 4，**查看防火墙状态**
  >
  > ```shell
  > 1  | systemctl status firewalld
  > 2  | firewall-cmd --state
  > ```
  >
  > **关闭80端口**
  >
  > ```shell 
  > firewall-cmd --zone=public --remove-port=80/tcp --permanent
  > ```
  >
  > **查看是否开启80端口**
  >
  > ```shell
  > firewall-cmd --zone=public --query-port=80/tcp
  > ```
  >
  > **查看所以已经开放的端口**
  >
  > ```shell
  > firewall-cmd --list-ports
  > ```
  >
  > <font color=red size=5>最后别忘了</font>
  >
  > **重启防火墙**
  >
  > ```shell 
  > firewall-cmd --reload
  > ```

  当然，这都不是重点，重点是要引出一个强大的nodejs 进程管理器===> **PM2** (Process Manager)：[官网](https://pm2.keymetrics.io/)

  什么是PM2？

  ​	官网的定义是：PM2是一个可以让你的应用7 * 24都持续在线的进程管理守护。换句话说就是它可以让你的项目开机自启动且永不掉线。

  它能做什么？

  - [行为配置](https://pm2.keymetrics.io/docs/usage/application-declaration/)
  - [支持source-map](https://pm2.keymetrics.io/docs/usage/source-map-support)
  - [容器整合](https://pm2.keymetrics.io/docs/usage/docker-pm2-nodejs/)
  - [监控并重新加载](https://pm2.keymetrics.io/docs/usage/watch-and-restart/)
  - [日志管理](https://pm2.keymetrics.io/docs/usage/log-management/)
  - [监控方式](https://pm2.keymetrics.io/docs/usage/monitoring/)
  - [模块系统](https://pm2.keymetrics.io/docs/advanced/pm2-module-system/)
  - [最大内存重载](https://pm2.keymetrics.io/docs/usage/monitoring/#max-memory-restart)

  - [集群模式](https://pm2.keymetrics.io/docs/usage/cluster-mode/)
  - [热更新](https://pm2.keymetrics.io/docs/usage/cluster-mode/#reload-without-downtime)
  - [开发流程](https://pm2.keymetrics.io/docs/usage/pm2-development/)
  - [启动脚本](https://pm2.keymetrics.io/docs/usage/startup/)
  - [部署工作流程](https://pm2.keymetrics.io/docs/usage/deployment/)

