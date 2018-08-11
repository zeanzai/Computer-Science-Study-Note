老师，您好！

不知道怎么称呼您，姑且称呼您为老师吧，前些时候，我看到您的博客上看到关于谈使用Jekyll上传到GitHub，然后再由腾讯云服务器定时从GitHub上拉取增量文件后，执行Jekyll build等命令部署静态文件到nginx上，实现自动化部署的文章后，我很受启发。于是我决定自己也实验一下，我就执行了下面操作：

1. 在阿里云服务器上面安装了nginx、ruby、gem、Jekyll、git基础环境；
2. 克隆GitHub上面的项目到云服务器的指定目录；
3. 执行Jekyll build命令生成静态文件，配置nginx的目录为生成的_sit文件目录；
4. 启动nginx，测试访问：成功；

这一切都很顺利，然而就是在我使用云服务器自带的定时器设置定时器任务时，出现问题了，我在网上查了资料了，可是怎么都找不到，我猜应该是Jekyll的环境变量问题。我几乎用尽了从百度和谷歌上面搜到的解决方式，但是还是毫无进展，遂向老师请求帮助！谢谢老师的帮助。


下面这些信息您可能会需要：
```shell
# ruby -v
ruby 2.5.1p57 (2018-03-29 revision 63029) [x86_64-linux]
# gem -v
2.7.7
# jekyll -v
jekyll 3.8.3
# cat /etc/crontab
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin:/usr/local/rvm/gems/ruby-2.5.1/bin:/usr/local/rvm/rubies/ruby-2.5.1/lib/ruby/gems/2.5.0/bin:/usr/local/rvm/rubies/ruby-2.5.1/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/usr/local/rvm/bin:/usr/setup/git_2.18.0/bin:/root/bin:/usr/local/rvm/rubies/ruby-2.5.1/lib/ruby/gems/2.5.0
MAILTO=root

# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed
45 18 * * * root /bin/sh /opt/script/jekyll.sh >> /home/jekyll.log 2>&1
# cat /opt/script/jekyll.sh
#!/bin/sh
echo 'start-----------------------------------------------'
date "+%Y-%m-%d %H:%M:%S"

echo '[1] : open the dir...'
cd /home/resource/open/zeanzai.github.io

echo '[2] : excute git pull...'
/usr/setup/git_2.18.0/bin/git pull

source /usr/local/rvm/rubies/ruby-2.5.1/lib/ruby/gems/2.5.0/environment
echo '[3] : excute jekyll build...'
jekyll build

echo '[4] : reload nginx...'
systemctl reload nginx
echo '----------------------------------------------- end'
# cat /home/jekyll.log
start-----------------------------------------------
2018-08-11 18:45:01
[1] : open the dir...
[2] : excute git pull...
Already up to date.
[3] : excute jekyll build...
/usr/local/rvm/rubies/ruby-2.5.1/lib/ruby/site_ruby/2.5.0/rubygems.rb:289:in `find_spec_for_exe': can't find gem jekyll (>= 0.a) with executable jekyll (Gem::GemNotFoundException)
	from /usr/local/rvm/rubies/ruby-2.5.1/lib/ruby/site_ruby/2.5.0/rubygems.rb:308:in `activate_bin_path'
	from /usr/local/rvm/gems/ruby-2.5.1/bin/jekyll:23:in `<main>'
	from /usr/local/rvm/gems/ruby-2.5.1@global/bin/ruby_executable_hooks:24:in `eval'
	from /usr/local/rvm/gems/ruby-2.5.1@global/bin/ruby_executable_hooks:24:in `<main>'
[4] : reload nginx...
----------------------------------------------- end
```