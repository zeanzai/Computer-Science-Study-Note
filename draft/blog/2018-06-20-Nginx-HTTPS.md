---
layout: post
title:  "在Nginx上面配置HTTPS，并实现跳转"
date:   2018-06-20
author: zeanzai
catalog: true
tags: 
    - work
---
# 查看是否安装ssl模块
```shell
./nginx -V

```
查看是否有http_ssl_module模块

# 安装http_ssl_module模块
## 关闭Nginx
```
cd /usr/program/nginx/sbin/
./nginx -s stop
```

## 进入源码目录
```shell
cd /opt/setups/nginx-1.12.2
```
## 重新config
```shell
./configure --prefix=/usr/program/nginx --pid-path=/usr/program/nginx/nginx.pid --lock-path=/usr/program/nginx/lock/nginx.lock --error-log-path=/usr/program/nginx/log/error.log --http-log-path=/usr/program/nginx/log/access.log **--with-http_stub_status_module --with-http_ssl_module --with-http_gzip_static_module** --http-client-body-temp-path=/usr/program/nginx/temp/client --http-proxy-temp-path=/usr/program/nginx/temp/proxy  --http-fastcgi-temp-path=/usr/program/nginx/temp/fastcgi  --http-uwsgi-temp-path=/usr/program/nginx/temp/uwsgi --http-scgi-temp-path=/usr/program/nginx/temp/scgi
```
## 然后make，万万不可执行make install
```shell
make
```
## 备份原来Nginx运行文件，并拷贝make产生的Nginx到原来安装目录
```shell
// 备份
mv /usr/program/nginx/sbin/nginx /usr/program/nginx/sbin/nginx_bak

// 拷贝
cp /opt/setups/nginx-1.12.2/objs/nginx /usr/program/nginx/sbin/
```
# 设置HTTPS域名
1. 在西部数码上面申请相对应的SSL证书
申请的是TrustAsia 域名型（DV）证书，即一个域名对应一个ssl证书。
2. 下载ssl证书，按照西部数码上面的部署导引，合并文件，并将文件上传到服务器指定目录（一般情况下放到conf目录下面，并在此建立cert目录，就放到cert目录下面）
3. 配置Nginx
```shell
server {  
	listen  80;  
	server_name api.xxxx.com;   
	rewrite ^(.*)$  https://$host$1 permanent;  
    }  
    server {
        listen       443 ssl;
        server_name  api.xxxx.com;

        ssl_certificate      cert/api.beidiancloud.com_ca.crt;
        ssl_certificate_key  cert/api.beidiancloud.com.key;

	ssl_session_cache    shared:SSL:1m;
	ssl_session_timeout  5m;   
	
	ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers ALL:!DH:!EXPORT:!RC4:+HIGH:+MEDIUM:!LOW:!aNULL:!eNULL;

        location / {
            root   html/xxx; // 可使用绝对路径
            index  index.html index.htm;
        }
    }
```
4. 重启Nginx
5. 西部数码上面配置域名解析

# 踩坑
1. 不能更改证书的名称，从西部数码上面下载下来的是什么样子的，放到服务器上面的也就应该是什么样子的。
2. 遇到Nginx： [emerg] xxxxx 问题，极有可能是更改了证书名称，或者是证书放置的位置不对。
3. 配置完成后在浏览器中输入相对应的域名，却不能访问成功，原因是没有配置rewrite。


> 参考： 
> 1. 
nginx使用ssl模块配置支持HTTPS访问    https://blog.csdn.net/duyusean/article/details/79348613
> 2. 自助安装SSL证书教程 https://www.west.cn/faq/list.asp?unid=1406
> 3. nginx强制使用https访问(http跳转到https) http://www.cnblogs.com/yun007/p/3739182.html


