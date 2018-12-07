---
layout: post
title: GitHub Pages域名绑定
date: 2018-12-07 08:18:15
categories: test
tags: github 
---

### 1. 创建GitHub Pages

   如果不知道如何创建GitHub Pages，https://pages.github.com/

### 2. 注册域名

   到阿里云或者腾讯云买个自己喜欢的域名（.top域名不能作为腾讯域名邮箱）,以下用 example.com 表示你买的域名

### 3. 到项目的设置中添加刚刚买的域名，推荐下面的方式，不要用新建文件方式，免得出错

   ![](https://img-blog.csdn.net/20180613211251916?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Zsb3dlckRhbmNlMTc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

   往下翻输入你自己的域名，比如：example.com

   ![](https://img-blog.csdn.net/2018061321173766?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Zsb3dlckRhbmNlMTc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

### 4. 域名解析

   到域名控制台（阿里云，腾讯云，或者其他域名供应商）找到域名列表点击解析设置，

很多人在这一步出错，或者设置完了但是访问不了（username.github.io 和 example.com 都无法访问），

<p style='color:red;'>仅仅添加两个CNAME类型即可，不需要管A类型，或者IP什么的</p>

![](https://img-blog.csdn.net/20180613213831352?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Zsb3dlckRhbmNlMTc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

有的旧教程里面各种ping主机，填IP，统统不需要。

主机记录www和@是为了 www.example.com 和 example.com 都能访问到你的页面

这个时候你会发现还是不能访问，因为在等服务商分配DNS（可能不止10分钟），这样配置准没错，去睡个觉醒来就能访问了

