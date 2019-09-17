# ruby环境安装
## 导论

## 环境
ruby:
* [ruby下载地址](https://rubyinstaller.org/downloads/)
* 下载版本： `rubyinstaller-devkit-2.5.1-1-x64.exe`

msys2:
* [msys2下载地址](http://www.msys2.org/)
* 下载版本：`msys2-x86_64-20180531.exe`

## 安装步骤

* ruby安装

![](https://jekyll-github.oss-cn-shenzhen.aliyuncs.com/jekyll-install-guidence/001.png)

![](https://jekyll-github.oss-cn-shenzhen.aliyuncs.com/jekyll-install-guidence/002.png)

![](https://jekyll-github.oss-cn-shenzhen.aliyuncs.com/jekyll-install-guidence/003.png)

* msys2安装

![](https://jekyll-github.oss-cn-shenzhen.aliyuncs.com/jekyll-install-guidence/006.png)

![](https://jekyll-github.oss-cn-shenzhen.aliyuncs.com/jekyll-install-guidence/007.png)

* 执行ridk install命令

![](https://jekyll-github.oss-cn-shenzhen.aliyuncs.com/jekyll-install-guidence/009.png)
此过程无比慢。

## 测试

![](https://jekyll-github.oss-cn-shenzhen.aliyuncs.com/jekyll-install-guidence/004.png)

![](https://jekyll-github.oss-cn-shenzhen.aliyuncs.com/jekyll-install-guidence/008.png)

## 修改gem源
```shell
C:\Users\shawnvong>gem sources --remove https://rubygems.org/
https://rubygems.org/ removed from sources

C:\Users\shawnvong>gem sources -a https://ruby.taobao.org/
https://ruby.taobao.org/ added to sources

C:\Users\shawnvong>gem sources -l
*** CURRENT SOURCES ***

https://ruby.taobao.org/
```

## 安装jekyll
```
C:\Users\shawnvong>gem install jekyll
C:\Users\shawnvong>jekyll -v
jekyll 3.8.2
```

## 安装bundler依赖
```shell
D:\develop\resource\blog\zeanzai.github.io>gem install bundler
Fetching: bundler-1.16.2.gem (100%)
Successfully installed bundler-1.16.2
Parsing documentation for bundler-1.16.2
Installing ri documentation for bundler-1.16.2
Done installing documentation for bundler after 8 seconds
1 gem installed

D:\develop\resource\blog\zeanzai.github.io>bundler -v
Bundler version 1.16.2

D:\develop\resource\blog\zeanzai.github.io>bundle install
```

## 从GitHub上面克隆

![](https://jekyll-github.oss-cn-shenzhen.aliyuncs.com/jekyll-install-guidence/011.png)

## 启动

![](https://jekyll-github.oss-cn-shenzhen.aliyuncs.com/jekyll-install-guidence/012.png)

![](https://jekyll-github.oss-cn-shenzhen.aliyuncs.com/jekyll-install-guidence/013.png)

## 新建博客

![](https://jekyll-github.oss-cn-shenzhen.aliyuncs.com/jekyll-install-guidence/014.png)


## 推送到GitHub上

![](https://jekyll-github.oss-cn-shenzhen.aliyuncs.com/jekyll-install-guidence/015.png)

## 测试

![](https://jekyll-github.oss-cn-shenzhen.aliyuncs.com/jekyll-install-guidence/016.png)


> 参考链接
1. [Jekyll搭建个人博客]( http://baixin.io/2016/10/jekyll_tutorials1/)
2. [在windows下安装jekyll（一个很好地博客系统） - 简书](https://www.jianshu.com/p/88e3474cef72)
3. [快速在 Windows 上搭建 Jekyll 开发环境](http://www.itboth.com/d/vM3Qjy/windows-jekyll)
4. [Windows下安装Ruby和Jekyll](http://www.itboth.com/d/vEniYb/windows-devkit-jekyll-rouge-github)






