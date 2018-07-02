
# install vsftpd
```shell
$ yum install -y vsftpd
```

# add user
add user and passwor it
```shell
$ useradd ftpuser
$ passwd ftpuser
```

# open 21 port
```shell
$ firewall-cmd --zone=public --add-port=21/tcp --permanent
$ firewall-cmd --reload
```

# config selinux
```shell
$ getsebool -a | grep ftp
ftpd_anon_write --> off
ftpd_connect_all_unreserved --> off
ftpd_connect_db --> off
ftpd_full_access --> off
ftpd_use_cifs --> off
ftpd_use_fusefs --> off
ftpd_use_nfs --> off
ftpd_use_passive_mode --> off
httpd_can_connect_ftp --> off
httpd_enable_ftp_server --> off
tftp_anon_write --> off
tftp_home_dir --> off

$ setsebool -P allow_ftpd_full_access on
$ setsebool -P tftp_home_dir on
```

# config vsftpd.conf
/etc/vsftpd/vsftpd.conf YES->NO
```shell
anonymous_enable=NO
```

# enable autorun
```shell
$ systemctl enable vsftpd
```

# install nginx
refer to [install nginx and config https](https://github.com/zeanzai/Computer-Science-Study-Note/blob/master/operation/install-nginx-and-config-https.md)

# config image root document
```shell
$ cd /home/ftpuser/
$ mkdir /home/ftpuser/www
$ mkdir /home/ftpuser/www/images
$ chmod -R 777 /home/ftpuser
```

# config nginx
```shell
server {
        listen       80;
        server_name  image.beidiancloud.com;

        location / {
            root   /home/ftpuser/www;
            index  index.html index.htm;
        }
    }
```
# config hosts
file loaction: C:\Windows\System32\drivers\etc

`192.168.100.203 image.beidiancloud.com`

# test
use filezilla to upload some image to `/home/ftpuser/www/images`

succeed!


# remark
```shell
// list the port opened
$ firewall-cmd --list-ports

// open 80 port
$ firewall-cmd --zone=public(作用域) --add-port=80/tcp(端口和访问类型) --permanent(永久生效)

// reload firewall
$ firewall-cmd --reload

// stop firewall
$ systemctl stop firewalld.service

// disable firewalld autorun after boot operate system 
$ systemctl disable firewalld.service

// enable firewall autorun after boot operate system
$ systemctl enable firewalld.service

// delete some port
$ firewall-cmd --zone= public --remove-port=80/tcp --permanent
```
# reference
> 1. CentOS7 安装vsftpd 服务器 https://blog.csdn.net/uq_jin/article/details/51684722
> 2. Centos7,配置防火墙，开启端口 https://blog.csdn.net/u013410747/article/details/61696178
> 3. SELinux百度百科： https://baike.baidu.com/item/SELinux/8865268?fr=aladdin
> 4. 【Linux/CentOS】Boolean ftp_home_dir is not defined https://www.cnblogs.com/guxin/p/centos-boolean-ftp-home-dir-is-not-defined.html



