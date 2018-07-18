[TOC]

# 1 卸载MariaDB
## 1.1 MariaDB知识
```
作者：嘎吱喀吧
链接：https://www.zhihu.com/question/41832866/answer/92726790
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

MariaDB数据库管理系统是MySQL的一个分支，主要由开源社区在维护，采用GPL授权许可。开发这个分支的原因之一是：甲骨文公司收购了MySQL后，有将MySQL闭源的潜在风险，因此社区采用分支的方式来避开这个风险。

MariaDB的目的是完全兼容MySQL，包括API和命令行，使之能轻松成为MySQL的代替品。在存储引擎方面，10.0.9版起使用XtraDB（名称代号为Aria）来代替MySQL的InnoDB。MariaDB由MySQL的创始人麦克尔·维德纽斯主导开发，他早前曾以10亿美元的价格，将自己创建的公司MySQLAB卖给了SUN，此后，随着SUN被甲骨文收购，MySQL的所有权也落入Oracle的手中。MariaDB名称来自麦克尔·维德纽斯的女儿玛丽亚（英语：Maria）的名字。

版本
MariaDB直到5.5版本，均依照MySQL的版本。因此，使用MariaDB5.5的人会从MySQL5.5中了解到MariaDB的所有功能。从2012年11月12日起发布的10.0.0版开始，不再依照MySQL的版号。10.0.x版以5.5版为基础，加上移植自MySQL5.6版的功能和自行开发的新功能。

第三方软件
MariaDB的API和协议兼容MySQL，另外又添加了一些功能，以支持本地的非阻塞操作和进度报告。这意味着，所有使用MySQL的连接器、程序库和应用程序也将可以在MariaDB下工作。在此基础上，由于担心甲骨文MySQL的一个更加封闭的软件项目，Fedora的计划在Fedora19中的以MariaDB取代MySQL，维基媒体基金会的服务器同样也使用MariaDB取代了MySQL。
```
## 1.2 卸载命令
```
[root@studyCentOS7 mysql5.7.16]# rpm -qa|grep mariadb
mariadb-libs-5.5.47-1.el7_2.x86_64
[root@studyCentOS7 mysql5.7.16]# rpm -e mariadb-libs-5.5.47-1.el7_2.x86_64 --nodeps 
[root@studyCentOS7 mysql5.7.16]# rpm -qa|grep mariadb
[root@studyCentOS7 mysql5.7.16]# 

```


# 2 上传
略

# 3 解压
```
// 解压
[root@studyCentOS7 software_bak]#  tar xvf mysql-5.7.16-1.el7.x86_64.rpm-bundle.tar 
mysql-community-libs-compat-5.7.16-1.el7.x86_64.rpm
mysql-community-devel-5.7.16-1.el7.x86_64.rpm
mysql-community-minimal-debuginfo-5.7.16-1.el7.x86_64.rpm
mysql-community-libs-5.7.16-1.el7.x86_64.rpm
mysql-community-common-5.7.16-1.el7.x86_64.rpm
mysql-community-embedded-compat-5.7.16-1.el7.x86_64.rpm
mysql-community-test-5.7.16-1.el7.x86_64.rpm
mysql-community-embedded-devel-5.7.16-1.el7.x86_64.rpm
mysql-community-server-minimal-5.7.16-1.el7.x86_64.rpm
mysql-community-server-5.7.16-1.el7.x86_64.rpm
mysql-community-client-5.7.16-1.el7.x86_64.rpm
mysql-community-embedded-5.7.16-1.el7.x86_64.rpm

// 创建目录并移动解压出来的文件
[root@studyCentOS7 software_bak]# mkdir mysql5.7.16 
[root@studyCentOS7 software_bak]# mv mysql-community-* ./mysql5.7.16/

// 列出所有的文件
[root@studyCentOS7 mysql5.7.16]# ll
总用量 556624
-rw-r--r--. 1 7155 31415  25034716 9月  29 2016 mysql-community-client-5.7.16-1.el7.x86_64.rpm
-rw-r--r--. 1 7155 31415    277604 9月  29 2016 mysql-community-common-5.7.16-1.el7.x86_64.rpm
-rw-r--r--. 1 7155 31415   3771664 9月  29 2016 mysql-community-devel-5.7.16-1.el7.x86_64.rpm
-rw-r--r--. 1 7155 31415  45342240 9月  29 2016 mysql-community-embedded-5.7.16-1.el7.x86_64.rpm
-rw-r--r--. 1 7155 31415  23923288 9月  29 2016 mysql-community-embedded-compat-5.7.16-1.el7.x86_64.rpm
-rw-r--r--. 1 7155 31415 125563004 9月  29 2016 mysql-community-embedded-devel-5.7.16-1.el7.x86_64.rpm
-rw-r--r--. 1 7155 31415   2237116 9月  29 2016 mysql-community-libs-5.7.16-1.el7.x86_64.rpm
-rw-r--r--. 1 7155 31415   2112700 9月  29 2016 mysql-community-libs-compat-5.7.16-1.el7.x86_64.rpm
-rw-r--r--. 1 7155 31415  51541740 9月  29 2016 mysql-community-minimal-debuginfo-5.7.16-1.el7.x86_64.rpm
-rw-r--r--. 1 7155 31415 159295840 9月  29 2016 mysql-community-server-5.7.16-1.el7.x86_64.rpm
-rw-r--r--. 1 7155 31415  14000136 9月  29 2016 mysql-community-server-minimal-5.7.16-1.el7.x86_64.rpm
-rw-r--r--. 1 7155 31415 116862620 9月  29 2016 mysql-community-test-5.7.16-1.el7.x86_64.rpm
```

# 4 安装与配置
## 4.1 安装
```
[root@studyCentOS7 mysql5.7.16]#  rpm -ivh mysql-community-common-5.7.16-1.el7.x86_64.rpm 
警告：mysql-community-common-5.7.16-1.el7.x86_64.rpm: 头V3 DSA/SHA1 Signature, 密钥 ID 5072e1f5: NOKEY
准备中...                          ################################# [100%]
正在升级/安装...
   1:mysql-community-common-5.7.16-1.e################################# [100%]

[root@studyCentOS7 mysql5.7.16]# rpm -ivh mysql-community-libs-5.7.16-1.el7.x86_64.rpm  
警告：mysql-community-libs-5.7.16-1.el7.x86_64.rpm: 头V3 DSA/SHA1 Signature, 密钥 ID 5072e1f5: NOKEY
准备中...                          ################################# [100%]
正在升级/安装...
   1:mysql-community-libs-5.7.16-1.el7################################# [100%]

[root@studyCentOS7 mysql5.7.16]# rpm -ivh mysql-community-client-5.7.16-1.el7.x86_64.rpm  
警告：mysql-community-client-5.7.16-1.el7.x86_64.rpm: 头V3 DSA/SHA1 Signature, 密钥 ID 5072e1f5: NOKEY
准备中...                          ################################# [100%]
正在升级/安装...
   1:mysql-community-client-5.7.16-1.e################################# [100%]

[root@studyCentOS7 mysql5.7.16]# rpm -ivh mysql-community-server-5.7.16-1.el7.x86_64.rpm  
警告：mysql-community-server-5.7.16-1.el7.x86_64.rpm: 头V3 DSA/SHA1 Signature, 密钥 ID 5072e1f5: NOKEY
准备中...                          ################################# [100%]
正在升级/安装...
   1:mysql-community-server-5.7.16-1.e################################# [100%]
```
## 4.2 初始化并登录测试
如果是以 mysql 身份运行，则可以去掉 --user 选项。
另外 --initialize 选项默认以“安全”模式来初始化，则会为 root 用户生成一个密码并将该密码标记为过期，登陆后你需要设置一个新的密码，而使用 --initialize-insecure 命令则不使用安全模式，则不会为root 用户生成一个密码。

```
// 初始化数据库
[root@studyCentOS7 mysql5.7.16]# mysqld --initialize --user=mysql

// 查看密码，最后一行就是密码
[root@studyCentOS7 mysql5.7.16]# cat /var/log/mysqld.log   
2017-11-29T03:06:29.971884Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
2017-11-29T03:06:30.186439Z 0 [Warning] InnoDB: New log files created, LSN=45790
2017-11-29T03:06:30.239928Z 0 [Warning] InnoDB: Creating foreign key constraint system tables.
2017-11-29T03:06:30.321974Z 0 [Warning] No existing UUID has been found, so we assume that this is the first time that this server has been started. Generating a new UUID: 4c0c1fa1-d4b2-11e7-b57d-000c294e37eb.
2017-11-29T03:06:30.323108Z 0 [Warning] Gtid table is not ready to be used. Table 'mysql.gtid_executed' cannot be opened.
2017-11-29T03:06:30.324861Z 1 [Note] A temporary password is generated for root@localhost: yvw>IycC_8<T

// 查看MySQL服务是否启动
[root@studyCentOS7 mysql5.7.16]# systemctl status mysqld     
● mysqld.service - MySQL Server
   Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; vendor preset: disabled)
   Active: inactive (dead)

// 启动mysql服务
[root@studyCentOS7 mysql5.7.16]# systemctl start mysqld
[root@studyCentOS7 mysql5.7.16]# systemctl status mysqld  
● mysqld.service - MySQL Server
   Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; vendor preset: disabled)
   Active: active (running) since 三 2017-11-29 11:11:33 CST; 1s ago
  Process: 9702 ExecStart=/usr/sbin/mysqld --daemonize --pid-file=/var/run/mysqld/mysqld.pid $MYSQLD_OPTS (code=exited, status=0/SUCCESS)
  Process: 9685 ExecStartPre=/usr/bin/mysqld_pre_systemd (code=exited, status=0/SUCCESS)
 Main PID: 9706 (mysqld)
   CGroup: /system.slice/mysqld.service
           └─9706 /usr/sbin/mysqld --daemonize --pid-file=/var/run/mysqld/mysqld.pid

11月 29 11:11:32 studyCentOS7.203 systemd[1]: Starting MySQL Server...
11月 29 11:11:33 studyCentOS7.203 systemd[1]: Started MySQL Server.

// 使用root用户登录MySQL进行测试
[root@studyCentOS7 mysql5.7.16]# mysql -uroot -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.16

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 

// 此刻做任何事情都是不允许的，因为使用的是默认密码登录的，MySQL强制你修改默认密码
mysql> show databases;
ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.
```

## 4.3 配置
### 4.3.1 修改root用户密码
```
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'northmeter@2017&*(';
Query OK, 0 rows affected (0.00 sec)
```
### 4.3.2 创建用户
```
// 1. 创建能够在任意主机上登录的northmeter用户
mysql> create user 'northmeter'@'%' identified by 'northmeter!@#';
Query OK, 0 rows affected (0.00 sec)

// 2. 为northmeter用户赋予增删改查的权限
mysql> grant select, insert, update, delete on *.* to 'northmeter'@'%';
Query OK, 0 rows affected (0.00 sec)

// 3. 刷新权限
mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)
********************************************************************
// 1. 创建管理员用户
mysql> create user 'admin'@'%' identified by 'admin@2017!@#';
Query OK, 0 rows affected (0.00 sec)

// 2. 赋予权限
mysql> grant all on *.* to 'admin'@'%' identified by 'admin@2017!@#';
Query OK, 0 rows affected, 1 warning (0.00 sec)

// 3. 刷新权限
mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)
create user 'admin'@'%' identified by 'admin@2017!@#';
grant all on *.* to 'admin'@'%' identified by 'admin@2017!@#';
flush privileges;

```

### 4.3.3 为root赋予全程登录权限
**说明：** *此过程非必须，应该是最好不要。*
```
// 为root用户赋予远程登录的权限
mysql> grant all on *.* to 'root'@'%' identified by 'northmeter@2017&*(';
Query OK, 0 rows affected, 1 warning (0.00 sec)

// 刷新权限
mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)
```

### 4.3.4 开放3306端口
```
[root@studyCentOS7 mysql5.7.16]# firewall-cmd --zone=public --add-port=3306/tcp --permanent    
success
[root@studyCentOS7 mysql5.7.16]# firewall-cmd --reload
success
```

### 4.3.5 软件配置信息
```
[root@200 etc]# cat my.cnf
# For advice on how to change settings please see
# http://dev.mysql.com/doc/refman/5.7/en/server-configuration-defaults.html

[mysqld]
#
# Remove leading # and set to the amount of RAM for the most important data
# cache in MySQL. Start at 70% of total RAM for dedicated server, else 10%.
# innodb_buffer_pool_size = 128M
#
# Remove leading # to turn on a very important data integrity option: logging
# changes to the binary log between backups.
# log_bin
#
# Remove leading # to set options mainly useful for reporting servers.
# The server defaults are faster for transactions and fast SELECTs.
# Adjust sizes as needed, experiment to find the optimal values.
# join_buffer_size = 128M
# sort_buffer_size = 2M
# read_rnd_buffer_size = 2M
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock

# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0

log-error=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid

# 设置不区分大小写
lower_case_table_names=1
```

# 5 补充
## 5.1 创建用户
命令格式
```
CREATE USER 'username'@'host' IDENTIFIED BY 'password';
```
例子 
```
CREATE USER 'dog'@'localhost' IDENTIFIED BY '123456';

CREATE USER 'dog2'@'localhost' IDENTIFIED BY '';
```
命令讲解：
```
username - 你将创建的用户名
host - 指定该用户在哪个主机上可以登陆，此处的"localhost"，是指该用户只能在本地登录，不能在另外一台机器上远程登录，如果想远程登录的话，将"localhost"改为"%"，表示在任何一台电脑上都可以登录;也可以指定某台机器可以远程登录;
password - 该用户的登陆密码,密码可以为空,如果为空则该用户可以不需要密码登陆服务器。
```

## 5.2 授权
```
命令:GRANT privileges ON databasename.tablename TO 'username'@'host'

PS: privileges - 用户的操作权限,如SELECT , INSERT , UPDATE 等(详细列表见该文最后面).如果要授予所的权限则使用ALL.;databasename - 数据库名,tablename-表名,如果要授予该用户对所有数据库和表的相应操作权限则可用*表示, 如*.*.

例子: GRANT SELECT, INSERT ON mq.* TO 'dog'@'localhost';
```

## 5.3 创建用户同时授权
```
mysql> grant all privileges on mq.* to test@localhost identified by '1234';
Query OK, 0 rows affected, 1 warning (0.00 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.01 sec)

PS:必须执行flush privileges; 

否则登录时提示：ERROR 1045 (28000): Access denied for user 'user'@'localhost' (using password: YES ) 
```

## 5.4 其他
```
四.设置与更改用户密码

命令:SET PASSWORD FOR 'username'@'host' = PASSWORD('newpassword');

例子: SET PASSWORD FOR 'dog2'@'localhost' = PASSWORD("dog");

五.撤销用户权限

      命令: REVOKE privilege ON databasename.tablename FROM 'username'@'host';

     说明: privilege, databasename, tablename - 同授权部分.

      例子: REVOKE SELECT ON mq.* FROM 'dog2'@'localhost';

PS: 假如你在给用户'dog'@'localhost''授权的时候是这样的(或类似的):GRANT SELECT ON test.user TO 'dog'@'localhost', 则在使用REVOKE SELECT ON *.* FROM 'dog'@'localhost';命令并不能撤销该用户对test数据库中user表的SELECT 操作.相反,如果授权使用的是GRANT SELECT ON *.* TO 'dog'@'localhost';则REVOKE SELECT ON test.user FROM 'dog'@'localhost';命令也不能撤销该用户对test数据库中user表的Select 权限.

      具体信息可以用命令SHOW GRANTS FOR 'dog'@'localhost'; 查看.

六.删除用户

      命令: DROP USER 'username'@'host';

七.查看用户的授权

mysql> show grants for dog@localhost;
+---------------------------------------------+
| Grants for dog@localhost |
+---------------------------------------------+
| GRANT USAGE ON *.* TO 'dog'@'localhost' |
| GRANT INSERT ON `mq`.* TO 'dog'@'localhost' |
+---------------------------------------------+
2 rows in set (0.00 sec)

PS:GRANT USAGE:mysql usage权限就是空权限，默认create user的权限，只能连库，啥也不能干
```











