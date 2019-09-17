---
layout: post
title:  "Docker学习笔记"
date:   2018-06-05 12:58:00 +0800
author: zeanzai
tags:
    - study
    - note
---
# 1. 基础知识

## 1.1 Docker容器和镜像
> 镜像构建时，会一层层构建，前一层是后一层的基础。因此在构建镜像的时候，需要小心，每一层尽量只包含该层所需要添加的东西，任何额外的东西都应该在构建结束前清理掉。
> 镜像与容器的关系，就像面向对象程序设计中的类和对象的关系。
> 镜像使用分层存储，容器也是如此。容器运行时，会创建一个容器存储层。
最佳实践要求，容器不应该向其存储层写入任何数据，应该保持无状态化。所有文件的写操作，都应该使用数据卷、或绑定宿主目录。也就是对宿主（或网络存储）直接发生读写。

## 1.2 DockerRegistry
每个DockerRegistry包含多个Repository（仓库）；
每个Repository包含多个（Tag）标签；标签相当于软件的版本号；latest为默认标签
每个Tag对应一个镜像
公开的Registry服务是：DockerHub，也是默认的Registry。

## 1.3 Docker版本命名规则
分为
	社区版（CE免费，支持周期三个月）
	企业版（EE，付费使用）
版本格式： YY.MM
Stable版本： 每个季度发行
Edger版本： 每个月发行


# 2. CentOS安装DockerCE版本

## 2.1 环境要求：
支持64位，但要求内核版本不低于3.10。
```
[root@studyCentOS7 ~]# uname -r
3.10.0-693.5.2.el7.x86_64
```

## 2.2 卸载旧版本
```
[root@studyCentOS7 ~]#   yum remove docker \
>     docker-client \
>     docker-client-latest \
>     docker-common \
>     docker-latest \
>     docker-latest-logrotate \
>     docker-logrotate \
>     docker-selinux \
>     docker-engine-selinux \
>     docker-engine
已加载插件：fastestmirror
参数 docker 没有匹配
参数 docker-client 没有匹配
参数 docker-client-latest 没有匹配
参数 docker-common 没有匹配
参数 docker-latest 没有匹配
参数 docker-latest-logrotate 没有匹配
参数 docker-logrotate 没有匹配
参数 docker-selinux 没有匹配
参数 docker-engine-selinux 没有匹配
参数 docker-engine 没有匹配
不删除任何软件包
```

## 2.3 更新yum源并配置要安装的版本号
```
[root@studyCentOS7 ~]# yum-config-manager --add-repo https://mirrors.ustc.edu.cn/docker-ce/linux/centos/docker-ce.repo
已加载插件：fastestmirror
adding repo from: https://mirrors.ustc.edu.cn/docker-ce/linux/centos/docker-ce.repo
grabbing file https://mirrors.ustc.edu.cn/docker-ce/linux/centos/docker-ce.repo to /etc/yum.repos.d/docker-ce.repo
repo saved to /etc/yum.repos.d/docker-ce.repo

[root@studyCentOS7 ~]# yum-config-manager --enable docker-ce-edge
已加载插件：fastestmirror
======================================================= repo: docker-ce-edge =======================================================
[docker-ce-edge]
async = True
bandwidth = 0
base_persistdir = /var/lib/yum/repos/x86_64/7
baseurl = https://download.docker.com/linux/centos/7/x86_64/edge
cache = 0
cachedir = /var/cache/yum/x86_64/7/docker-ce-edge
check_config_file_age = True
compare_providers_priority = 80
cost = 1000
deltarpm_metadata_percentage = 100
deltarpm_percentage = 
enabled = 1
enablegroups = True
exclude = 
failovermethod = priority
ftp_disable_epsv = False
gpgcadir = /var/lib/yum/repos/x86_64/7/docker-ce-edge/gpgcadir
gpgcakey = 
gpgcheck = True
gpgdir = /var/lib/yum/repos/x86_64/7/docker-ce-edge/gpgdir
gpgkey = https://download.docker.com/linux/centos/gpg
hdrdir = /var/cache/yum/x86_64/7/docker-ce-edge/headers
http_caching = all
includepkgs = 
ip_resolve = 
keepalive = True
keepcache = False
mddownloadpolicy = sqlite
mdpolicy = group:small
mediaid = 
metadata_expire = 21600
metadata_expire_filter = read-only:present
metalink = 
minrate = 0
mirrorlist = 
mirrorlist_expire = 86400
name = Docker CE Edge - x86_64
old_base_cache_dir = 
password = 
persistdir = /var/lib/yum/repos/x86_64/7/docker-ce-edge
pkgdir = /var/cache/yum/x86_64/7/docker-ce-edge/packages
proxy = False
proxy_dict = 
proxy_password = 
proxy_username = 
repo_gpgcheck = False
retries = 10
skip_if_unavailable = False
ssl_check_cert_permissions = True
sslcacert = 
sslclientcert = 
sslclientkey = 
sslverify = True
throttle = 0
timeout = 30.0
ui_id = docker-ce-edge/x86_64
ui_repoid_vars = releasever,
   basearch
username = 

```


## 2.4 使用yum安装
 - 使用yum安装
 ```
 [root@studyCentOS7 ~]# yum makecache fast
 [root@studyCentOS7 ~]# yum install docker-ce
 ```

 - 使用脚本安装（未做实验）
 ```
 curl -fsSL get.docker.com -o get-docker.sh
 sh get-docker.sh --mirror Aliyun
 ```

## 2.5 启动
 ```
[root@studyCentOS7 ~]# systemctl enable docker
Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service.
[root@studyCentOS7 ~]# systemctl start docker 
 ```

## 2.6 建立docker用户组
 ```
[root@studyCentOS7 ~]# groupadd docker
[root@studyCentOS7 ~]# usermod -aG docker $USER
 ```
## 2.7 测试docker是否安装正确
```
[root@studyCentOS7 ~]# docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
9bb5a5d4561a: Pull complete 
Digest: sha256:f5233545e43561214ca4891fd1157e1c3c563316ed8e237750d59bde73361e77
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/engine/userguide/

```
## 2.8 配置镜像加速
```

[root@studyCentOS7 ~]# cd /etc/docker/
[root@studyCentOS7 docker]# ls
key.json
[root@studyCentOS7 docker]# vi daemon.json	// 创建文件
{
"registry-mirrors": [
"https://registry.docker-cn.com"
]
}
[root@studyCentOS7 docker]# systemctl daemon-reload
[root@studyCentOS7 docker]# systemctl restart docker

```

## 2.9 添加内核参数
如果出现
```
WARNING: bridge-nf-call-iptables is disabled
WARNING: bridge-nf-call-ip6tables is disabled
```
则： 
tee -a /etc/sysctl.conf <<-EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF

# 3 使用Docker镜像
## 3.1 从仓库中获取镜像

docker pull [选项] [Docker Registry 地址[:端口号]/]仓库名[:标签]

docker 镜像仓库地址：格式为<域名/IP>[:端口号]，默认是Docker Hub
仓库名： 格式为：<用户名>/<软件名>， 对于dockerhub，如果不给出用户名，则默认为：library。

> 镜像体积
docker image ls命令显示出来的结果：
包含了仓库名、标签、镜像id、创建时间、所占的空间

DockerHub上面显示的是压缩后的体积，镜像在下载和上传过程中都是保持压缩状态的。
上述命令显示出来的体积是镜像下载到本地后展开的大小。
又因为不同的镜像可能会有相同的基础镜像，从而拥有共同的层。所以，实际镜像硬盘占用的空间可能比上述命令展示出来的总和要小。

> 造成虚悬镜像的原因：
新版本使用旧版本镜像而导致的旧版本镜像的名称被取消从而变成<none>的镜像就是虚悬镜像。

> 中间层镜像
docker采用的是上层镜像依赖下层镜像。所以使用docker image ls -a显示出来的是包括中间层镜像在内的所有的镜像。

> 列出部分镜像
docker image ls ubuntu
docker image ls ubuntu:16.04

> 删除本地镜像
docker image rm [选项] <镜像1>[<镜像2>...]
eg: docker image rm 501 
其中501是id的前三个字符

## 3.2 管理主机上的镜像

## 3.3 镜像实现的基本原理

​	 






