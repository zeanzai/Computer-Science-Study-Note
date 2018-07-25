---
layout: post
title:  "Jekyll Install Turial"
date:   2018-06-04 18:31:27 +0800
author: zeanzai
tags:
    - jekyll
    - blog
---


# 安装过程


## 下载ruby安装软件
[下载地址](https://rubyinstaller.org/downloads/)

## 双击安装
略

## 测试
```
C:\Users\northmeter>ruby -v
ruby 2.5.1p57 (2018-03-29 revision 63029) [x64-mingw32]

C:\Users\northmeter>gem -v
2.7.6

C:\Users\northmeter>jekyll -v
jekyll 3.8.2
```

## 安装jekyll
```
$ gem install jekyll     
```

# 创建本地博客

## 在自定义目录中初始化博客
```
$ jekyll new zeanzai.github.io
```

## 启动并测试
```
$ cd zeanzai.github.io
$ jekyll server
```
浏览器中打开`http://127.0.0.1:4000/`。

## 创建新文章
在 `_post`文件夹内创建 `2018-06-04-first-test.md` 文件。刷新浏览器即可查看效果。
> jekyll 中的文件命名方式为:  `yyyy-MM-dd-your-title.md` 或 `2018-06-04-first-test.markdown`。

# 上传到GitHub上

## 创建仓库
在GitHub上面创建`zeanzai.github.io`仓库。

## 上传到GitHub上面
```shell
// 进入上一个步骤生成的文件夹内
$ cd zeanzai.github.io/

// 执行初始化
$ git init

// 将所有的文件添加到暂存区中
$ git add .

// 执行提交操作
$ git commit -m "初始化导入"

// 添加仓库地址
$ git remote add origin https://github.com/zeanzai/zeanzai.github.io.git

// push到远程仓库中， 在此过程中可能需要多次验证用户名和密码
$ git push -u origin master

$ git push -u origin master
```


## 测试
在浏览器中输入： https://zeanzai.github.io/
done。

## 使用sourcetree进行导入
以后就可以使用sourcetree软件进行上传和下载。

# 多终端同步

## 安装环境

## 浏览器中打开




# 拷贝别人的博客
寻找一份别人的jekyll博客，要是开源的，最好是部署到GitHub上面的。然后下载下来，当然也可以克隆下来。下载完成之后解压到自定义文件夹中，自定义文件夹路径中不能包含中文。打开cmd并切换到该目录，执行`jekyll server`命令，最后浏览器中打开`http://localhost:4000/`，便可以看到别人博客的效果。

<br />
参考资料
> 
1. [windows安装jekyll步骤及问题 - CSDN博客](  https://blog.csdn.net/mouday/article/details/79300135)
2. [jekyll 完整安装教程 - CSDN博客]( https://blog.csdn.net/joelcat/article/details/78642434)
3. [Jekyll搭建个人博客]( http://baixin.io/2016/10/jekyll_tutorials1/)
4. [如何快速给自己构建一个温馨的"家"——用Jekyll搭建静态博客 - 简书]( https://www.jianshu.com/p/9a6bc31d329d)
5. [欢迎]( https://www.jekyll.com.cn/docs/home/)






