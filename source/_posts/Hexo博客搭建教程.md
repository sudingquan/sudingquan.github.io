---
title: Hexo博客搭建教程
date: 2018-12-04 00:22:35
tags: hexo
categories: hexo
---
## 前言

本文采用 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) 许可协议，转载请注明出处！


## 环境准备
1. [Node.js](http://nodejs.org/) 下载，并安装；  

2. [Git](https://git-scm.com/) 下载，并安装；  

3. 安装Hexo，在命令行运行以下命令：

   ```
    npm install -g hexo-cli
   ```

<!-- more -->

4. 在命令行依次运行以下命令（folder为你的博客文件夹名）：

   ```
   hexo init <folder>
   cd <folder>
   npm install
   ```

  **注意：**

  + 与hexo相关命令均在站点目录下；  

  + 站点配置文件：站点目录下的`_config.yml`；  

  + 主题配置文件：站点目录下的/themes/主题名/`_config.yml`；  

5. 启动服务。在命令行中输入以下命令：  
  `hexo server`  

  或

  `hexo s`

6. 打开浏览器，访问 http://localhost:4000/  

   至此，你的Hexo博客已经搭建在本地。


## 部署至GithubPages
1. 创建Github账号

2. 创建仓库，仓库名为：你的Github账号名称.github.io

3. 将本地搭建的Hexo博客推送到GithubPages，方法如下：

   + 安装hexo-deployer-git插件，运行以下命令：
     `npm install hexo-deployer-git --save`  

   + 添加SSH key。
   
     + 创建一个 SSH key 。在命令行输入以下命令， 然后一路回车即可。
      `ssh-keygen -t rsa -C "github邮箱地址"`  
     
     + 复制生成的密钥文件内容（id_rsa.pub），粘贴到github的New SSH Key即可。
     
     + 测试是否添加成功,依次输入以下命令：
     
        ```
        ssh -T git@github.com
        yes
        ```
     
   + 修改博客目录下的`_config.yml`，修改末尾:
   
     ```
     deploy:
     type: git
     repo: git@github.com:你的Github账号名称/你的Github账号名称.github.io.git
     branch: master
     ```
   
   + 部署到GithubPages，依次输入以下命令：
   
     ```
     hexo g
     hexo d
     ```
   
   + 等待几十秒，打开：https://你的Github账号名称.github.io。
   
     至此，你的Hexo博客已经搭建在GithubPages，并可以通过域名 https://你的Github账号名称.github.io 访问。
   