---
layout: post
title:  "Nginx安装手册"
date:   2018-06-06 10:42:27 +0800
author: zeanzai
catalog: true
tags: 
    - install
    - DevOps
    - Centos7
---

# Nginx安装手册

## 准备
 - 建立`/opt/setups`和`/usr/program`目录
 - 下载 `nginx-1.12.2.tar.gz` 文件，并上传到服务器 /opt/setups目录下

## 安装依赖
```shell
[root@VM_0_6_centos nginx-1.12.2]# cat /etc/redhat-release # 查看系统版本
CentOS Linux release 7.4.1708 (Core) 

[root@VM_0_6_centos ~]# yum install gcc-c++ 
[root@VM_0_6_centos ~]# yum install -y pcre pcre-devel
[root@VM_0_6_centos ~]# yum install -y zlib zlib-devel
[root@VM_0_6_centos ~]# yum install -y openssl openssl-devel
```

## 编译安装
```shell
[root@VM_0_6_centos /]# cd /opt/setups/
[root@VM_0_6_centos setups]# tar zxf nginx-1.12.2.tar.gz 
```

```shell
// 配置文件
./configure --prefix=/usr/program/nginx --pid-path=/usr/program/nginx/nginx.pid --lock-path=/usr/program/nginx/lock/nginx.lock --error-log-path=/usr/program/nginx/log/error.log --http-log-path=/usr/program/nginx/log/access.log --with-http_stub_status_module --with-http_ssl_module --with-http_gzip_static_module --http-client-body-temp-path=/usr/program/nginx/temp/client --http-proxy-temp-path=/usr/program/nginx/temp/proxy  --http-fastcgi-temp-path=/usr/program/nginx/temp/fastcgi  --http-uwsgi-temp-path=/usr/program/nginx/temp/uwsgi --http-scgi-temp-path=/usr/program/nginx/temp/scgi


// 配置
[root@VM_0_6_centos nginx-1.12.2]# ./configure --prefix=/usr/program/nginx --pid-path=/usr/program/nginx/nginx.pid --lock-path=/usr/program/nginx/lock/nginx.lock --error-log-path=/usr/program/nginx/log/error.log --http-log-path=/usr/program/nginx/log/access.log --with-http_stub_status_module --with-http_ssl_module --with-http_gzip_static_module --http-client-body-temp-path=/usr/program/nginx/temp/client --http-proxy-temp-path=/usr/program/nginx/temp/proxy  --http-fastcgi-temp-path=/usr/program/nginx/temp/fastcgi  --http-uwsgi-temp-path=/usr/program/nginx/temp/uwsgi --http-scgi-temp-path=/usr/program/nginx/temp/scgi

......
Configuration summary
  + using system PCRE library
  + using system OpenSSL library
  + using system zlib library

  nginx path prefix: "/usr/program/nginx"
  nginx binary file: "/usr/program/nginx/sbin/nginx"
  nginx modules path: "/usr/program/nginx/modules"
  nginx configuration prefix: "/usr/program/nginx/conf"
  nginx configuration file: "/usr/program/nginx/conf/nginx.conf"
  nginx pid file: "/usr/program/nginx/nginx.pid"
  nginx error log file: "/usr/program/nginx/log/error.log"
  nginx http access log file: "/usr/program/nginx/log/access.log"
  nginx http client request body temporary files: "/usr/program/nginx/temp/client"
  nginx http proxy temporary files: "/usr/program/nginx/temp/proxy"
  nginx http fastcgi temporary files: "/usr/program/nginx/temp/fastcgi"
  nginx http uwsgi temporary files: "/usr/program/nginx/temp/uwsgi"
  nginx http scgi temporary files: "/usr/program/nginx/temp/scgi"
  
[root@VM_0_6_centos nginx-1.12.2]# make
[root@VM_0_6_centos nginx-1.12.2]# make install
[root@VM_0_6_centos nginx-1.12.2]# cd /usr/program/
[root@VM_0_6_centos program]# cd nginx/
[root@VM_0_6_centos nginx]# ll
total 24
drwxr-xr-x 2 root root 4096 May 29 13:52 conf
drwxr-xr-x 2 root root 4096 May 29 13:52 html
drwxr-xr-x 2 root root 4096 May 29 13:50 lock
drwxr-xr-x 2 root root 4096 May 29 13:50 log
drwxr-xr-x 2 root root 4096 May 29 13:52 sbin
drwxr-xr-x 2 root root 4096 May 29 13:50 temp
[root@VM_0_6_centos nginx]# cd sbin/
[root@VM_0_6_centos sbin]# ll
[root@VM_0_6_centos sbin]# ./nginx 
[root@VM_0_6_centos sbin]# ps aux | grep nginx
```

> 执行./nginx启动nginx，这里可以-c指定加载的nginx配置文件，如下：
./nginx -c /usr/local/nginx/conf/nginx.conf
如果不指定-c，nginx在启动时默认加载conf/nginx.conf文件，此文件的地址也可以在编译安装nginx时指定./configure的参数（--conf-path= 指向配置文件（nginx.conf））

## 停止
方式1，快速停止：
cd /usr/local/nginx/sbin
./nginx -s stop
此方式相当于先查出nginx进程id再使用kill命令强制杀掉进程。

方式2，完整停止(建议使用)：
cd /usr/local/nginx/sbin
./nginx -s quit
此方式停止步骤是待nginx进程处理任务完毕进行停止。


## 重新启动
方式1，先停止再启动（建议使用）：
对nginx进行重启相当于先停止nginx再启动nginx，即先执行停止命令再执行启动命令。
如下：
./nginx -s quit
./nginx

方式2，重新加载配置文件：
当nginx的配置文件nginx.conf修改后，要想让配置生效需要重启nginx，使用-s reload不用先停止nginx再启动nginx即可将配置信息在nginx中生效，如下：
./nginx -s reload

## 测试
![nginx_result](F:\budget\install\nginx_result.png)

## 设置开机自启动
```shell
#!/bin/bash
# nginx Startup script for the Nginx HTTP Server
# it is v.0.0.2 version.
# chkconfig: - 85 15
# description: Nginx is a high-performance web and proxy server.
#              It has a lot of features, but it's not for everyone.
# processname: nginx
# pidfile: /usr/program/nginx/nginx.pid
# config: /usr/program/nginx/nginx.conf
nginxd=/usr/program/nginx/sbin/nginx
nginx_config=/usr/program/nginx/conf/nginx.conf
nginx_pid=/usr/program/nginx/nginx.pid
RETVAL=0
prog="nginx"
# Source function library.
. /etc/rc.d/init.d/functions
# Source networking configuration.
. /etc/sysconfig/network
# Check that networking is up.
[ ${NETWORKING} = "no" ] && exit 0
[ -x $nginxd ] || exit 0
# Start nginx daemons functions.
start() {
if [ -e $nginx_pid ];then
   echo "nginx already running...."
   exit 1
fi
   echo -n $"Starting $prog: "
   daemon $nginxd -c ${nginx_config}
   RETVAL=$?
   echo
   [ $RETVAL = 0 ] && touch /usr/program/nginx/lock/subsys/nginx
   return $RETVAL
}
# Stop nginx daemons functions.
stop() {
        echo -n $"Stopping $prog: "
        killproc $nginxd
        RETVAL=$?
        echo
        [ $RETVAL = 0 ] && rm -f /usr/program/nginx/lock/subsys/nginx /usr/program/nginx/nginx.pid
}
# reload nginx service functions.
reload() {
    echo -n $"Reloading $prog: "
    #kill -HUP `cat ${nginx_pid}`
    killproc $nginxd -HUP
    RETVAL=$?
    echo
}
# See how we were called.
case "$1" in
start)
        start
        ;;
stop)
        stop
        ;;
reload)
        reload
        ;;
restart)
        stop
        start
        ;;
status)
        status $prog
        RETVAL=$?
        ;;
*)
        echo $"Usage: $prog {start|stop|restart|reload|status|help}"
        exit 1
esac
exit $RETVAL
```

:wq  保存并退出

## 设置文件的访问权限
```shell
[root@VM_0_6_centos init.d]# chmod a+x /etc/init.d/nginx   
[root@VM_0_6_centos init.d]# nginx status
[root@VM_0_6_centos init.d]# ./nginx stop
[root@VM_0_6_centos init.d]# ./nginx start
```
如果修改了nginx的配置文件nginx.conf，也可以使用上面的命令重新加载新的配置文件并运行，可以将此命令加入到rc.local文件中，这样开机的时候nginx就默认启动了

vi /etc/rc.local

加入一行  /etc/init.d/nginx start    保存并退出，下次重启会生效。











