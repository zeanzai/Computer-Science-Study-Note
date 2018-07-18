```shell
// 解压文件
[root@study software_bak]# tar zxf jdk-8u144-linux-x64.tar.gz 
[root@study software_bak]# ll
总用量 337108
-rw-r--r--. 1 root root   8234674 9月  19 18:43 apache-tomcat-7.0.47.tar.gz
drwxr-xr-x. 8 uucp  143      4096 7月  22 13:11 jdk1.8.0_144
-rw-r--r--. 1 root root 185515842 9月  19 19:42 jdk-8u144-linux-x64.tar.gz
-rw-r--r--. 1 root root     57856 9月  15 00:40 redis-3.0.0.gem
-rw-r--r--. 1 root root   1358081 9月  14 19:05 redis-3.0.0.tar.gz
-rw-r--r--. 1 root root 150010621 9月  19 18:43 solr-4.10.3.tgz.tgz

// 移动到安装目录
[root@study software_bak]# mv jdk1.8.0_144/ /usr/local/


// 在/etc/profile文件末尾追加环境变量
// 查看修改后的环境变量文件
[root@study local]# tail -f /etc/profile


# Java安装环境
export JAVA_HOME=/softwares/jdk1.8.0_111
export JRE_HOME=$JAVA_HOME/jre
export CLASSPATH=./:$JAVA_HOME/lib:$JAVA_HOME/jre/lib
export PATH=$PATH:$JAVA_HOME/bin

// 使环境变量文件生效
[root@study local]# source /etc/profile

// 永久生效需要重启
[root@study local]# reboot


```