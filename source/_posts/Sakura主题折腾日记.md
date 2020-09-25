---
title: Sakura主题折腾日记
author: 小枫叶
avatar: https://cdn.jsdelivr.net/gh/starlovei/cdn/img/avatar/01.jpg
tags:
  - 主题
  - 网络
date: 2020-04-27 19:23:53
photos: https://cdn.jsdelivr.net/gh/starlovei/picgo/Sakura/photo1.png
categories: 技术
description: 年更状态...
---
> 记录一下博客瞎折腾过程,以及遇到过的bug,借用了不少大佬的灵感
> https://ctz45562.github.io/
> https://blog.ukenn.top/sakura7/

## 超链接鼠标悬浮特效
``` css
/* 文章中的超链接,鼠标悬浮特效*/
.article-entry a{
    display: inline-block;
    position: relative;
    color: #9400D3;
    font-family: lucida console;/* 这种字体的英文比较好看（像代码样式） */
}
/* 鼠标悬浮时，变色可自行修改,需要了解颜色16进制编码 */
.article-entry a:hover{
    color: #9400D3;
    transition:.8s;
}
/* 鼠标悬浮时，下划线从中间向两边延伸 */
.article-entry a:hover::after{
  transform: scaleX(1);
  /* 旋转，与transform连用；这里作用：鼠标悬浮时，底部的下划线从中间扩散到两边。
   bottom right :左到右出现，左到右消失（需配合上面的::after）
  */
  transform-origin: bottom center;
}
/* 鼠标移开后，下划线从两边向中间消失 */
.article-entry a::after{
  content: '';
  position: absolute;
  width: 100%;
  transform: scaleX(0);
  height: 1.5px;
  bottom: 0;
  left: 0;
  background-color: #0087ca;
  /*旋转，与transform连用；这里作用：鼠标移开后，底部的下划线从中间开始消失*/
  transform-origin: bottom center;
  transition: transform 0.5s ease-out;
}
```
如果开启了fancybox可能会出现鼠标放到图片链接显示特效的情况,看起来有点奇怪.
解决方案:从[ctz45562](https://ctz45562.github.io/)大佬嫖来的,在`Sakura\source\css\style.css`里添加

``` css
a#escape-link {
    background-size: 0 0 !important;
}
```
图片引用格式:

```css
<a data-fancybox="gallery" href="图片链接" id="escape-link"><img src="图片链接"></a>
```

## 归档页面时间轴颜色
找了好久才找到
在`Sakura\source\css\style.css`里
``` css
.art-content #archives .al_mon_list:before {
    max-height:100%;
    height:100%;
    width:4px;
    background:#8A2BE2;
    position:absolute;
    left:120px;
    content:"";
    top:0
}
```
和
``` css
.art-content #archives .al_mon_list .al_mon:after,.art .art-content .al_mon_list .al_post_list>li:after {
    background:#8A2Be2
}
```
修改颜色编码
## 网站灰色代码
一些特殊节日有些网站会把网站变成灰色,同样在`Sakura\source\css\style.css`添加
``` css
html {
    filter: progid:DXImageTransform.Microsoft.BasicImage(grayscale=1);
    -webkit-filter: grayscale(100%);
}
```
## 公式渲染问题
有时可能会出现刷新才能渲染的情况
感谢https://github.com/ikeq/hexo-theme-inside/issues/29 的帮助
公式渲染有两种方案：

### 方案1

在需要渲染公式的 post 里这样写：
``` css
<!-- post.md  -->
When $a \ne 0$, there are two solutions to $ax^2 + bx + c = 0$ and they are

$$x = {-b \pm \sqrt{b^2-4ac} \over 2a}.$$

<script>
if (!window.MathJax || !MathJax.Hub) {
  const script = document.createElement('script');
  script.src='//cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML';
  document.head.appendChild(script);
} else MathJax.Hub.Queue(['Typeset', MathJax.Hub, document.querySelector('main')]);
</script>
```
如果是同步加载的 MathJax 脚本，去掉加载脚本的 js：
``` css
<!-- post.md  -->
When $a \ne 0$, there are two solutions to $ax^2 + bx + c = 0$ and they are

$$x = {-b \pm \sqrt{b^2-4ac} \over 2a}.$$

<script>
window.MathJax && MathJax.Hub && MathJax.Hub.Queue(['Typeset', MathJax.Hub, document.querySelector('main')]);
</script>
```
### 方案2

如果需求很多，不希望 md 里写这么多 js，可以配置成 plugin，默认对所有 post（或 page） 进行渲染：
``` css
plugins:
  - position: post
     template: |
        <script>
        if (!window.MathJax || !MathJax.Hub) {
          const script = document.createElement('script');
          script.src='//cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML';
          document.head.appendChild(script);
        } else MathJax.Hub.Queue(['Typeset', MathJax.Hub, document.querySelector('main')]);
        </script>
```
同方案1，如果是同步加载 MathJax 脚本，去掉多余 js：
``` css
plugins:
  - position: post
    template: |
      <script>
       window.MathJax && MathJax.Hub && MathJax.Hub.Queue(['Typeset', MathJax.Hub, document.querySelector('main')]);
      </script>
```
post 里不需要多写任何 js，所以这种兼容性最好。
``` css
<!-- post.md  -->
When $a \ne 0$, there are two solutions to $ax^2 + bx + c = 0$ and they are

$$x = {-b \pm \sqrt{b^2-4ac} \over 2a}.$$
```
关于为什么第一次打开网页时无法加载公式，因为这个主题是个 SPA，页面渲染是异步的，在 MathJax 脚本加载完成之后，post 还未渲染，所以关键就在使用 `MathJax.Hub.Queue()` 这个方法手动渲染。

示例：https://blog.oniuo.com/post/inside-theme-showcase#MathJax
关于 plugin：https://blog.oniuo.com/theme-inside/docs/misc#plugins

## 菜单栏

### 菜单栏位置: `css\style.css`
```css
.site-top ul li a
```
### 社交图标
就是这个
![logo](https://cdn.jsdelivr.net/gh/starlovei/picgo/Sakura/Snipaste02.jpg)

`C:\Hexo\themes\Sakura\_config.yml`
``` yml
social:
  名称: {url: 链接, img: 图标位置}
```
可以在[ICONS8](https://icons8.cn)里寻找,如果是矢量图标到[阿里巴巴矢量图标库](https://www.iconfont.cn/?spm=a313x.7781069.1998910419.d4d0a486a)最好选择200x200像素

#### 更换图标颜色
PS打开图标
双击图层-选择颜色叠加-选择自己喜欢的颜色-点击确定
<a data-fancybox="gallery" href="https://cdn.jsdelivr.net/gh/starlovei/picgo/Sakura/Snipaste.jpg" id="escape-link"><img src="https://cdn.jsdelivr.net/gh/starlovei/picgo/Sakura/Snipaste.jpg"></a>
可以看到颜色已经改变,保存
<a data-fancybox="gallery" href="https://cdn.jsdelivr.net/gh/starlovei/picgo/Sakura/Snipaste01.jpg" id="escape-link"><img src="https://cdn.jsdelivr.net/gh/starlovei/picgo/Sakura/Snipaste01.jpg"></a>

## 首页打字特效

首先把主题配置里的标题注释掉,然后打开`Sakura\layout\_partial\headertop.ejs`在`<div class="header-info">`后面添加
``` js
			<!-- 首页一言打字效果 -->
			<script src="https://cdn.jsdelivr.net/npm/typed.js@2.0.11/lib/typed.min.js"></script>
			<i class="fa fa-quote-left"></i>
			<span class="element">疯狂造句中......</span>
			<i class="fa fa-quote-right"></i>
			<span class="element"></span>
			<script>
            var typed = new Typed('.element', {
              strings: ["给时光以生命，给岁月以文明", "寒蝉黎明之时,便是重生之日。","当你在凝视着网页的时候,网页也正在凝视着你"], //输入内容, 支持html标签
              typeSpeed: 140, //打字速度
              backSpeed: 50, //回退速度
              loop: false,//是否循环
              loopCount: Infinity,
              showCursor: true//是否开启光标
            });
            </script>
```