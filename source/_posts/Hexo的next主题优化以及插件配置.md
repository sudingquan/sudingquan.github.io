---
title: Hexo的next主题优化以及插件配置
date: 2018-12-15 15:16:22
tags: hexo
categories: hexo
---
## 前言
Hexo默认的主题是landscape,但是~~爱折腾~~的我显然并不满足于默认主题(真香)，找来找去也就发现了这个便于折腾的主题————NexT。  

注：在 Hexo 中有两份主要的配置文件，其名称都是`_config.yml`。 其中，一份位于站点根目录下，主要包含 Hexo 本身的配置；另一份位于主题目录下，这份配置由主题作者提供，主要用于配置主题相关的选项 。

为了描述方便，在以下说明中，将前者称为 `站点配置文件`， 后者称为 `主题配置文件`。

## 安装NexT
### 下载主题
在终端窗口下，定位到 Hexo 站点目录下。运行命令`git clone https://github.com/theme-next/hexo-theme-next themes/next`来安装主题。

### 启用主题
与所有 Hexo 主题启用的模式一样。 当 克隆/下载 完成后，打开 `站点配置文件`， 找到 theme 字段，并将其值更改为 next。  

到此，NexT 主题安装完成。下一步我们将验证主题是否正确启用。在切换主题之后、验证之前， 我们最好使用 hexo clean 来清除 Hexo 的缓存。

### 验证主题
首先启动 Hexo 本地站点，并开启调试模式（即加上 `--debug`），整个命令是 `hexo s --debug`。 在服务启动的过程，注意观察命令行输出是否有任何异常信息，如果你碰到问题，这些信息将帮助他人更好的定位错误。 当命令行输出中提示出：

``` shell
INFO  Hexo is running at http://0.0.0.0:4000/. Press Ctrl+C to stop.
```

此时即可使用浏览器访问 http://localhost:4000 ,检查站点是否正确运行。  

现在，你已经成功安装并启用了 NexT 主题。下一步我们将要更改一些主题的设定。  

## 主题设定
### 选择 Scheme
Scheme 是 NexT 提供的一种特性，借助于 Scheme，NexT 为你提供多种不同的外观。同时，几乎所有的配置都可以 在 Scheme 之间共用。目前 NexT 支持四种 Scheme。具体的样式可以参考 https://github.com/theme-next/hexo-theme-next

Scheme 的切换通过更改 `主题配置文件`，搜索 scheme 关键字。 你会看到有四行 scheme 的配置，将你需用启用的 scheme 前面注释 `#` 去除即可。

### 设置语言
编辑 `站点配置文件`， 将 language 设置成中文。配置如下

```
language: zh-CN
```
### 设置作者昵称
编辑 `站点配置文件`， 设置 author 为你的昵称。
### 站点描述
编辑 `站点配置文件`， 设置 description 字段为你的站点描述。站点描述可以是你喜欢的一句签名:)
### 设置头像
编辑 `主题配置文件`， 修改字段 avatar， 值设置成头像的链接地址。其中，头像的链接地址可以是：

地址|值
---|--
完整的互联网 URI|http://example.com/avatar.png
站点内的地址|将头像放置主题目录下的 source/uploads/ （新建 uploads 目录 若不存在）<br> 配置为：avatar: /uploads/avatar.png<br>或者放置在 source/images/ 目录下<br>配置为：avatar: /images/avatar.png
### 设置阅读全文

Hexo 的 Next 主题默认是首页显示你每篇文章的全文内容，但这会使你的首页篇幅过于冗长。

要解决这个问题，打开 `主题配置文件`，找到以下代码：
```
auto_excerpt:
      enable: false
      length: 150
```
将false改为true，length为预览文章的长度。
### 添加gitalk评论功能
#### 注册新应用
在GitHub上注册新应用，链接：https://github.com/settings/applications/new

参数说明：
Application name： # 应用名称，随意
Homepage URL： # 网站URL，如 https://xxxxxxxx.github.io
Application description # 描述，随意
Authorization callback URL：# 网站URL，如 https://xxxxxxxx.github.io

点击注册后，页面跳转到应用界面，其中`Client ID`和`Client Secret`在后面的配置中需要用到，到时复制粘贴即可
#### 修改主题配置文件
在`主题配置文件`中搜索`gitalk`字段：
```
gitalk:
  enable: true
  githubID: github帐号  
  repo: 仓库名称
  ClientID: Client ID
  ClientSecret: Client Secret
  adminUser: github帐号 #指定可初始化评论账户
  distractionFreeMode: true
```
### 添加「标签」页面
在你的站点目录下，运行命令：
```shell
hexo new page tags
```

添加分类页面过程类似  

如果有集成评论服务，页面也会带有评论。 若需要关闭的话，请添加字段 comments 并将值设置为 false。
### 设置代码高亮主题
NexT 使用 Tomorrow Theme 作为代码高亮，共有5款主题供你选择。 NexT 默认使用的是 白色的 normal 主题，可选的值有 normal，night， night blue， night bright， night eighties。

更改 highlight_theme 字段，将其值设定成你所喜爱的高亮主题。
### 侧边栏社交链接
找到social字段，将注释去掉并修改即可。
### 开启打赏功能
只需要`主题配置文件` 中填入 微信 和 支付宝 收款二维码图片地址 即可开启该功能。
```
reward:
  enable: true
  #comment: Donate comment here
  wechatpay: /images/wechatpay.jpg
  alipay: /images/alipay.jpg
  #bitcoin: /images/bitcoin.png
```
### 友情链接
编辑 `主题配置文件`,修改links字段。
### 设置「背景动画」
1. 定位到你的NexT文件夹中，运行命令`git clonegit clone https://github.com/theme-next/theme-next-three source/lib/three`
2. 修改three_waves,canvas_lines,canvas_sphere任意一种为true

### 设置页面透明度
由于NexT主题默认的背景是不透明的，所以我们刚刚设置的背景动画会被文章挡住，所以我们要修改页面的透明度，主要依靠修改CSS样式来实现
#### 文章部分

文件位置 `~Hexo根目录/themes/next/source/css/_schemes/Gemini/index.styl`

```
// Post & Comments blocks.
.post-block {
  padding: $content-desktop-padding;
  background: rgba(255,255,255,0.7);
  box-shadow: $box-shadow-inner;
  border-radius: $border-radius-inner;
}
```
#### 阅读全文按钮
文件位置 `~Hexo根目录/themes/next/source/css/_variables/Pisces.styl`

```
// Button
  $btn-default-radius           = 2px
  $btn-default-bg               = rgba(255,255,255,0.5)
  $btn-default-color            = $text-color
  $btn-default-border-color     = $text-color
  $btn-default-hover-color      = white
  $btn-default-hover-bg         = $black-deep
```

#### 分页部分
文件位置 `~Hexo根目录/themes/next/source/css/_schemes/Gemini/index.styl`

```
// Pagination.
.pagination {
  .prev, .next, .page-number {
    margin-bottom: initial;
    top: initial;
  }
  margin: sboffset 0 0;
  background: rgba(255,255,255,0.7);
  box-shadow: $box-shadow;
  border-radius: $border-radius;
  padding: 10px 0 10px;
}
```
#### 评论区部分
文件位置 `~Hexo根目录/themes/next/source/css/_schemes/Gemini/index.styl`
```
// Comments blocks.
.comments {
  padding: $content-desktop-padding;
  margin: initial;
  margin-top: sboffset;
  background: rgba(255,255,255,0.7);
  box-shadow: $box-shadow;
  border-radius: $border-radius;
}
```
#### 侧边菜单界面
文件位置 `~Hexo根目录/themes/next/source/css/_schemes/Pisces/_layout.styl`

```
.header-inner {
  position: absolute;
  top: 0;
  overflow: hidden;
  padding: 0;
  width: $sidebar-desktop;
  background: rgba(255,255,255,0.7);
  box-shadow: $box-shadow-inner;
  border-radius: $border-radius-inner;

  +desktop-large() {
    .container & { width: $sidebar-desktop; }
  }
  +tablet() {
    position: relative;
    width: auto;
    border-radius: initial;
  }
  +mobile() {
    position: relative;
    width: auto;
    border-radius: initial;
  }
}
```
#### 个人资料界面
文件位置 `~Hexo根目录/themes/next/source/css/_schemes/Pisces/_sidebar.styl`
```
.sidebar-inner {
//padding: 20px 10px 0;
  box-sizing: border-box;
  width: $sidebar-desktop;
  color: $text-color;
  background: rgba(255,255,255,0.7);
  box-shadow: $box-shadow;
  border-radius: $border-radius;
  if (hexo-config('motion.enable') and hexo-config('motion.transition.sidebar')) { opacity: 0; }

  &.affix {
    position: fixed;
    top: $sidebar-offset;
  }
```
### 添加字数统计和阅读时间
定位到你的博客下，运行以下命令：
```
$ npm install hexo-symbols-count-time --save
```
打开 `站点配置文件` ,在最后加入以下内容：
```
symbols_count_time:
  symbols: true
  time: true
  total_symbols: true
  total_time: true
```
打开 `主题配置文件` ，搜索`symbols_count_time`并修改成以下内容：
```
symbols_count_time:
  separated_meta: true
  item_text_post: true
  item_text_total: false
  awl: 4
  wpm: 275
```
### 显示当前浏览进度
打开主题配置文件，修改以下内容：
```
# Back to top in sidebar
b2t: true

# Scroll percent label in b2t button
scrollpercent: true
```
### 在右上角或者左上角实现fork me on github
首先在[这里](https://blog.github.com/2008-12-19-github-ribbons/)或[这里](http://tholman.com/github-corners/)挑选你喜欢的样式并复制代码，然后将其中的链接改为你自己的github地址，最后打开`themes/next/layout/_layout.swig`将改好的代码粘贴在`<div class="headband"></div>`下面。  

### 添加点击爱心效果
#### 创建js文件
在`/themes/next/source/js/src`下新建文件 `clicklove.js`,然后将以下代码拷贝进去：
```
!function(e,t,a){function n(){c(".heart{width: 10px;height: 10px;position: fixed;background: #f00;transform: rotate(45deg);-webkit-transform: rotate(45deg);-moz-transform: rotate(45deg);}.heart:after,.heart:before{content: '';width: inherit;height: inherit;background: inherit;border-radius: 50%;-webkit-border-radius: 50%;-moz-border-radius: 50%;position: fixed;}.heart:after{top: -5px;}.heart:before{left: -5px;}"),o(),r()}function r(){for(var e=0;e<d.length;e++)d[e].alpha<=0?(t.body.removeChild(d[e].el),d.splice(e,1)):(d[e].y--,d[e].scale+=.004,d[e].alpha-=.013,d[e].el.style.cssText="left:"+d[e].x+"px;top:"+d[e].y+"px;opacity:"+d[e].alpha+";transform:scale("+d[e].scale+","+d[e].scale+") rotate(45deg);background:"+d[e].color+";z-index:99999");requestAnimationFrame(r)}function o(){var t="function"==typeof e.onclick&&e.onclick;e.onclick=function(e){t&&t(),i(e)}}function i(e){var a=t.createElement("div");a.className="heart",d.push({el:a,x:e.clientX-5,y:e.clientY-5,scale:1,alpha:1,color:s()}),t.body.appendChild(a)}function c(e){var a=t.createElement("style");a.type="text/css";try{a.appendChild(t.createTextNode(e))}catch(t){a.styleSheet.cssText=e}t.getElementsByTagName("head")[0].appendChild(a)}function s(){return"rgb("+~~(255*Math.random())+","+~~(255*Math.random())+","+~~(255*Math.random())+")"}var d=[];e.requestAnimationFrame=function(){return e.requestAnimationFrame||e.webkitRequestAnimationFrame||e.mozRequestAnimationFrame||e.oRequestAnimationFrame||e.msRequestAnimationFrame||function(e){setTimeout(e,1e3/60)}}(),n()}(window,document);
```
#### 修改_layout.swig
在`\themes\next\layout\_layout.swig`文件末尾添加：
```
<!-- 页面点击小红心 -->
<script type="text/javascript" src="/js/src/clicklove.js"></script>
```
### 文章尾部添加版权信息
打开 `主题配置文件` ,搜索`creative_commons`，将`post`修改为`true`。
### 修改文章底部的那个带#号的标签
修改模板`/themes/next/layout/_macro/post.swig`，搜索 `rel="tag">#`，将 `#` 换成`<i class="fa fa-tag"></i>`
### 添加顶部加载条
#### 安装插件
定位到`/themes/next`，运行以下命令：
```
git clone https://github.com/theme-next/theme-next-pace source/lib/pace
```
#### 修改主题配置文件
打开 `主题配置文件` 搜索`pace`，改为`true`,然后修改`pace-theme`为你喜欢的样式
### 添加搜索服务
添加百度/谷歌/本地 自定义站点内容搜索  

1. 在站点的根目录下执行以下命令：
```
$ npm install hexo-generator-searchdb --save
```
2. 编辑 `站点配置文件` ，新增以下内容到任意位置：
```
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
```
3. 编辑 `主题配置文件` ，启用本地搜索功能：
```
# Local search
local_search:
  enable: true
```

### 添加分享服务
1. 定位到`/themes/next`,运行以下命令：
```
git clone https://github.com/theme-next/theme-next-needmoreshare2 source/lib/needsharebutton
```
2. 编辑 `主题配置文件`
```
needmoreshare2:
  enable: true
  postbottom:
    enable: true
  float:
    enable: true
```

### 添加在线聊天DaoVoice
#### 注册
首先需要注册一个 DaoVoice，[点击注册](http://dashboard.daovoice.io/get-started?invite_code=7f3d6e70)

注册成功后，进入后台控制台，进入到 `应用设置-->安装到网站` 页面，可以得到一个 `app_id`。
#### 设置
打开 `themes/next/layout/_partials/head.swig` 文件中添加如下代码，位置随意：
```css
{% if theme.daovoice %}
  <script>
  (function(i,s,o,g,r,a,m){i["DaoVoiceObject"]=r;i[r]=i[r]||function(){(i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;a.charset="utf-8";m.parentNode.insertBefore(a,m)})(window,document,"script",('https:' == document.location.protocol ? 'https:' : 'http:') + "//widget.daovoice.io/widget/0f81ff2f.js","daovoice")
  daovoice('init', {
      app_id: "{{theme.daovoice_app_id}}"
    });
  daovoice('update');
  </script>
{% endif %}
```
打开 `主题配置文件` ，添加以下代码：
```
# Online contact
daovoice: true
daovoice_app_id: 这里输入前面获取的app_id
```
### 添加网站运行时间
打开 `\themes\next\layout\_partial\footer.ejs` 文件下添加以下代码：
```
<span id="timeDate">载入天数...</span><span id="times">载入时分秒...</span>
<script>
    var now = new Date();
    function createtime() {
        var grt= new Date("12/04/2018 00:00:00");//在此处修改你的建站时间
        now.setTime(now.getTime()+250);
        days = (now - grt ) / 1000 / 60 / 60 / 24; dnum = Math.floor(days);
        hours = (now - grt ) / 1000 / 60 / 60 - (24 * dnum); hnum = Math.floor(hours);
        if(String(hnum).length ==1 ){hnum = "0" + hnum;} minutes = (now - grt ) / 1000 /60 - (24 * 60 * dnum) - (60 * hnum);
        mnum = Math.floor(minutes); if(String(mnum).length ==1 ){mnum = "0" + mnum;}
        seconds = (now - grt ) / 1000 - (24 * 60 * 60 * dnum) - (60 * 60 * hnum) - (60 * mnum);
        snum = Math.round(seconds); if(String(snum).length ==1 ){snum = "0" + snum;}
        document.getElementById("timeDate").innerHTML = "本博客已萌萌哒运行 "+dnum+" 天 ";
        document.getElementById("times").innerHTML = hnum + " 小时 " + mnum + " 分 " + snum + " 秒";
    }
setInterval("createtime()",250);
</script>
```
