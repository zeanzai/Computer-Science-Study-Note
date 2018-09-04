
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

## 安装hugo
[root@iZwz97ekphk0kmqgt9zvc4Z git-2.18.0]# go get -u -v github.com/spf13/hugo
