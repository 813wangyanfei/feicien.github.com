---
layout: post
title: "创建Android4.0平板模拟器"
date: 2012-04-10 18:36
comments: true
categories: Android
---
今天让我们来创建一个Android4.0的平板电脑模拟器，体验一下它有哪些特性吧。

过程和创建一般模拟器是一样的，如下图所示：

![avd](/images/blog/2012/04/10/avd.png)

1. 输入模拟器的名称
2. 选择target android4.0.3
3. 设置SD卡的大小
4. Skin 选择WXGA800
5. Hardware中的最后一项 默认是1024，在这里我把它改为了512（或者256）。

如果不进行修改，电脑配置比较低的话，是无法启动模拟器的。1024好像代表给模拟器分配1G内存。

刚开始因为不了解这个情况，在Eclipse中，点击avd manager启动刚创建好的模拟器，无法启动，一定反应都没有。

傻傻等了几分钟。

后来在cmd命令行中启动，报了一个异常。google之，才找到原因