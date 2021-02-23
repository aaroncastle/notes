# Mysql安装

- 下载后，进入terminal，输入命令mysql，查看有没有mysql命令，如果没有命令，要安装环境变量
- mac安装环境变量
  - ```vi ~/.zshrc```
  - ``` export PATH=${PATH}:/usr/local/mysql/bin```
  - ```source ~/.zshrc```

- 检查当前数据库字符编码

  - ```show variables like 'character\_set\_%';```

  - 如果是下面的这种，则是我们想要的utf-8编码。

  - ```
    +--------------------------+---------+
    | Variable_name            | Value   |
    +--------------------------+---------+
    | character_set_client     | utf8mb4 |
    | character_set_connection | utf8mb4 |
    | character_set_database   | utf8mb4 |
    | character_set_filesystem | binary  |
    | character_set_results    | utf8mb4 |
    | character_set_server     | utf8mb4 |
    | character_set_system     | utf8    |
    +--------------------------+---------+
    7 rows in set (0.00 sec)
    ```

  - 如果不是，则要修改`my.ini`文件里的 `character`字段设置成 `utf8mb4`,重新启动mysql `service mysql restart`

- 数据库字段类型

  - `bit`占一位，0或1，false/true;
  - `int` 占32位，整数；
  - `decimal(M,N)` 精确计算的实数，M是总的数字位数，N是小数位数；
  - `char(n)` 固定长度位n的字符；
  - `varchar(n)`长度可变，最大长度为n的字符；
  - `text` 大量的字符；
  - `date` 仅日期；
  - `datetime`日期和时间；
  - `time`仅时间；

# mysql驱动程序

- mysql2 <https://github.com/sidorares/node-mysql2> 
  - `yarn add mysql2`
- ORM Sequelize
  - <https://github.com/demopark/sequelize-docs-Zh-CN>

# Mac 安装 mariadb

```shell
brew install mariadb
mysql_install_db
# 如果没有出现下面的内容去 /usr/local/var/mysql 下找到 yourcomputername.lan.err 打印一下日志
Installing MariaDB/MySQL system tables in '/usr/local/var/mysql' ...
OK

To start mysqld at boot time you have to copy
support-files/mysql.server to the right place for your system


Two all-privilege accounts were created.
One is root@localhost, it has no password, but you need to
be system 'root' user to connect. Use, for example, sudo mysql
The second is aaron@localhost, it has no password either, but
you need to be the system 'aaron' user to connect.
After connecting you can set the password, if you would need to be
able to connect as any of these users with a password and without sudo

See the MariaDB Knowledgebase at https://mariadb.com/kb or the
MySQL manual for more instructions.

You can start the MariaDB daemon with:
cd '/usr/local/Cellar/mariadb/10.5.8' ; /usr/local/Cellar/mariadb/10.5.8/bin/mysqld_safe --datadir='/usr/local/var/mysql'

You can test the MariaDB daemon with mysql-test-run.pl
cd '/usr/local/Cellar/mariadb/10.5.8/mysql-test' ; perl mysql-test-run.pl

Please report any problems at https://mariadb.org/jira

The latest information about MariaDB is available at https://mariadb.org/.
You can find additional information about the MySQL part at:
https://dev.mysql.com
Consider joining MariaDB's strong and vibrant community:
https://mariadb.org/get-involved/
```

```shell
more iMac.lan.err 
2021-01-04  5:47:09 0 [Warning] InnoDB: innodb_open_files 300 should not be greater than the open_files_limit 256
2021-01-04  5:47:09 0 [Note] InnoDB: The first innodb_system data file 'ibdata1' did not exist. A new tablespace will be created!
2021-01-04  5:47:09 0 [Note] InnoDB: Uses event mutexes
2021-01-04  5:47:09 0 [Note] InnoDB: Compressed tables use zlib 1.2.11
2021-01-04  5:47:09 0 [Note] InnoDB: Number of pools: 1
2021-01-04  5:47:09 0 [Note] InnoDB: Using crc32 + pclmulqdq instructions
2021-01-04  5:47:09 0 [Note] InnoDB: Initializing buffer pool, total size = 134217728, chunk size = 134217728
2021-01-04  5:47:09 0 [Note] InnoDB: Completed initialization of buffer pool
2021-01-04  5:47:09 0 [Note] InnoDB: Setting file './ibdata1' size to 12 MB. Physically writing the file full; Please wait ...
2021-01-04  5:47:09 0 [Note] InnoDB: File './ibdata1' size is now 12 MB.
2021-01-04  5:47:09 0 [Note] InnoDB: Setting log file ./ib_logfile101 size to 100663296 bytes
2021-01-04  5:47:09 0 [Note] InnoDB: Renaming log file ./ib_logfile101 to ./ib_logfile0
2021-01-04  5:47:09 0 [Note] InnoDB: New log file created, LSN=10329
2021-01-04  5:47:09 0 [Note] InnoDB: Doublewrite buffer not found: creating new
2021-01-04  5:47:09 0 [Note] InnoDB: Doublewrite buffer created
2021-01-04  5:47:09 0 [Note] InnoDB: 128 rollback segments are active.
2021-01-04  5:47:09 0 [Note] InnoDB: Creating foreign key constraint system tables.
2021-01-04  5:47:09 0 [Note] InnoDB: Creating tablespace and datafile system tables.
2021-01-04  5:47:09 0 [Note] InnoDB: Creating sys_virtual system tables.
2021-01-04  5:47:09 0 [Note] InnoDB: Creating shared tablespace for temporary tables
2021-01-04  5:47:09 0 [Note] InnoDB: Setting file './ibtmp1' size to 12 MB. Physically writing the file full; Please wait ...
2021-01-04  5:47:09 0 [Note] InnoDB: File './ibtmp1' size is now 12 MB.
2021-01-04  5:47:09 0 [Note] InnoDB: 10.5.8 started; log sequence number 0; transaction id 7
2021-01-04  5:47:09 0 [Note] Plugin 'FEEDBACK' is disabled.
2021-01-04  5:47:09 0 [ERROR] Could not open mysql.plugin table: "Table 'mysql.plugin' doesn't exist". Some plugins may be not loaded
2021-01-04  5:47:09 0 [ERROR] /usr/local/opt/mariadb/bin/mariadbd: unknown variable 'mysqlx-bind-address=127.0.0.1' #在这里出现错误，是/usr/local/etc/my.cnf 倒数第二行出现的。把它删掉或注释，再次运行mysql_install_db,就会出现上面的内容。上面的内容大致说了创建了两个特权帐户都没有密码一个是root,另一个是以你电脑名字作为帐户，但你得用sudo来登录。
2021-01-04  5:47:09 0 [ERROR] Aborting

```

解决了以上问题，就可以用 mysql_secure_installation 来进行mysql的安全设置了。不过它是要root权限的，所以，必须

```shell
sudo mysql_secure_installation

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user. If you've just installed MariaDB, and
haven't set the root password yet, you should just press enter here.

Enter current password for root (enter for none): 
OK, successfully used password, moving on...

Setting the root password or using the unix_socket ensures that nobody
can log into the MariaDB root user without the proper authorisation.

You already have your root account protected, so you can safely answer 'n'.

Switch to unix_socket authentication [Y/n] n
 ... skipping.

You already have your root account protected, so you can safely answer 'n'.

Change the root password? [Y/n] Y
New password: 
Re-enter new password: 
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] y
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
 ~  mysql -uroot -p
Enter password: 
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 28
Server version: 10.5.8-MariaDB Homebrew

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> show variables like 'character_set%';
+--------------------------+--------------------------------------------------------+
| Variable_name            | Value                                                  |
+--------------------------+--------------------------------------------------------+
| character_set_client     | utf8                                                   |
| character_set_connection | utf8                                                   |
| character_set_database   | utf8mb4                                                |
| character_set_filesystem | binary                                                 |
| character_set_results    | utf8                                                   |
| character_set_server     | utf8mb4                                                |
| character_set_system     | utf8                                                   |
| character_sets_dir       | /usr/local/Cellar/mariadb/10.5.8/share/mysql/charsets/ |
+--------------------------+--------------------------------------------------------+
8 rows in set (0.001 sec)

MariaDB [(none)]> 

```

这个时候登录mysql 需要`sudo mysql -uroot -p`,需要在数据库里设置

```mysql 
MariaDB [(none)]>  set password for 'root'@'localhost' = password('root');
MariaDB [(none)]>  flush privileges;
```

这样下次就可以不用借助root权限登录了。这样做的目的是为了后端代码可以在代码层面接管mysql。

