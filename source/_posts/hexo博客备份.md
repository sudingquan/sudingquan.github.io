---
title: hexo博客备份
date: 2018-12-04 23:46:45
tags: hexo
---
- ## 正文
针对博客已经搭建并发布过文章的。    <br/><br/>
1.在你的博客仓库创建一个分支hexo(名称随意);<br/><br/>
2.设置hexo为默认分支;<br/><br/>
3.将博客仓库clone至本地，将之前的Hexo文件夹中的`_config.yml`，`themes/`，`source`，`scffolds/`，`package.json`，`.gitignore`复制到你克隆下来的仓库文件夹，即Username.github.io；(Username是你自己的用户名);<br><br>
这里说一下为什么只需要拷贝6个，而不需要全部：  
    + `_config.yml`站点的配置文件，需要拷贝
    + `themes/`主题文件夹，需要拷贝
    + `source`博客文章的md文件，需要拷贝
    + `scaffolds/`文章的模板，需要拷贝
    + `package.json`安装包的名称，需要拷贝
    + `.gitignore`限定在push时哪些文件可以忽略，需要拷贝
    + `.git/`主题和站点都有，标志这是一个git项目，不需要拷贝
    + `node_modules/`是安装包的目录，在执行npm install的时候会重新生成，不需要拷贝
    + `public`是`hexo g`生成的静态网页，不需要拷贝
    + `.deploy_git`同上，`hexo g`也会生成，不需要拷贝
    + `db.json文件`，不需要拷贝<br><br>
    其实不需要拷贝的文件正是`.gitignore`中所忽略的。<br><br>

  4.将`themes/你使用的主题文件夹`中的`.git/`删除，否则无法将主题文件夹push;  <br><br>
  5.cd 到 Username.github.io，执行`npm install`,`npm install hexo-deployer-git`;  <br><br>
  6.执行`git add .`，`git commit -m ""`，`git push origin hexo`来提交Hexo网站源文件;  <br><br>
  7.执行`hexo g -d`生成静态网页部署到github上。  <br><br>
  这样，Username.github.io仓库就有master分支保存静态网页，hexo分支保存源文件。
- ## 修改
在本地对博客修改后  <br><br>
   + 执行`git add .`，`git commit -m ""`，`git push origin hexo`来提交Hexo网站源文件  <br><br>
   + 执行`hexo g -d`生成静态网页部署到github上。  <br><br>
- ## 恢复
换电脑改博客   <br><br>
1.安装git;   <br><br>
2.安装Nodejs和npm; <br><br>
3.使用`git clone`将仓库拷贝至本地;<br><br>
4.在文件夹内执行命令`npm install hexo-cli -g`、`npm install`、`npm install hexo-deployer-git`。  
<br><br>
>参考  
原始链接：https://blog.itswincer.com/posts/7efd2818/  
文章作者：[WincerChan](https://blog.itswincer.com?)
