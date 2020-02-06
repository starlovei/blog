---
title: 如何搭建V2Ray
author: 小枫叶
avatar: https://starlovei-1257629504.cos.ap-chengdu.myqcloud.com/Image/78468086_p0.png
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 技术
comments: true
date: 2020-02-05 18:04:05
photos: https://starlovei-1257629504.cos.ap-chengdu.myqcloud.com/Image/ss.jpg
---
## 所需工具:
1:vps一台
2:xshell

### 注册VPS
vps商家有很多: Hostwinds,Dreamhost,vultr,根据需求自行购买,我使用的是vultr.

### 注册账户
打开[官网](https://www.vultr.com/),注册登录账号.
![logo](https://github.com/starlovei/Figure-bed/raw/master/images/20200205190439.png)
选择添加服务器,地点选择Tokyo,系统Debian9x64,服务器大小25 SSD,点击部署(Deploy Now)等待安装完成.

安装完成后点击"管理"就会看到配置信息,把ip地址和密码记下来,接下来会用到.

安装完成后要先ping一下ip,看是否畅通. 方法:按Win和R键,输入cmd.主要看时间那一栏,数字越小延迟越低
![logo](https://github.com/starlovei/Figure-bed/raw/master/images/20200205191122.png)

### 安装xshell
[下载地址](https://www.netsarang.com/zh/xshell/),点击下载按钮,点击"家庭和学校用户的免费许可证"下面"免费授权页面".
![logo](https://github.com/starlovei/Figure-bed/raw/master/images/20200205191700.png)
输入姓名和邮件,勾选只需Xshell,点击下载并安装
![logo](https://github.com/starlovei/Figure-bed/raw/master/images/20200205192003.png)
打开xshell,文件-新建,名称随意,主机填写刚刚部署vps的ip,端口号默认.
![logo](https://github.com/starlovei/Figure-bed/raw/master/images/20200205192341.png)
点击"用户身份验证",用户名填写root,密码填写刚刚vps管理界面的密码.点击连接会出现是否保存密钥,点击保存并连接,连接成功!

### 开始搭建
搭建v2的方法有很多,这里推荐一键脚本,比较方便.
输入
``` txt
bash <(curl -s -L https://git.io/v2ray.sh)
```
回车
![logo](https://github.com/starlovei/Figure-bed/raw/master/images/20200205193305.png)
输入1:安装
![logo](https://github.com/starlovei/Figure-bed/raw/master/images/20200205193421.png)
协议默认Tcp
![logo](https://github.com/starlovei/Figure-bed/raw/master/images/20200205193501.png)
端口默认
是否开启广告拦截,选择否
![logo](https://github.com/starlovei/Figure-bed/raw/master/images/20200205193608.png)
是否配置Shadowsocks,选择否
![logo](https://github.com/starlovei/Figure-bed/raw/master/images/20200205193717.png)
回车安装,需要一点时间
![logo](https://github.com/starlovei/Figure-bed/raw/master/images/20200205193810.png)
出现这个界面说明安装成功
![logo](https://github.com/starlovei/Figure-bed/raw/master/images/20200205193957.png)
输入
``` txt
v2ray url
```
[下载v2ray](https://www.v2ray.com/)
点击从剪贴板添加设置https代理.