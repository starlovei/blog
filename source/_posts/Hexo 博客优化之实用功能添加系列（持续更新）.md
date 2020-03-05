---
layout: Hexo
title: 博客优化之实用功能添加系列（持续更新）
author: 小枫叶
avatar: https://starlovei-1257629504.cos.ap-chengdu.myqcloud.com/Image/78468086_p0.png
photos: https://starlovei-1257629504.cos.ap-chengdu.myqcloud.com/Image/68422578_p0.jpg
date: 2020-02-07 12:22:00
authorDesc: 一个好奇的人
categories: 转载
tags:
    - 主题
    - 网络
---
> **转载自: https://itrhx.blog.csdn.net/article/details/85010191**
**本文将讲述一些 Hexo 博客的美化，本文以作者 [luuman](https://blog.luuman.club/Home/H1/index.html) 的 [spfk](https://github.com/luuman/hexo-theme-spfk) 主题和作者 [xaoxuu](https://xaoxuu.com/) 的 [MaterialX](https://xaoxuu.com/wiki/material-x/) 主题为例，实际效果欢迎访问[我的博客](https://www.itrhx.com/)进行查看，本文章会不定时进行更新。文章涉及有关参考资料、教程、链接如有侵权请联系我删除！**

**请注意**：不同主题可能方法有些不同，相同主题不同版本，配置方法也有所差异！

**博客美化前提条件**：有一定的前端基础，了解 HTML、CSS、JS，了解 CSS 预处理语言 Sass、Less、Stylus，搞懂 hexo 的目录结构。

**博客美化通用步骤**：选定主题，认真阅读主题文档，分析主题目录结构，了解每个文件是对应网页哪个部分的，认真阅读美化教程，美化教程本质上只为你提供核心代码和思路，具体代码要添加到哪个地方，需要你自己搞懂主题结构，添加到需要的、合适的位置！

**博客美化终极奥秘**：创作第一，体验第二，避免繁杂，简洁为上！

## 1 添加评论系统
主流的评论系统有很多，比如：网易云跟帖、多说、友言、畅言、来必力（LiveRe）、Disqus、Valine、Gitment等等，目前网易云跟帖、多说、友言都已经关闭了，还有些可能需要魔法，比较麻烦，百度了一下，最后还是选择了来必力评论系统

进入[来必力官网](https://livere.com/), 注册一个账号（注册时可能需要魔法）
![logo](https://starlovei-1257629504.cos.ap-chengdu.myqcloud.com/NEXT/aHR0cHM6Ly9pLmxvbGkubmV0LzIwMTkvMDMvMzAvNWM5ZjkxZWQ4OTczYS5wbmc.jfif)
注册完毕之后，登录，进入安装页面，选择 City 免费版安装，安装之后你会得到一段代码
![logo](https://starlovei-1257629504.cos.ap-chengdu.myqcloud.com/NEXT/20190807103722227.png)
![logo](https://starlovei-1257629504.cos.ap-chengdu.myqcloud.com/NEXT/20190807103745470.png)
![logo](https://starlovei-1257629504.cos.ap-chengdu.myqcloud.com/NEXT/aHR0cHM6Ly9pLmxvbGkubmV0LzIwMTkvMDMvMzEvNWM5ZjkyOWU3ZDYyYi5wbmc.jfif)
我们打开主题文件下的 **_config.yml** 文件，添加如下代码:
``` css
livere:
  on: true
  live_uid: XXXX #这里填写你自己的uid
```
在 **\themes\hexo-theme-spfk\layout\_partial\comments** 文件夹下新建一个 **livere.ejs** 的文件，在里面填写来必力提供的代码：
``` cs
<!-- 来必力City版安装代码 -->
<div id="lv-container" data-id="city" data-uid="这里是你的uid">
	<script type="text/javascript">
		(function(d, s) {
        var j, e = d.getElementsByTagName(s)[0];
    
        if (typeof LivereTower === 'function') { return; }
    
        j = d.createElement(s);
        j.src = 'https://cdn-city.livere.com/js/embed.dist.js';
        j.async = true;
    
        e.parentNode.insertBefore(j, e);
        })(document, 'script');
	</script>
    <noscript>为正常使用来必力评论功能请激活JavaScript</noscript>
</div>
<!-- City版安装代码已完成 -->
```
打开 **\themes\hexo-theme-spfk\layout\_partial\article.ejs** 文件，在适当位置添加如下红框中的代码：
![logo](https://starlovei-1257629504.cos.ap-chengdu.myqcloud.com/NEXT/aHR0cHM6Ly9pLmxvbGkubmV0LzIwMTkvMDMvMzEvNWM5ZjkyZTNlZWJhYS5wbmc.jfif)
完成以上操作之后，我们就可以使用来必力评论系统了
![logo](https://starlovei-1257629504.cos.ap-chengdu.myqcloud.com/NEXT/20190807103817644.png)
另外推荐使用 [Valine](https://valine.js.org/) 评论系统，和 gitalk 评论系统
## 2 添加字数统计和阅读时长
先在博客目录下执行以下命令安装 **hexo-wordcount** 插件：
``` css
$ npm i --save hexo-wordcount
```
注意：在 [MaterialX](https://xaoxuu.com/wiki/material-x/) 主题中，字数统计和阅读时长的功能我已提交 PR，在最新版本中，只需要安装插件后，在主题 *config.yml* 配置文件里，将 *word_count* 关键字设置为 *true* 即可，对于旧版本，可以通过以下方法实现：
以 [MaterialX](https://xaoxuu.com/wiki/material-x/) 主题（版本 1.2.1）为例，在 **\themes\material-x\layout\_meta** 目录下创建 **word.ejs** 文件，在 **word.ejs** 文件中写入以下代码：
``` js
<% if(isPostList || !isPostList){ %>
  <% if (theme.word_count && !post.no_word_count) { %>
    <div style="margin-right: 10px;">
      <span class="post-time">
        <span class="post-meta-item-icon">
          <i class="fa fa-keyboard"></i>
          <span class="post-meta-item-text">  字数统计: </span>
          <span class="post-count"><%= wordcount(post.content) %>字</span>
        </span>
      </span>
      &nbsp; | &nbsp;
      <span class="post-time">
        <span class="post-meta-item-icon">
          <i class="fa fa-hourglass-half"></i>
          <span class="post-meta-item-text">  阅读时长≈</span>
          <span class="post-count"><%= min2read(post.content) %>分</span>
        </span>
      </span>
    </div>
  <% } %>
<% } %>
```
然后在主题的配置文件 **_config.yml** 找到 **meta** 关键字，将 **word** 填入 **header** 中：
``` css
meta:
  header: [title, author, date, categories, tags, counter, word, top]
  footer: [updated, share]
```
最后在主题目录下的 **_config.yml** 添加以下配置即可
``` css
word_count: true
```
效果图：
![logo](https://starlovei-1257629504.cos.ap-chengdu.myqcloud.com/NEXT/20191129201331969.png)
同样的，以 [spfk](https://github.com/luuman/hexo-theme-spfk) 主题为例，在 **\themes\hexo-theme-spfk\layout\_partial\post** 目录下创建 **word.ejs** 文件，在 **word.ejs** 文件中写入以下代码：
``` js
<div style="margin-top:10px;">
    <span class="post-time">
      <span class="post-meta-item-icon">
        <i class="fa fa-keyboard-o"></i>
        <span class="post-meta-item-text">  字数统计: </span>
        <span class="post-count"><%= wordcount(post.content) %>字</span>
      </span>
    </span>
    &nbsp; | &nbsp;
    <span class="post-time">
      <span class="post-meta-item-icon">
        <i class="fa fa-hourglass-half"></i>
        <span class="post-meta-item-text">  阅读时长: </span>
        <span class="post-count"><%= min2read(post.content) %>分</span>
      </span>
    </span>
</div>
```
然后在 **\themes\hexo-theme-spfk\layout\_partial\article.ejs** 中适当位置添加以下代码：
![logo](https://starlovei-1257629504.cos.ap-chengdu.myqcloud.com/NEXT/aHR0cHM6Ly9pLmxvbGkubmV0LzIwMTkvMDMvMzEvNWM5Zjk1ZmU0ZTM2YS5wbmc.jfif)
最后在主题目录下的 **_config.yml** 添加以下配置即可
``` css
word_count: true
```
如果显示的位置不好，可以自行更改其位置，成功配置后的效果如下：
![logo](https://starlovei-1257629504.cos.ap-chengdu.myqcloud.com/NEXT/20190807104015998.png)
![logo](https://starlovei-1257629504.cos.ap-chengdu.myqcloud.com/NEXT/aHR0cHM6Ly9pLmxvbGkubmV0LzIwMTkvMDMvMzEvNWM5Zjk2ODkyZTRkYi5wbmc.jfif)
另外：要在博客底部显示所有文章的总字数，可以[点击此处](https://www.npmjs.com/package/hexo-wordcount)，根据你博客底部文件的类型选择相应的代码放在适当的位置即可，前提是要安装好 **hexo-wordcount** 插件，例如我使用 [MaterialX](https://xaoxuu.com/wiki/material-x/) 主题，在 **\themes\material-x\layout\_partial** 目录下的 **footer.ejs** 文件中添加如下代码：
``` css
<i class="fas fa-chart-area"></i>
<span class="post-count">字数统计：<%= totalcount(site) %></span>
```
实现效果如下：
![logo](https://starlovei-1257629504.cos.ap-chengdu.myqcloud.com/NEXT/20190807104042630.png)
## 3 添加网站运营时间
一个比较好的小功能，可以看见自己的博客运行多久了，时间一天天的增加，成就感也会一天天增加的
在 **\themes\hexo-theme-spfk\layout\_partial\footer.ejs** 文件下添加以下代码：
``` js
<span id="timeDate">载入天数...</span><span id="times">载入时分秒...</span>
<script>
    var now = new Date(); 
    function createtime() { 
        var grt= new Date("08/10/2018 17:38:00");//在此处修改你的建站时间，格式：月/日/年 时:分:秒
        now.setTime(now.getTime()+250); 
        days = (now - grt ) / 1000 / 60 / 60 / 24; dnum = Math.floor(days); 
        hours = (now - grt ) / 1000 / 60 / 60 - (24 * dnum); hnum = Math.floor(hours); 
        if(String(hnum).length ==1 ){hnum = "0" + hnum;} minutes = (now - grt ) / 1000 /60 - (24 * 60 * dnum) - (60 * hnum); 
        mnum = Math.floor(minutes); if(String(mnum).length ==1 ){mnum = "0" + mnum;} 
        seconds = (now - grt ) / 1000 - (24 * 60 * 60 * dnum) - (60 * 60 * hnum) - (60 * mnum); 
        snum = Math.round(seconds); if(String(snum).length ==1 ){snum = "0" + snum;} 
        document.getElementById("timeDate").innerHTML = "本站已安全运行 "+dnum+" 天 "; 
        document.getElementById("times").innerHTML = hnum + " 小时 " + mnum + " 分 " + snum + " 秒"; 
    } 
setInterval("createtime()",250);
</script>
```
最后效果如下：
![logo](https://starlovei-1257629504.cos.ap-chengdu.myqcloud.com/NEXT/aHR0cHM6Ly9pLmxvbGkubmV0LzIwMTkvMDMvMzEvNWM5Zjk3ZGRlY2E4YS5wbmc.jfif)
## 4 添加百度统计
百度统计是百度推出的一款免费的专业网站流量分析工具，能够告诉用户访客是如何找到并浏览用户的网站，在网站上做了些什么，非常有趣，接下来我们把百度统计添加到自己博客当中

访问[百度统计首页](https://tongji.baidu.com/web/welcome/login)，注册一个账号后登陆，添加你的博客网站
![logo](https://starlovei-1257629504.cos.ap-chengdu.myqcloud.com/NEXT/aHR0cHM6Ly9pLmxvbGkubmV0LzIwMTkvMDMvMzEvNWM5Zjk4NTk0Yzg5NS5wbmc.jfif)
接着点击代码获取，复制该代码
![logo](https://starlovei-1257629504.cos.ap-chengdu.myqcloud.com/NEXT/aHR0cHM6Ly9pLmxvbGkubmV0LzIwMTkvMDMvMzEvNWM5Zjk4ODE1M2MzNS5wbmc.jfif)
然后到目录 **\themes\hexo-theme-spfk\layout\_partial** 下新建一个 **baidu-analytics.ejs** 文件，里面粘贴你刚刚复制的代码
![logo](https://starlovei-1257629504.cos.ap-chengdu.myqcloud.com/NEXT/aHR0cHM6Ly9pLmxvbGkubmV0LzIwMTkvMDMvMzEvNWM5Zjk4YjBiYjRlZi5wbmc.jfif)
修改主题文件夹下的 **_config.yml** 文件，将你的key（图中涂掉部分）填写进去：
![logo](https://starlovei-1257629504.cos.ap-chengdu.myqcloud.com/NEXT/aHR0cHM6Ly9pLmxvbGkubmV0LzIwMTkvMDMvMzEvNWM5Zjk4ZDQzNTQ5ZC5wbmc.jfif)
所有操作完成后可以在百度统计管理页面检查代码是否安装成功，如果代码安装正确，一般20分钟后，可以查看网站分析数据
![logo](https://starlovei-1257629504.cos.ap-chengdu.myqcloud.com/NEXT/aHR0cHM6Ly9pLmxvbGkubmV0LzIwMTkvMDMvMzEvNWM5Zjk5MDBiOWI4Ny5wbmc.jfif)
另外推荐：[友盟](https://web.umeng.com/main.php?c=user&a=index)，2010年4月在北京成立，安全、可靠、公正、第三方的网站流量统计分析系统
## 5 添加RSS订阅
RSS订阅是站点用来和其他站点之间共享内容的一种简易方式，即Really Simple Syndication（简易信息聚合），如果不会使用，可以参见百度百科：https://baike.baidu.com/item/RSS%E8%AE%A2%E9%98%85/663114 ；首先我们安装feed插件，在本地hexo目录下右键*Git Bash Here*，输入以下命令：
``` css
$ npm install hexo-generator-feed
```
等待安装完成后，打开hexo目录下的配置文件 **_config.yml**，在末尾添加以下配置：
``` css
# Extensions
## Plugins: http://hexo.io/plugins/
#RSS订阅
plugin:
- hexo-generator-feed
#Feed Atom
feed:
type: atom
path: atom.xml
limit: 20
```
随后打开主题配置文件 **_config.yml**，添加以下配置：
``` css
rss: /atom.xml
```
至此，RSS订阅功能添加完成
## 6 添加 Fork me on GitHub 效果
效果图：
![logo](https://starlovei-1257629504.cos.ap-chengdu.myqcloud.com/NEXT/20190807104140857.png)
[点击此处](https://github.blog/2008-12-19-github-ribbons/)可以查看更多样式，将相应样式的代码复制到你想要放的地方就OK了，代码里的链接也要替换成你的，更多创意，比如 Follow me on CSDN ，只需要用PS改掉图片里的文字，替换掉相应链接即可
## 7 更改本地预览端口号
hexo博客在执行*hexo s*进行本地预览的时候，默认端口号是4000，当该端口号被占用时会报错 *Error: listen EADDRINUSE 0.0.0.0:4000* ，此时可以关闭占用该端口的进程，也可以更换端口号，更换端口号可以通过以下两种方法实现：
方法一：在根目录的*_config.yml*配置文件内加上如下代码更改*hexo s*运行时的端口号：
``` css
server:
  port: 5000
  compress: true
  header: true
```
方法二：通过 *hexo server -p 5000* 命令来指定端口，这种方法只是本次执行有效