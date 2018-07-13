# 前言

笔者在进行试验时，有以下几个操作习惯，具体[参考](https://github.com/zeanzai/Computer-Science-Study-Note/blob/master/operation/README.md)。

Centos7默认开通了80端口和22端口。

# 环境检查

# 创建安装目录

```shell
$ mkdir /usr/setup/nginx_1.12.2
$ mkdir /usr/setup/nginx_1.12.2/temp
```

# 安装依赖

```shell
$ yum install -y gcc-c++ pcre pcre-devel zlib zlib-devel openssl openssl-devel
```

# 下载安装包并上传

将安装包下载到本地，然后使用filezilla上传到`/opt/package`

# 安装

## 解压

```shell
$ tar zxf nginx-1.12.2.tar.gz
$ cd nginx-1.12.2/
```

## 安装

```shell
$ ./configure --prefix=/usr/setup/nginx_1.12.2 \
--pid-path=/usr/setup/nginx_1.12.2/nginx.pid \
--lock-path=/usr/setup/nginx_1.12.2/lock/nginx.lock \
--error-log-path=/usr/setup/nginx_1.12.2/log/error.log \
--http-log-path=/usr/setup/nginx_1.12.2/log/access.log \
--http-client-body-temp-path=/usr/setup/nginx_1.12.2/temp/client \
--http-proxy-temp-path=/usr/setup/nginx_1.12.2/temp/proxy \
--http-fastcgi-temp-path=/usr/setup/nginx_1.12.2/temp/fastcgi \
--http-uwsgi-temp-path=/usr/setup/nginx_1.12.2/temp/uwsgi \
--http-scgi-temp-path=/usr/setup/nginx_1.12.2/temp/scgi \
--with-http_stub_status_module \
--with-http_gzip_static_module \
--with-http_ssl_module 

......
Configuration summary
  + using system PCRE library
  + using system OpenSSL library
  + using system zlib library

  nginx path prefix: "/usr/setup/nginx_1.12.2"
  nginx binary file: "/usr/setup/nginx_1.12.2/sbin/nginx"
  nginx modules path: "/usr/setup/nginx_1.12.2/modules"
  nginx configuration prefix: "/usr/setup/nginx_1.12.2/conf"
  nginx configuration file: "/usr/setup/nginx_1.12.2/conf/nginx.conf"
  nginx pid file: "/usr/setup/nginx_1.12.2/nginx.pid"
  nginx error log file: "/usr/setup/nginx_1.12.2/log/error.log"
  nginx http access log file: "/usr/setup/nginx_1.12.2/log/access.log"
  nginx http client request body temporary files: "/usr/setup/nginx_1.12.2/temp/client"
  nginx http proxy temporary files: "/usr/setup/nginx_1.12.2/temp/proxy"
  nginx http fastcgi temporary files: "/usr/setup/nginx_1.12.2/temp/fastcgi"
  nginx http uwsgi temporary files: "/usr/setup/nginx_1.12.2/temp/uwsgi"
  nginx http scgi temporary files: "/usr/setup/nginx_1.12.2/temp/scgi"
  
$ make
$ make install
```

# 测试

## 启动和关闭

```shell
// 切换到Nginx安装目录
$ cd /usr/setup/nginx_1.12.2/sbin

// 启动 （也可以加上 -C /usr/setup/nginx_1.12.2/config/nginx.conf 配置文件）
$ ./nginx

// 关闭
第一种方式 查找相关进程，然后kill进程
第二种方式 ./nginx stop
第三种方式 ./nginx quit
$ ./


```



# 测试







# 配置开机自启

## 创建服务文件nginx.service

```shell
$ vi /lib/systemd/system/nginx.service
[Unit]
Description=nginx
After=network.target

[Service]
Type=forking
ExecStart=/usr/setup/nginx_1.12.2/sbin/nginx
ExecReload=/usr/setup/nginx_1.12.2/sbin/nginx -s reload
ExecStop=/usr/setup/nginx_1.12.2/sbin/nginx -s quit
PrivateTmp=true

[Install]
WantedBy=multi-user.target

```



## 将服务文件添加到开机自启进程

```shell
$ systemctl enable nginx.service
```





# 填坑

- 启动Nginx后，不能够打开测试网页

  检查系统进程

  检查安全组 -> 检查端口开放情况

  





 









