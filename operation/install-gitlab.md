
# 1. 更新yum源
* 备份原有的yum源
```shell
[root@vm yum.repos.d]# mv CentOS-Base.repo  CentOS-Base.repo.bak_20180611
```
* 拷贝到目录下
```shell
[root@vm yum.repos.d]# mv /opt/program/Centos-7.repo ./CentOS-Base.repo
```
* 删除原有yum缓存
```shell
[root@vm yum.repos.d]# yum clean all
已加载插件：fastestmirror
正在清理软件源： base extras updates
Cleaning up everything
Maybe you want: rm -rf /var/cache/yum, to also free up space taken by orphaned data from disabled or removed repos
Cleaning up list of fastest mirrors
```
* 重新生成yum缓存
```shell
[root@vm yum.repos.d]# yum makecache
已加载插件：fastestmirror
.....
元数据缓存已建立
```

# 2. 安装gitlab
## 2.1 安装依赖软件
```shell
[root@vm ~]# yum -y install policycoreutils openssh-server openssh-clients postfix
```

## 2.2 设置postfix开机自启，并启动，postfix支持gitlab发信功能
```shell
[root@vm yum.repos.d]# systemctl enable postfix && systemctl start postfix
```

## 2.3 上传
前往[下载地址](https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el7)下载`gitlab-ce-10.8.4-ce.0.el7.x86_64.rpm`，下载完成后上传到/opt/program/ 目录下面。

```shell
[root@vm yum.repos.d]# cd /opt/program/
[root@vm program]# ll
总用量 410428
-rw-r--r--. 1 root root 420274392 6月  11 22:40 gitlab-ce-10.8.4-ce.0.el7.x86_64.rpm
[root@vm program]# rpm -ivh gitlab-ce-10.8.4-ce.0.el7.x86_64.rpm 
警告：gitlab-ce-10.8.4-ce.0.el7.x86_64.rpm: 头V4 RSA/SHA1 Signature, 密钥 ID f27eab47: NOKEY
错误：依赖检测失败：
        policycoreutils-python 被 gitlab-ce-10.8.4-ce.0.el7.x86_64 需要
[root@vm program]# yum install policycoreutils-python
[root@vm program]# rpm -ivh gitlab-ce-10.8.4-ce.0.el7.x86_64.rpm 
警告：gitlab-ce-10.8.4-ce.0.el7.x86_64.rpm: 头V4 RSA/SHA1 Signature, 密钥 ID f27eab47: NOKEY
准备中...                          ################################# [100%]
正在升级/安装...
   1:gitlab-ce-10.8.4-ce.0.el7        ################################# [100%]
It looks like GitLab has not been configured yet; skipping the upgrade script.

       *.                  *.
      ***                 ***
     *****               *****
    .******             *******
    ********            ********
   ,,,,,,,,,***********,,,,,,,,,
  ,,,,,,,,,,,*********,,,,,,,,,,,
  .,,,,,,,,,,,*******,,,,,,,,,,,,
      ,,,,,,,,,*****,,,,,,,,,.
         ,,,,,,,****,,,,,,
            .,,,***,,,,
                ,*,.
  


     _______ __  __          __
    / ____(_) /_/ /   ____ _/ /_
   / / __/ / __/ /   / __ `/ __ \
  / /_/ / / /_/ /___/ /_/ / /_/ /
  \____/_/\__/_____/\__,_/_.___/
  

Thank you for installing GitLab!
GitLab was unable to detect a valid hostname for your instance.
Please configure a URL for your GitLab instance by setting `external_url`
configuration in /etc/gitlab/gitlab.rb file.
Then, you can start your GitLab instance by running the following command:
  sudo gitlab-ctl reconfigure

For a comprehensive list of configuration options please see the Omnibus GitLab readme
https://gitlab.com/gitlab-org/omnibus-gitlab/blob/master/README.md

```

## 2.4 修改gitlab配置文件


```shell
[root@vm sysconfig]# vi /etc/gitlab/gitlab.rb

// 指定服务器ip和自定义端口
external_url 'http://192.168.100.200:8001'

// 关闭SMTP，开启postfix
gitlab_rails['smtp_enable'] = false
```

## 2.5 修改防火墙
```shell
[root@vm sysconfig]# firewall-cmd --zone=public --add-port=8001/tcp --permanent 
success
[root@vm sysconfig]# firewall-cmd --reload
success
```

## 2.6 重置并启动
```shell
[root@vm program]# gitlab-ctl reconfigure
[root@vm program]# gitlab-ctl restart
```

## 2.7 访问gitlab页面
浏览器中输入IP：PORT
初始化账户root 密码：5iveL!fe
设置为root1003

# 3. gitlab的简单使用


> 参考资料
1. https://www.cnblogs.com/wenwei-blog/p/5861450.html
2. https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el7/
3. https://www.gitlab.com.cn/installation/#centos-7
4. https://github.com/judasn/Linux-Tutorial/blob/master/markdown-file/Gitlab-Install-And-Settings.md



