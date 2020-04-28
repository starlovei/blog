---
title: Sakura主题折腾日记
author: 小枫叶
avatar: https://cdn.jsdelivr.net/gh/starlovei/picgo/Image/78468086_p0.png
tags:
  - 主题
  - 网络
date: 2020-04-27 19:23:53
photos: https://cdn.jsdelivr.net/gh/starlovei/picgo/Sakura-theme/photos.jpg
categories: 技术
mathjax:
description: 日更状态...
incomplete: true
---
> 记录一下博客瞎折腾过程,以及遇到过的bug,借用了不少大佬的灵感

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
## 归档页面时间轴颜色
找了好久才找到
在`Sakura/source/css/style.css`里
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
一些特殊节日有些网站会把网站变成灰色,同样在`Sakura/source/css/style.css`添加
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
**方案1：**
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
**方案2**
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