# 前言

redis，我们常用来做缓存，也是著名的nosql存储中间件。

# 安装


## 信息统计

- 下载地址：暂无。
- 软件版本： redis-3.0.0
- 安装地址： /usr/setup/redis-3.0.0
- 配置文档地址： /usr/setup/redis-3.0.0/redis.conf
- 占用端口：6379
- 用户名和密码：无

## 安装步骤

- 安装gcc编译工具包

```
$ yum install -y gcc
```

- 解压

```
$ tar zxf /opt/package/redis-3.0.0.tar.gz -C /opt/unziped/
```

- 创建文件夹

```
$ mkdir /usr/setup/redis-3.0.0
$ mkdir /usr/setup/redis-3.0.0/log
$ mkdir /usr/setup/redis-3.0.0/data
```

- 进入解压后的redis目录并执行make命令

```
$ make
```

- 执行安装命令

```
$ make install PREFIX=/usr/setup/redis-3.0.0
```

- 拷贝配置文件

```
$ cp /opt/unziped/redis-3.0.0/redis.conf /usr/setup/redis-3.0.0
```

- 打开守护进程运行模式

```
// 修改配置文件，将daemonize的值改为yes
$ vi /usr/setup/redis-3.0.0/redis.conf
daemonize yes
```

- 加入开机自启

```
$ ./opt/unziped/redis-3.0.0/utils/install_server.sh
Welcome to the redis service installer
This script will help you easily set up a running redis server
Please select the redis port for this instance: [6379]
Selecting default: 6379
Please select the redis config file name [/etc/redis/6379.conf] /usr/setup/redis-3.0.0/redis.conf
Please select the redis log file name [/var/log/redis_6379.log] /usr/setup/redis-3.0.0/log/redis.log
Please select the data directory for this instance [/var/lib/redis/6379] /usr/setup/redis-3.0.0/data/6379
Please select the redis executable path [] /usr/setup/redis-3.0.0/bin/redis-server
Selected config:
Port           : 6379
Config file    : /usr/setup/redis-3.0.0/redis.conf
Log file       : /usr/setup/redis-3.0.0/log/redis.log
Data dir       : /usr/setup/redis-3.0.0/data/6379
Executable     : /usr/setup/redis-3.0.0/bin/redis-server
Cli Executable : /usr/setup/redis-3.0.0/bin/redis-cli
Is this ok? Then press ENTER to go on or Ctrl-C to abort.
Copied /tmp/6379.conf => /etc/init.d/redis_6379
Installing service...
Successfully added to chkconfig!
Successfully added to runlevels 345!
Starting Redis server...
Installation successful!
```

- 开启端口，并重启防火墙

```
$ firewall-cmd --zone=public --permanent --add-port=6379/tcp
$ firewall-cmd --reload
```

- 重启后测试

```
1. 本机连接测试
$ cd /usr/setup/redis-3.0.0/bin/
$ ./redis-cli
127.0.0.1:6379> info

2. 开发机连接测试
cmd: redis-cli.exe -h 10.168.0.120 -p 6379
```

- redis5的安装

redis5的安装方式和上述安装方式基本一致，只需要下载对应的安装包，再把redis3.0替换成redis5.0即可。


# 文件及各自作用

```
[root@redis ~]# ll /usr/setup/redis-5.0.3/bin/
总用量 32672
-rwxr-xr-x. 1 root root 4366656 9月  25 22:08 redis-benchmark
-rwxr-xr-x. 1 root root 8090088 9月  25 22:08 redis-check-aof
-rwxr-xr-x. 1 root root 8090088 9月  25 22:08 redis-check-rdb
-rwxr-xr-x. 1 root root 4801928 9月  25 22:08 redis-cli
lrwxrwxrwx. 1 root root      12 9月  25 22:08 redis-sentinel -> redis-server
-rwxr-xr-x. 1 root root 8090088 9月  25 22:08 redis-server

./redis-benchmark   //用于进行redis性能测试的工具
./redis-check-dump  //用于修复出问题的dump.rdb文件
./redis-cli         //redis的客户端
./redis-server      //redis的服务端
./redis-check-aof   //用于修复出问题的AOF文件
./redis-sentinel    //用于集群管理

```

# 简单使用

## 前期使用

- 查看redis进程

```
ps ef | grep redis
```

- 查看端口占用

```
// 需要安装lsof工具
yum install -y lsof

lsof -i:6379
```

- 启动服务端

```
// 需注意配置redis-server在%REDIS_HOME%/bin目录下面，redis.conf路径也要正确

./redis-server redis.conf
```

- 启动客户端

在服务器的机子上面打开客户端，直接使用下面的命令即可：

```
./redis-cli
```

也可以加上端口和ip地址

```
./redis-cli -p 6379 -h 127.0.0.1
```

- 关闭服务端

```
// 在打开的redis-cli命令行中直接输入shutdown即可
shutdown
```

- 关闭客户端

```
// 在打开的redis-cli命令行中直接输入exit即可退出客户端
exit
```

## key



## string

- 数据结构：字符串
- 类似于java中的字符串，是二进制安全的，也就是说可以将图片文件的二进制内容作为字符串来存储
- 操作
  - set key1 "value1"
  - get key1
  - set strNum "1"
  - get strNum
  - incr strNum
  - decr strNum
  - incrby strNum 2
  - decrby strNum 1
- 使用场景
  - 不少网站都利用redis的这个特性来实现业务上的统计计数需求。

## list

- 数据结构：链表
- 查找效率较低；新增和删除时间复杂度为O(1)
- 操作
  - lpush
  - rpush
  - lrang
- 使用场景
  - 我们可以利用lists来实现一个消息队列，而且可以确保先后顺序，不必像MySQL那样还需要通过ORDER BY来进行排序。
  - 利用LRANGE还可以很方便的实现分页的功能。
  - 在博客系统中，每片博文的评论也可以存入一个单独的list中。


## set

- 数据结构：集合
- 特点：无序
- 操作
  - sadd
  - smembers
  - sismember
  - spop
  - sunion
- 使用场景
  - 添加新元素、删除已有元素、取交集、取并集、取差集
  - QQ有一个社交功能叫做“好友标签”，大家可以给你的好友贴标签，比如“大美女”、“土豪”、“欧巴”等等，这时就可以使用redis的集合来实现，把每一个用户的标签都存储在一个集合之中。

## zset

- 数据结构：集合
- 特点：有序
- 操作
  - zadd
  - zrange
  - zrangebyscore
- 使用场景
  - 需要排序的，并且使用到集合的都可以

## hash

- 数据结构：

# 配置文件

https://blog.csdn.net/weixin_42425970/article/details/94132652

# 持久化

## RDB

redis database
- 在不同时刻点，将内存镜像到本地磁盘，使用的是快照的方式。
- 在持久化过程时，主进程会fork出一个子进程，此时主进程不会进程任何的io操作的，子进程会将内存中的数据先持久化到一个临时文件中，完成后，使用临时文件替换旧的rdb文件。
- 这种方式总会丢失一些持久化之前的数据内容

## AOF

append only file
- 将执行的所有的写操作都记录到一个文件中，恢复时使用该文件重新在redis中执行一次，就可以复原原来的内存数据了。
-

## which one


# 集群和主从复制

# 事务

# 发布和订阅


# 参考链接

1. https://blog.csdn.net/liqingtx/article/details/60330555
2. https://blog.csdn.net/weixin_42425970/article/details/94132652
