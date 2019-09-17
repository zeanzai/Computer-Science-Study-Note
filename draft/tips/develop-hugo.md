[toc]

# 安装

> 笔者在进行试验时，有以下几个操作习惯，具体[参考](https://github.com/zeanzai/Computer-Science-Study-Note/blob/master/operation/README.md)。
> Centos7默认开通了80端口和22端口。

## 安装golang

1. 安装过程如下：

```shell
# 1. 进入安装包存放位置
[root@iZwz97ekphk0kmqgt9zvc4Z ~]# cd /opt/packages/

# 2. 下载
[root@iZwz97ekphk0kmqgt9zvc4Z packages]# wget -c https://studygolang.com/dl/golang/go1.11.linux-amd64.tar.gz
--2018-09-04 18:14:18--  https://studygolang.com/dl/golang/go1.11.linux-amd64.tar.gz
Resolving studygolang.com (studygolang.com)... 59.110.219.94
Connecting to studygolang.com (studygolang.com)|59.110.219.94|:443... connected.
HTTP request sent, awaiting response... 303 See Other
Location: https://dl.google.com/go/go1.11.linux-amd64.tar.gz [following]
--2018-09-04 18:14:19--  https://dl.google.com/go/go1.11.linux-amd64.tar.gz
Resolving dl.google.com (dl.google.com)... 203.208.40.35, 203.208.40.39, 203.208.40.40, ...
Connecting to dl.google.com (dl.google.com)|203.208.40.35|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 127163815 (121M) [application/octet-stream]
Saving to: ‘go1.11.linux-amd64.tar.gz’

100%[=======================================================================>] 127,163,815 11.9MB/s   in 10s

2018-09-04 18:14:29 (11.6 MB/s) - ‘go1.11.linux-amd64.tar.gz’ saved [127163815/127163815]

[root@iZwz97ekphk0kmqgt9zvc4Z packages]# ll
total 124188
-rw-r--r-- 1 root root 127163815 Aug 25 06:10 go1.11.linux-amd64.tar.gz

# 3. 创建安装目录
[root@iZwz97ekphk0kmqgt9zvc4Z packages]# mkdir /usr/setup

# 4. 解压到安装目录
[root@iZwz97ekphk0kmqgt9zvc4Z packages]# tar zxf go1.11.linux-amd64.tar.gz -C /usr/setup/
[root@iZwz97ekphk0kmqgt9zvc4Z packages]# cd /usr/setup/
[root@iZwz97ekphk0kmqgt9zvc4Z setup]# ll
total 4
drwxr-xr-x 10 root root 4096 Aug 25 04:41 go

# 5. 创建gopath目录结构
[root@iZwz97ekphk0kmqgt9zvc4Z setup]# mkdir go/gopath/
[root@iZwz97ekphk0kmqgt9zvc4Z setup]# mkdir go/gopath/bin
[root@iZwz97ekphk0kmqgt9zvc4Z setup]# mkdir go/gopath/src
[root@iZwz97ekphk0kmqgt9zvc4Z setup]# mkdir go/gopath/pkg

# 6. 配置环境变量
[root@iZwz97ekphk0kmqgt9zvc4Z setup]# vi /etc/profile
# 添加以下内容
export GOROOT=/usr/setup/go
export GOPATH=/usr/setup/go/gopath
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin

# 7. 使配置文件生效
[root@iZwz97ekphk0kmqgt9zvc4Z setup]# source /etc/profile

# 8. 测试
[root@iZwz97ekphk0kmqgt9zvc4Z setup]# go version
go version go1.11 linux/amd64

```

2. 参考网址
> https://www.cnblogs.com/chy123/p/6750347.html
> https://studygolang.com/dl
> https://blog.csdn.net/shida_csdn/article/details/79441694

---

## 安装git
```shell
# 1. 查看是否已经安装
[root@iZwz97ekphk0kmqgt9zvc4Z ~]# git --version

# 2. 安装依赖包
[root@iZwz97ekphk0kmqgt9zvc4Z ~]# yum install -y curl-devel expat-devel gettext-devel openssl-devel zlib-devel gcc perl-ExtUtils-MakeMaker

# 3. 进入安装包存放目录
[root@iZwz97ekphk0kmqgt9zvc4Z ~]# cd /opt/packages/

# 4. 下载
[root@iZwz97ekphk0kmqgt9zvc4Z packages]# wget -c https://github.com/git/git/archive/v2.18.0.tar.gz

# 5. 解压到当前目录
[root@iZwz97ekphk0kmqgt9zvc4Z packages]# tar zxf v2.18.0.tar.gz

# 6. 创建安装目录
[root@iZwz97ekphk0kmqgt9zvc4Z packages]# mkdir /usr/setup/git_2.18.0

# 7. 进入源码目录，并进行make配置
[root@iZwz97ekphk0kmqgt9zvc4Z packages]# cd git-2.18.0/
[root@iZwz97ekphk0kmqgt9zvc4Z packages]# make prefix=/usr/setup/git_2.18.0 all
[root@iZwz97ekphk0kmqgt9zvc4Z packages]# make prefix=/usr/setup/git_2.18.0 install

# 8. 配置环境变量，并使配置生效
[root@iZwz97ekphk0kmqgt9zvc4Z packages]# vi /etc/profile
# 添加以下内容
export GIT_HOME=/usr/setup/git_2.18.0
export PATH=$PATH:$GIT_HOME/bin
[root@iZwz97ekphk0kmqgt9zvc4Z packages]# source /etc/profile

# 9. 测试
[root@iZwz97ekphk0kmqgt9zvc4Z packages]# git --version

# 10. 配置git全局用户名和邮箱
[root@iZwz97ekphk0kmqgt9zvc4Z packages]# git config --global user.name "zeanzai"
[root@iZwz97ekphk0kmqgt9zvc4Z packages]# git config --global user.email "438123371@qq.com"
```

---

## 安装hugo
安装过程分为两种，一种是使用yum安装，一种是使用安装包形式安装。

#### 安装包安装方式

1. 查看安装包下载地址： https://github.com/gohugoio/hugo/releases/
2. 选取最新版本，并复制下载地址
3. 安装
```shell
# 1. 进入安装包存放目录
[root@izwz97ekphk0kmqgt9zvc4z ~]# cd opt/packages/

# 2. 下载安装包
[root@izwz97ekphk0kmqgt9zvc4z packages]# wget -c https://github.com/gohugoio/hugo/releases/download/v0.48/hugo_0.48_Linux-64bit.tar.gz

# 3. 解压
[root@izwz97ekphk0kmqgt9zvc4z packages]# tar zxf hugo_0.48_Linux-64bit.tar.gz

# 4. 移动
[root@izwz97ekphk0kmqgt9zvc4z packages]# mv /usr/setup/hugo/hugo /usr/local/bin/

# 5. 测试
[root@izwz97ekphk0kmqgt9zvc4z packages]# hugo version
Hugo Static Site Generator v0.48 linux/amd64 BuildDate: 2018-08-29T06:33:51Z
```
4. 参考
> https://www.jianshu.com/p/bb2d483f4caf

#### yum安装方式（建议）
1. 根据hugo官方[文档](https://gohugo.io/getting-started/installing/)提示，找到[Epel Repo](https://copr.fedorainfracloud.org/coprs/daftaupe/hugo/)下载地址，获取hugo的Epel Repo里面的[内容](https://copr.fedorainfracloud.org/coprs/daftaupe/hugo/repo/epel-7/daftaupe-hugo-epel-7.repo)
2. 安装
```shell
# 1. 将第一步获取到的内容复制到hugo.repo文件中
[root@study ~]# vi /etc/yum.repos.d/hugo.repo

# 2. 使用yum进行安装
[root@study ~]# yum -y install hugo

# 3. 测试
[root@study ~]# hugo version
Hugo Static Site Generator v0.48 linux/amd64 BuildDate: 2018-08-29T09:42:25Z
```

---

## 安装webhook
webhook可以理解成可以通过远程调用来执行的相关命令的工具。举个例子来说明：
> 需求分析
>   远程调用一个命令后，服务器打印当前服务器的时间到/opt/scripts/hugo-deploy/redeploy.log


```shell
# 1. 安装webhook客户端
[root@izwz97ekphk0kmqgt9zvc4z ~]# go get github.com/adnanh/webhook
# 这一步需要安装golang，执行webhook安装过程后，webhook的相关文件就在安装golang时配置的gopath目录下面

# 2. 创建可执行的文件
[root@izwz97ekphk0kmqgt9zvc4z ~]# mkdir -p /opt/scripts/hugo-deploy
[root@izwz97ekphk0kmqgt9zvc4z ~]# vi /opt/scripts/hugo-deploy/redeploy.sh
  #!/bin/bash

  time=`date "+%Y-%m-%d %H:%M:%S"`
  echo "${time}" >> /opt/scripts/hugo-deploy/redeploy.log
[root@izwz97ekphk0kmqgt9zvc4z ~]# chmod +x /opt/scripts/hugo-deploy/redeploy.sh

# 3. 测试是否能够打印
[root@izwz97ekphk0kmqgt9zvc4z ~]# /opt/scripts/hugo-deploy/redeploy.sh
[root@izwz97ekphk0kmqgt9zvc4z ~]# cat /opt/scripts/hugo-deploy/redeploy.log

# 4. 创建hooks.json文件
[root@izwz97ekphk0kmqgt9zvc4z ~]# vi /opt/scripts/hugo-deploy/hooks.json
  [
    {
      "id": "redeploy-webhook",
      "execute-command": "/opt/scripts/hugo-deploy/redeploy.sh",
      "command-working-directory": "./"
    }
  ]

# 5. 启动webhook客户端
[root@izwz97ekphk0kmqgt9zvc4z ~]# /usr/setup/go/gopath/bin/webhook -hooks /opt/scripts/hugo-deploy/hooks.json -port=9876 -verbose

# 6. 开放9876端口，并测试
在阿里云安全组上放开9876端口，然后在浏览器中输入：http://<your-ip>:9876/hooks/redeploy-webhook
打印log文件

# 7. 创建服务
[root@izwz97ekphk0kmqgt9zvc4z ~]# cd /etc/systemd/system
[root@izwz97ekphk0kmqgt9zvc4z system]# vi webhook.service
  [Unit]
  Description=Webhooks

  [Service]
  ExecStart=/usr/setup/go/gopath/bin/webhook -hooks /opt/scripts/hugo-deploy/hooks.json -port=9876 -hotreload

  [Install]
  WantedBy=multi-user.target
[root@izwz97ekphk0kmqgt9zvc4z system]# systemctl enable webhook.service
[root@izwz97ekphk0kmqgt9zvc4z system]# systemctl start webhook.service

# 8. 测试

```
参考
> https://davidauthier.wearemd.com/blog/deploy-using-github-webhooks.html

---

## 安装Nginx

```shell
# 1. 创建安装目录
[root@izwz97ekphk0kmqgt9zvc4z ~]# mkdir /usr/setup/nginx_1.12.2
[root@izwz97ekphk0kmqgt9zvc4z ~]# mkdir /usr/setup/nginx_1.12.2/temp

# 2. 安装依赖
[root@izwz97ekphk0kmqgt9zvc4z ~]# yum install -y gcc-c++ pcre pcre-devel zlib zlib-devel openssl openssl-devel

# 3. 下载安装包并上传到/opt/package

# 4. 解压
[root@izwz97ekphk0kmqgt9zvc4z ~]# cd /opt/packages/
[root@izwz97ekphk0kmqgt9zvc4z packages]# tar zxf nginx-1.12.2.tar.gz
[root@izwz97ekphk0kmqgt9zvc4z packages]# cd nginx-1.12.2/

# 5. 配置
[root@izwz97ekphk0kmqgt9zvc4z nginx-1.12.2]# ./configure --prefix=/usr/setup/nginx_1.12.2 \
> --pid-path=/usr/setup/nginx_1.12.2/nginx.pid \
> --lock-path=/usr/setup/nginx_1.12.2/lock/nginx.lock \
> --error-log-path=/usr/setup/nginx_1.12.2/log/error.log \
> --http-log-path=/usr/setup/nginx_1.12.2/log/access.log \
> --http-client-body-temp-path=/usr/setup/nginx_1.12.2/temp/client \
> --http-proxy-temp-path=/usr/setup/nginx_1.12.2/temp/proxy \
> --http-fastcgi-temp-path=/usr/setup/nginx_1.12.2/temp/fastcgi \
> --http-uwsgi-temp-path=/usr/setup/nginx_1.12.2/temp/uwsgi \
> --http-scgi-temp-path=/usr/setup/nginx_1.12.2/temp/scgi \
> --with-http_stub_status_module \
> --with-http_gzip_static_module \
> --with-http_ssl_module

[root@izwz97ekphk0kmqgt9zvc4z nginx-1.12.2]# make
[root@izwz97ekphk0kmqgt9zvc4z nginx-1.12.2]# make install

# 6. 开放80端口，并进行测试
[root@izwz97ekphk0kmqgt9zvc4z nginx_1.12.2]# /usr/setup/nginx_1.12.2/sbin/nginx

# 7. 配置服务文件
[root@izwz97ekphk0kmqgt9zvc4z nginx_1.12.2]# cd /etc/systemd/system
[root@izwz97ekphk0kmqgt9zvc4z system]# vi nginx.service
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

[root@izwz97ekphk0kmqgt9zvc4z system]# systemctl enable nginx.service
[root@izwz97ekphk0kmqgt9zvc4z system]# systemctl start nginx.service

# 8. 启动Nginx，并进行测试

```
**扩展**：
1. 启动和关闭
```shell
// 切换到Nginx安装目录
$ cd /usr/setup/nginx_1.12.2/sbin

// 启动 （也可以加上 -C /usr/setup/nginx_1.12.2/config/nginx.conf 配置文件）
$ ./nginx

// 关闭
第一种方式 查找相关进程，然后kill进程
第二种方式 ./nginx -s stop
第三种方式 ./nginx -s quit
```


2. 关于centos7的系统服务命令

```shell
$ systemctl start nginx.service
$ systemctl enable nginx.service
$ systemctl disable nginx.service
$ systemctl status nginx.service
$ systemctl restart nginx.service
```

---
## 部署

1. 文件结构详解
hugo
  |- <username>.github.io
  |- <username>blogarticle
  |- <username>blogbase
  |- <username>blogbase

2. 在开发用的机子上面创建以上文件夹，并完成hugo的配置，然后将文件上传到github上面，总共是四个仓库。
3. 生成服务器的秘钥
```shell
[root@izwz97ekphk0kmqgt9zvc4z ~]# ssh-keygen -t rsa -C "438123371@qq.com"
```
4. 将/root/.ssh/id_rsa.pub文件中的字符串复制到GitHub添加sshkey的界面中

5. 创建仓库存放目录，并进入
```
[root@izwz97ekphk0kmqgt9zvc4z ~]# mkdir -p /home/project/hugoblog
[root@izwz97ekphk0kmqgt9zvc4z ~]# cd /home/project/hugoblog/
```

6. 克隆
```
[root@izwz97ekphk0kmqgt9zvc4z hugoblog]# git clone https://github.com/zeanzai/zeanzaiblogbase.git
[root@izwz97ekphk0kmqgt9zvc4z hugoblog]# git clone git@github.com:zeanzai/zeanzai.github.io.git
[root@izwz97ekphk0kmqgt9zvc4z hugoblog]# git clone https://github.com/zeanzai/zeanzaiblogtheme.git
[root@izwz97ekphk0kmqgt9zvc4z hugoblog]# git clone https://github.com/zeanzai/zeanzaiblogarticle.git

```

7. 配置Nginx
```shell
[root@izwz97ekphk0kmqgt9zvc4z hugoblog]# vi /usr/setup/nginx_1.12.2/conf/nginx.conf
  location / {
      root   /home/project/hugoblog/zeanzai.github.io;
      index  index.html index.htm;
  }
```

8. 编写钩子的执行文件.
关闭webhook服务，然后修改 `/opt/scripts/hugo-deploy/redeploy.sh` 文件
```shell
# 文件位置：/opt/scripts/hugo-deploy/redeploy.sh
#!/bin/bash

# 1. 开始
time1=`date "+%Y-%m-%d %H:%M:%S"` &&
echo "${time1} start ... " >> /opt/scripts/hugo-deploy/redeploy.log &&


# 2. 拉取GitHub的article
cd /home/project/hugoblog/zeanzaiblogarticle &&
git pull origin master &&
time2=`date "+%Y-%m-%d %H:%M:%S"` &&
echo "${time2} git pull article " >> /opt/scripts/hugo-deploy/redeploy.log &&

# 3. 删除io下面所有的文件
cd /home/project/hugoblog/zeanzai.github.io &&
rm -rf 404.html categories css favicon.ico img index.html index.xml js page post readme sitemap.xml tags &&
time3=`date "+%Y-%m-%d %H:%M:%S"` &&
echo "${time3} remove io " >> /opt/scripts/hugo-deploy/redeploy.log &&

# 4. 重新生成文件
cd /home/project/hugoblog/zeanzaiblogbase &&
hugo &&
time4=`date "+%Y-%m-%d %H:%M:%S"` &&
echo "${time4} deploy base " >> /opt/scripts/hugo-deploy/redeploy.log &&

# 5. 添加到缓存区中
cd /home/project/hugoblog/zeanzai.github.io &&
git add . &&
time5=`date "+%Y-%m-%d %H:%M:%S"` &&
echo "${time5} git add io " >> /opt/scripts/hugo-deploy/redeploy.log &&

# 6. 提交
git commit -m "update from aliyun" &&
time6=`date "+%Y-%m-%d %H:%M:%S"` &&
echo "${time6} git commit io " >> /opt/scripts/hugo-deploy/redeploy.log &&

# 7. push到GitHub
git push origin master &&
time7=`date "+%Y-%m-%d %H:%M:%S"` &&
echo "${time7} git push io " >> /opt/scripts/hugo-deploy/redeploy.log &&

# 8. 结束
time8=`date "+%Y-%m-%d %H:%M:%S"` &&
echo "${time8} ... end " >> /opt/scripts/hugo-deploy/redeploy.log &&
echo "" >> /opt/scripts/hugo-deploy/redeploy.log

```

9. 在article的GitHub仓库中配置GitHub钩子
10. 完成

参考：
> https://gohugo.io/hosting-and-deployment/hosting-on-github/#github-project-pages
> http://www.gohugo.org/post/coderzh-automated-deploy-hugo/
> https://blog.csdn.net/zmzwll1314/article/details/77678293
> https://blog.github.com/2016-02-01-working-with-submodules/
> https://davidauthier.wearemd.com/blog/deploy-using-github-webhooks.html
> https://blog.csdn.net/toyijiu/article/details/73611874
> https://git-scm.com/book/zh/v1/Git-%E5%9F%BA%E7%A1%80-%E8%AE%B0%E5%BD%95%E6%AF%8F%E6%AC%A1%E6%9B%B4%E6%96%B0%E5%88%B0%E4%BB%93%E5%BA%93

---

```shell
# 文件位置：/opt/scripts/hugo-deploy/redeploy.sh
#!/bin/bash

# 1. 开始
time1=`date "+%Y-%m-%d %H:%M:%S"`
start=`echo "${time1} start ... " >> /opt/scripts/hugo-deploy/redeploy.log`

# 2. 拉取GitHub的article
open_article=`cd /home/project/hugoblog/zeanzaiblogarticle`
gitpull_article=`git pull origin master`
time2=`date "+%Y-%m-%d %H:%M:%S"`
gitpull_article_log=`echo "${time2} git pull article " >> /opt/scripts/hugo-deploy/redeploy.log`

# 3. 删除io下面所有的文件
open_io=`cd /home/project/hugoblog/zeanzai.github.io`
rm_io=`rm -rf 404.html categories css favicon.ico img index.html index.xml js page post readme sitemap.xml tags`
time3=`date "+%Y-%m-%d %H:%M:%S"`
rm_log=`echo "${time3} remove io " >> /opt/scripts/hugo-deploy/redeploy.log`

# 4. 重新生成文件
open_base=`cd /home/project/hugoblog/zeanzaiblogbase`
base_deploy=`hugo`
time4=`date "+%Y-%m-%d %H:%M:%S"`
base_deploy_log=`echo "${time4} deploy base " >> /opt/scripts/hugo-deploy/redeploy.log`

# 5. 添加到缓存区中
open_io_again=`cd /home/project/hugoblog/zeanzai.github.io`
io_add=`git add .`
time5=`date "+%Y-%m-%d %H:%M:%S"`
io_add_log=`echo "${time5} git add io " >> /opt/scripts/hugo-deploy/redeploy.log`

# 6. 提交
io_commit=`git commit -m "update from aliyun"`
time6=`date "+%Y-%m-%d %H:%M:%S"`
io_commit_log=`echo "${time6} git commit io " >> /opt/scripts/hugo-deploy/redeploy.log`

# 7. push到GitHub
io_push=`git push origin master`
time7=`date "+%Y-%m-%d %H:%M:%S"`
io-push_log=`echo "${time7} git push io " >> /opt/scripts/hugo-deploy/redeploy.log`

# 8. 结束
time8=`date "+%Y-%m-%d %H:%M:%S"`
end=`echo "${time8} ... end " >> /opt/scripts/hugo-deploy/redeploy.log && echo "" >> /opt/scripts/hugo-deploy/redeploy.log

if [ -z "$start" ];then #判断RESULT是否为空，为空则说明进程未启动
  command2
  exit
fi


```












##
