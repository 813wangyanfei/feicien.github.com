---
layout: post
title: "JAVA实现微信公众账号自定义消息回复"
date: 2012-12-19 12:09
comments: true
categories: Java
---

最近有没有在玩微信公众账号？很多公众账号有自动回复功能，比如爱范儿的appsolution发送Android或者IOS,它就会想你推荐好玩的app.


微信对公众账号提供了api接口来自定义回复介绍到的消息，并且提供了PHP的demo.这里我使用JAVA来实现接口的调用。



###能够实现本demo中描述的功能，你需要满足的下面的条件：

1. 有一个微信公众账号
2. 有能够运行在公网上的服务器（推荐使用新浪的SAE）
3. 了解最基本的JAVA EE编程（会编写servlet）

###(一)注册微信公众账号



微信公众账号注册地址：<a href="http://mp.weixin.qq.com">http://mp.weixin.qq.com</a>。



公众账号需要与你的QQ号进行绑定。并且该QQ没有与其它的微信号绑定过（当然你可以先解除与其它微信的绑定）



###(二)设置公众平台接口信息



注册成功后，进入微信公众账号管理界面，点击设置-&gt;自定义回复-&gt;前往设置。填写相关信息，其中需要注意的是URL和Token. 如果你随便填写URL的话，会提示下面的信息：你的服务器没有正确响应Token验证，请阅读消息接口使用指南。如图所示：



![解析xml](http://huicheng-wordpress.stor.sinaapp.com/uploads/2012/12/QQ20121219-1.png)


这也是为什么要求“有能够运行在公网上的服务器”。本文以新浪的SAE作为服务器环境（好消息，SAE已经开放JAVA应用，不需要满世界找邀请码了），在SAE上创建一个JAVA应用，并上传已经写好的JAVA WEB应用war包，该应用中只有一个Servlet.



其中doGet方法如图所示



![doget](http://huicheng-wordpress.stor.sinaapp.com/uploads/2012/12/QQ20121219-14.png)


这里只是简单的把接收到的echostr字符串原样的返回回去了。这样写是为了能够先通过微信服务器的验证。在SAE上部署好应用后，在回到微信公众账号设置界面，URL添加该servlet的访问地址，token随便填写一个字符串，点击提交。



<!--more-->



出现“提交成功”提示，说明接口配置成功了。如图所示



![提交成功](http://huicheng-wordpress.stor.sinaapp.com/uploads/2012/12/QQ20121219-13.png)





###(三)实现自定义消息回复



微信服务器会在公众账号收到用户消息的时候，把消息通过post方法发送到你的服务器，也就是servlet的doPost方法，如图所示：



<a href="http://huicheng-wordpress.stor.sinaapp.com/uploads/2012/12/QQ20121219-15.png"><img class="aligncenter size-full wp-image-105" title="QQ20121219-15" src="http://huicheng-wordpress.stor.sinaapp.com/uploads/2012/12/QQ20121219-15.png" alt="" width="764" height="621" /></a>



doPost方法中主要实现3个功能：1、从输入流中获取到消息xml。2、通过业务逻辑得到回复的消息xml（具体逻辑后面会讲解）。3、把回复xml写给服务器服务器。



###(四)消息xml的格式



消息分为文本消息、图文消息、位置消息。



文本消息xml格式

<div>

<pre>&lt;xml&gt;

 &lt;ToUserName&gt;&lt;![CDATA[toUser]]&gt;&lt;/ToUserName&gt;

 &lt;FromUserName&gt;&lt;![CDATA[fromUser]]&gt;&lt;/FromUserName&gt;

 &lt;CreateTime&gt;1348831860&lt;/CreateTime&gt;

 &lt;MsgType&gt;&lt;![CDATA[text]]&gt;&lt;/MsgType&gt;

 &lt;Content&gt;&lt;![CDATA[this is a test]]&gt;&lt;/Content&gt;

 &lt;/xml&gt;</pre>

<pre> ToUserName 消息接收方微信号，一般为公众平台账号微信号

 FromUserName 消息发送方微信号

 CreateTime 消息创建时间

 MsgType 文本消息为text

 Content 消息内容</pre>

</div>

####地理位置消息xml格式

<div>

<pre> &lt;xml&gt;

 &lt;ToUserName&gt;&lt;![CDATA[toUser]]&gt;&lt;/ToUserName&gt;

 &lt;FromUserName&gt;&lt;![CDATA[fromUser]]&gt;&lt;/FromUserName&gt;

 &lt;CreateTime&gt;1351776360&lt;/CreateTime&gt;

 &lt;MsgType&gt;&lt;![CDATA[location]]&gt;&lt;/MsgType&gt;

 &lt;Location_X&gt;23.134521&lt;/Location_X&gt;

 &lt;Location_Y&gt;113.358803&lt;/Location_Y&gt;

 &lt;Scale&gt;20&lt;/Scale&gt;

 &lt;Label&gt;&lt;![CDATA[位置信息]]&gt;&lt;/Label&gt;

 &lt;/xml&gt;</pre>

<pre> ToUserName 消息接收方微信号，一般为公众平台账号微信号

 FromUserName 消息发送方微信号

 CreateTime 消息创建时间

 MsgType 消息类型，地理位置为location

 Location_X 地理位置纬度

 Location_Y 地理位置经度

 Scale 地图缩放大小

 Label 地理位置信息</pre>

</div>

####图片消息结构

<div>

<pre> &lt;xml&gt;

 &lt;ToUserName&gt;&lt;![CDATA[toUser]]&gt;&lt;/ToUserName&gt;

 &lt;FromUserName&gt;&lt;![CDATA[fromUser]]&gt;&lt;/FromUserName&gt;

 &lt;CreateTime&gt;1348831860&lt;/CreateTime&gt;

 &lt;MsgType&gt;&lt;![CDATA[image]]&gt;&lt;/MsgType&gt;

 &lt;PicUrl&gt;&lt;![CDATA[this is a url]&gt;&lt;/PicUrl&gt;

 &lt;/xml&gt;</pre>

<pre> ToUserName 消息接收方微信号，一般为公众平台账号微信号

 FromUserName 消息发送方微信号

 CreateTime 消息创建时间

 MsgType 消息类型image

 PicUrl 图片链接，开发者可以用HTTP GET获取</pre>

</div>

&nbsp;



&nbsp;



###(五) 回复消息xml的格式



对于每一个POST请求，开发者在响应包中返回特定xml结构，对该消息进行相应操作（现支持回复文本消息 、 回复图文消息和星标操作）。xml结构如下：

####回复文本消息格式

<div>

<pre> &lt;xml&gt;

 &lt;ToUserName&gt;&lt;![CDATA[toUser]]&gt;&lt;/ToUserName&gt;

 &lt;FromUserName&gt;&lt;![CDATA[fromUser]]&gt;&lt;/FromUserName&gt;

 &lt;CreateTime&gt;12345678&lt;/CreateTime&gt;

 &lt;MsgType&gt;&lt;![CDATA[text]]&gt;&lt;/MsgType&gt;

 &lt;Content&gt;&lt;![CDATA[content]]&gt;&lt;/Content&gt;

 &lt;FuncFlag&gt;0&lt;/FuncFlag&gt;

 &lt;/xml&gt;</pre>

<pre> FromUserName 消息发送方

 ToUserName 消息接收方

 CreateTime 消息创建时间

 MsgType 消息类型，文本消息必须填写text

 Content 消息内容，大小限制在2048字节，字段为空为不合法请求</pre>

</div>

####回复图文消息格式

<div>

<pre> &lt;xml&gt;

 &lt;ToUserName&gt;&lt;![CDATA[toUser]]&gt;&lt;/ToUserName&gt;

 &lt;FromUserName&gt;&lt;![CDATA[fromUser]]&gt;&lt;/FromUserName&gt;

 &lt;CreateTime&gt;12345678&lt;/CreateTime&gt;

 &lt;MsgType&gt;&lt;![CDATA[news]]&gt;&lt;/MsgType&gt;

 &lt;Content&gt;&lt;![CDATA[]]&gt;&lt;/Content&gt;

 &lt;ArticleCount&gt;2&lt;/ArticleCount&gt;

 &lt;Articles&gt;

 &lt;item&gt;

 &lt;Title&gt;&lt;![CDATA[title1]]&gt;&lt;/Title&gt;

 &lt;Description&gt;&lt;![CDATA[description1]]&gt;&lt;/Description&gt;

 &lt;PicUrl&gt;&lt;![CDATA[picurl]]&gt;&lt;/PicUrl&gt;

 &lt;Url&gt;&lt;![CDATA[url]]&gt;&lt;/Url&gt;

 &lt;/item&gt;

 &lt;item&gt;

 &lt;Title&gt;&lt;![CDATA[title]]&gt;&lt;/Title&gt;

 &lt;Description&gt;&lt;![CDATA[description]]&gt;&lt;/Description&gt;

 &lt;PicUrl&gt;&lt;![CDATA[picurl]]&gt;&lt;/PicUrl&gt;

 &lt;Url&gt;&lt;![CDATA[url]]&gt;&lt;/Url&gt;

 &lt;/item&gt;

 &lt;/Articles&gt;

 &lt;FuncFlag&gt;1&lt;/FuncFlag&gt;

 &lt;/xml&gt;</pre>

<pre> FromUserName 消息发送方

 ToUserName 消息接收方

 CreateTime 消息创建时间

 MsgType 消息类型，图文消息必须填写news

 Content 消息内容，图文消息可填空

 ArticleCount 图文消息个数，限制为10条以内

 Articles 多条图文消息信息，默认第一个item为大图

 Title 图文消息标题

 Description 图文消息描述

 PicUrl 图片链接，支持JPG、PNG格式，较好的效果为大图640*320，小图80*80，限制图片链接的域名需要与开发者填写的基本资料中的Url一致

 Url 点击图文消息跳转链接</pre>

</div>

####星标消息

在xml结构中，有一个FuncFlag字段，开发者可以通过填写FuncFlag字段为1来对消息进行星标，你可以在实时消息的<a href="http://mp.weixin.qq.com/cgi-bin/getmessage?t=wxm-message&amp;lang=zh_CN&amp;count=50&amp;star=1">星标消息分类</a>中找到该消息



###(六)自定义消息回复



在doPost方法中的第二步，我们要处理请求的xml得到回复的xml，具体的逻辑是：
1. 解析xml。
2. 保存消息到数据库（便于分析数据，你也可以不保存）。
3. 处理消息。如图所示：



![解析xml](http://huicheng-wordpress.stor.sinaapp.com/uploads/2012/12/QQ20121219-16.png)


1. 解析xml,把解析到的数据保存到HashMap中

![解析xml](http://huicheng-wordpress.stor.sinaapp.com/uploads/2012/12/QQ20121219-17.png)

+ 保存数据到数据库，如果你不想保存到数据库，这一步可以省略

+ 处理消息,不同类型的消息可以通过MsgType来识别


![处理消息](http://huicheng-wordpress.stor.sinaapp.com/uploads/2012/12/QQ20121219-18.png)

+ 处理文本消息，这里演示回应一个文本消息，实际的处理是你的具体业务决定的

![处理文本信息](http://huicheng-wordpress.stor.sinaapp.com/uploads/2012/12/QQ20121219-19.png)


+ 生成xml文件

![生成xml](http://huicheng-wordpress.stor.sinaapp.com/uploads/2012/12/QQ20121219-20.png)

**结束语**：解析与生成xml使用的是dom4j,如果你也使用它，需要添加相关jar包。另外，本人不提供源代码，因为相关的代码我都贴出来了。如果你不懂Java EE的相关编程，给你源代码，你还问我怎么运行呢。欢迎关注我的微博：[http://weibo.com/feicien](http://weibo.com/feicien)