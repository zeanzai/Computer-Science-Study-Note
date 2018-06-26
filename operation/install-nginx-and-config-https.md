
# 1. install dependencies
```shell
$ yum install -y gcc-c++ pcre pcre-devel zlib zlib-devel openssl openssl-devel
```

# 2. make dir
```shell
$ mkdir /usr/setup
$ mkdir /opt/package
$ mkdir /usr/setup/nginx
$ mkdir /usr/setup/nginx/temp
```

# 3. download and upload
download tar resource file 'nginx-1.12.2.tar.gz' 

upload to '/opt/package/'


# 4. intall 
## 4.1 release this tar file
```shell
$ tar zxf nginx-1.12.2.tar.gz
$ cd nginx-1.12.2/
```

## 4.2 install
```shell
$ ./configure --prefix=/usr/setup/nginx --with-http_stub_status_module --with-http_gzip_static_module --with-http_ssl_module --pid-path=/usr/setup/nginx/nginx.pid --lock-path=/usr/setup/nginx/lock/nginx.lock --error-log-path=/usr/setup/nginx/log/error.log --http-log-path=/usr/setup/nginx/log/access.log --http-client-body-temp-path=/usr/setup/nginx/temp/client --http-proxy-temp-path=/usr/setup/nginx/temp/proxy --http-fastcgi-temp-path=/usr/setup/nginx/temp/fastcgi --http-uwsgi-temp-path=/usr/setup/nginx/temp/uwsgi --http-scgi-temp-path=/usr/setup/nginx/temp/scgi
$ make
$ make install
```

# 5. run and test
```shell
$ cd /usr/setup/nginx/sbin
$ ./nginx
$ ps aux | grep nginx
```

# 6. config HTTPS
## 6.0 resolve doamain name
before start to config https, we should to resolve the doamain name to our ip

## 6.1 make dir
```shell
$ mkdir /usr/setup/nginx/html/sys
$ mkdir /usr/setup/nginx/conf/cert
$ mkdir /usr/setup/nginx/conf/cert/sys
```

## 6.2 upload certfiles
upload cert files to '/usr/setup/nginx/conf/cert/sys/'

## 6.3. config
add 2 servers to 'nginx.conf' file
```shell
server {  
    listen  80;  
    server_name your.doamain.name;   
    rewrite ^(.*)$  https:#$host$1 permanent;  
} 
server {
     listen       443 ssl;
     server_name  your.doamain.name;

     ssl_certificate      cert/sys/your.doamain.name_ca.crt;
     ssl_certificate_key  cert/sys/your.doamain.name.key;
     ssl_session_cache    shared:SSL:1m;
     ssl_session_timeout  5m;       
     ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
     ssl_ciphers ALL:!DH:!EXPORT:!RC4:+HIGH:+MEDIUM:!LOW:!aNULL:!eNULL;
    
     location / {
         root   html/sys;
         index  index.html index.htm;
     }
}
```

## 6.4. upload websites files
upload websites files to '/usr/setup/nginx/html/sys/'

## 6.5. test 
reload nginx
```shell
$ ./usr/setup/nginx/sbin/nginx -s reload
```
open brower and type 'your.doamain.name' and enter to test the websites

# 7. config autorun 
## 7.1 create config service file
create nginx.service and add the following string 
```shell
$ vi /lib/systemd/system/nginx.service
[Unit]
Description=nginx
After=network.target

[Service]
Type=forking
ExecStart=/usr/setup/nginx/sbin/nginx
ExecReload=/usr/setup/nginx/sbin/nginx -s reload
ExecStop=/usr/setup/nginx/sbin/nginx -s quit
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

## 7.2. enable config file
```shell
$ systemctl enable nginx.service
```

## 7.3. test 
reboot
```shell
$ reboot
```
open brower and type 'your.doamain.name' and enter to test the websites


# 8. other run orders
```shell
$ systemctl status nginx.service
$ systemctl start nginx.service
$ systemctl disable nginx.service
$ systemctl restart nginx.service
$ systemctl list-units --type=service
```

