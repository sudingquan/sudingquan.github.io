---
title: 用chevereto来搭建自己的图床
date: 2019-05-10 11:27:20
tags: ["图床"]
categories:
  - "linux"
---

## 说明

前段时间买了一个阿里云的学生服务器，之前一直用作学习linux的工具。最近写markdown文档的时候发现自己缺一个图床，于是我就想到了在自己的服务器上搭建一个图床。通过搜索，我找到了一个能在服务器上方便搭建图床的工具——`chevereto`。



`chevereto`是一个功能非常强大的图床，而`chevereto`的安装也非常简单，并且支持中文，可以上传本地图片或通过url上传图片。效果如下

***官网：*** [https://chevereto.com/](https://chevereto.com/)
<!-- more -->

![chevereto](http://39.105.82.248/images/2019/05/10/-2019-05-10-12.50.38.png)


## 安装

**环境要求：**

+ `Apache`/`Nginx`
+	`PHP 5.5+`
+	`MySQL 5.0+`

1. 搭建web环境

   可以使用`lnmp`来一键搭建web环境，然而安装lnmp的时间可能会很长，我们在它执行完毕之前，不能关掉窗口或者断开连接，否则这个任务就会被杀掉，一切半途而废了。为了避免这种情况，我们可以使用screen。

   **GNU Screen**是一款由GNU计划开发的用于命令行终端切换的自由软件。用户可以通过该软件同时连接多个本地或远程的命令行会话，并在其间自由切换。

   在登录服务器后，运行

   ```bash
   screen -S lnmp
   ```

   如果提示`screen: command not found` 命令不存在可以执行：`yum install screen` 或 `apt-get install screen`安装

   然后安装`lnmp`

   lnmp一键包安装地址：[https://lnmp.org/install.html](https://lnmp.org/install.html)

   ```bash
   wget http://soft.vpser.net/lnmp/lnmp1.5.tar.gz -cO lnmp1.5.tar.gz && tar zxf lnmp1.5.tar.gz && cd lnmp1.5 && ./install.sh lnmp
   ```

2. 添加网站

   ```bash
   lnmp vhost add
   ```

   按步骤填写，可参考：[https://lnmp.org/faq/lnmp-vhost-add-howto.html](https://lnmp.org/faq/lnmp-vhost-add-howto.html)

3. 下载`chevereto`

   `chevereto`下载地址：[https://github.com/Chevereto/Chevereto-Free](https://github.com/Chevereto/Chevereto-Free)

   切换到你的网站的目录

   ```bash
   cd /home/wwwroot/你的网站名
   ```

   执行命令

   ```bash
   wget https://github.com/Chevereto/Chevereto-Free/archive/1.1.3.tar.gz
   ```

   然后解压文件

   ```bash
   tar zfvx 1.1.3.tar.gz
   ```

   将解压的文件复制到网站根目录,进入解压好目录，执行

   ```bash
   cp -r * ..
   ```

   然后赋予目录相应的权限

   ```bash
   chmod -R 777 ./*
   ```

   最后进入网站的配置文件

   ```bash
   cd /usr/local/nginx/conf/vhost/你的网站名.conf
   ```

   在`server`中加入以下代码

   ```
   location / {
   try_files $uri $uri/ /index.php?$query_string;
   }
   ```

   然后重启Nginx

   ```bash
   /etc/init.d/nginx restart
   ```

   或者

   ```bash
   lnmp restart
   ```

   然后就可以打开网站按要求进行配置了。

   **注意：**打开网站后可能会出现`Chevereto can’t create the app/settings.php file. You must manually create this file`的错误，这时在`app`目录新建`settings.php`文件并给予可写入权限即可。

   