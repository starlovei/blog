---
title: Windows-Terminal
date: 2021-01-24 09:43:25
author: 小枫叶
avatar: 'https://cdn.jsdelivr.net/gh/starlovei/cdn/img/avatar/01.jpg'
photos: https://cdn.jsdelivr.net/gh/starlovei/picgo/Windows-Terminal/yzavvq4we0ao5akst1d2.png
categories: 技术
tags: 主题
---

> Windows Terminal是微软公司于西雅图开幕的Build 2019大会上所公布的面向Windows10的新命令行程序,系统自带的powershell颜值实在有些不敢恭维

我们可以通过[Microsoft应用商店](https://www.microsoft.com/zh-cn/p/windows-terminal/9n0dx20hk701?activetab=pivot:overviewtab)或者[Github](https://github.com/microsoft/terminal/releases)下载安装



## Terminal 配置

下拉菜单打开设置(Terminal配置文件)

![](https://cdn.jsdelivr.net/gh/starlovei/picgo/Windows-Terminal/2021-01-24_09-40-02.jpg)

配置文件大概分为四种类型:**全局设置**,**终端配置**,**主题配置**以及**快捷键**



### 全局设置



也就是最前面的一部分,列举几个比较重要的设置

+ 默认配置: 即打开自动进入的环境,通过GUID识别
  + 属性名称: `defaultProfile`
+ 深色/浅色主题
  + 属性名称: `theme`
  + 分为`system`(跟随系统),`light`和`dark`
+ 启动大小: 
  + 属性名称: launchMode
  + 分为`default` ,`maximized`, `fullscreen`,默认为`default`



### 终端配置



下拉菜单的不同终端入口,整体分为`defaults`和`list`

+ 默认配置"defaults": 如果你想要应用于所有配置文件可以在这里添加
+ 单独配置"list": 单独对某个环境配置

Windows Terminal可以自定义坏境

+ guid(必需): 配置文件可将 GUID 用作唯一标识符。 若要将某个配置文件设置为默认配置文件，则需要 `defaultProfile` 全局设置的 GUID。
  + 如果你想要新建一个环境必须设置guid
+ commandline: 执行路径例如"cmd.exe".如果没有配置坏境变量需要填写绝对路径
+ name: 名称
+ hidden: 是否在下拉菜单隐藏配置
+ fontface: 字体
+ backgroud: 背景颜色
+ backgroundImage: 背景图片
+ useAcrylic: 亚克力效果
+ colorScheme: 主题设置,填写主题配置中的name



### 主题配置



主题配置定义在schemes,通过以下格式写入:

``` json
{
    "name" : "Campbell",

    "cursorColor": "#FFFFFF",
    "selectionBackground": "#FFFFFF",

    "background" : "#0C0C0C",
    "foreground" : "#CCCCCC",

    "black" : "#0C0C0C",
    "blue" : "#0037DA",
    "cyan" : "#3A96DD",
    "green" : "#13A10E",
    "purple" : "#881798",
    "red" : "#C50F1F",
    "white" : "#CCCCCC",
    "yellow" : "#C19C00",
    "brightBlack" : "#767676",
    "brightBlue" : "#3B78FF",
    "brightCyan" : "#61D6D6",
    "brightGreen" : "#16C60C",
    "brightPurple" : "#B4009E",
    "brightRed" : "#E74856",
    "brightWhite" : "#F2F2F2",
    "brightYellow" : "#F9F1A5"
},
```

Terminal自带了几种主题,放置在defaults.json文件中

##### Campbell

![](https://cdn.jsdelivr.net/gh/starlovei/picgo/Windows-Terminal/campbell-color-scheme.png)

##### Campbell Powershell

![](https://cdn.jsdelivr.net/gh/starlovei/picgo/Windows-Terminal/campbell-powershell-color-scheme.png)



快捷键设置暂时先不讲



## 进阶美化

> 虽然以上方法能够很好的美化powershell，但是代码栏依然还是老样式

接下来我们要用到`oh-my-posh`这个插件啦，这是一个开源的powershell主题，[Github项目地址](https://github.com/JanDeDobbeleer/oh-my-posh)



### 注意事项

oh-my-posh主题使用了一些特殊字符，如果你用默认的字体会出现乱码

![](https://cdn.jsdelivr.net/gh/starlovei/picgo/Windows-Terminal/2021-02-12_08-06-57.jpg)

Powerline字体下载：https://github.com/powerline/fonts



### 下载与安装

安装`posh-git`和`oh-my-posh`

``` shell
Install-Module posh-git -Scope CurrentUser 
Install-Module oh-my-posh -Scope CurrentUser
```

编辑Powershell配置文件，这样就可以在每次启动Powershell的时候加载主题。

``` shell
#新增配置文件
if (!(Test-Path -Path $PROFILE )) { New-Item -Type File -Path $PROFILE -Force }
#用记事本打开文件
notepad $PROFILE
```

添加下面的内容：

``` shell
Import-Module posh-git 
Import-Module oh-my-posh 
Set-Theme Paradox
```

最后一句的`Set-Theme <主题名>`主题选项，可以选择喜欢的主题，例如Paradox：

![](https://cdn.jsdelivr.net/gh/starlovei/picgo/Windows-Terminal/paradox.png)

当然这个命令只是在当前窗口更换主题，如果你要使用主题需要在配置文件的主题选项里修改。

### 定制主题

oh-my-posh内置有十个主题，配置文件的位置在`C:\Users\<用户名>\Documents\WindowsPowerShell\Modules\oh-my-posh\<版本号>\Themes`，具体可以参考官方文档。或者你也可以自己写一个喜欢的主题，在主题文件夹里新建<主题名.psm1>然后按照其他主题的方法修改，我自己魔改了一下[spencerwooo大佬的主题](https://github.com/spencerwooo/dotfiles)存放在[Github](https://github.com/starlovei/Terminal-Theme),感兴趣的朋友可以下载

![](https://cdn.jsdelivr.net/gh/starlovei/picgo/Windows-Terminal/2021-02-12_08-36-24.jpg)

