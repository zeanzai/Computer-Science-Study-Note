# 运维手册

## 前言
良好的运维习惯有很多优点：
- 利于自己维护，利于后继者维护
- 对计算机服务器进行最小化改动
- 便于实现对计算机更好的管理

## 良好的习惯
笔者工作使用的电脑是window平台，所以使用Windows平台下的软件工具连接到远程服务器上进行相对应的操作。下面是笔者的一些工作习惯：

- 使用filezilla工具进行上传
- 使用SecureCRT工具进行连接
- 使用editplus编辑较为复杂的文本文件
- 在`/opt/package/`目录下面上传安装包
- 在`/usr/setup/`目录下面安装软件
- 每次对服务器进行修改时，都会创建后缀名为md的相对应的维护日志文件
- 在`/home/log/`目录下面放置维护日志文件，以日期（yyyyMMdd）+操作名称（eg: install-nginx.md）命名
- 在`/home/history/`目录下面备份history命令，以日期（yyyyMMdd）命名
- 安装依赖时，会优先使用yum install方式进行安装，在yum库中没有的相关依赖时才会使用源码形式安装
- 创建非root管理员进行操作

## 如何写维护日志
维护日志需要包括以下内容：

- 日志文件名称，如：安装Nginx、配置HTTPS、安装jdk、修改Tomcat端口
- 维护时间，格式为： yyyyMMdd hh:mm
- 维护人姓名，但笔者一般使用汉语拼音简称
- 维护内容，包括：操作命令记录过程

下面是一个例子：
```
---
file:		install-nginx.md
datetime:	20180621 16:43 
operator:	zeanzai 
operation:	install nginx
---
# install nginx

## make it ready
balabala

## install dependencies
balabala

## install Nginx
### download and upload
balabala
### release resource
balabala
### config and install
balabala
### start
balabala

## test
balabala

## remark
balabala
```



