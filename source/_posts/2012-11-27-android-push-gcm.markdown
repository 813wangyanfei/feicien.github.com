---
layout: post
title: "Android服务器推送之GCM"
date: 2012-11-27 11:56
comments: true
categories: 
---


GCM(Google Cloud Message for Android)是Google发布的Android服务器推送技术。之前的C2DM(Android Cloud to Device Messaging)已与2012年6月26日被正式弃用，申请使用GCM,请访问<a href="https://code.google.com/apis/console">Google apis网站</a>。



GCM有以下特点：

<ol>

	<li>可以使用第三方应用服务器向Android应用推送消息</li>

	<li>GCM不保证发送的消息的顺序，也不保证消息一定能够推送到手机</li>

	<li>Android应用不需要运行就可以接收消息（目前一些第三方的推送是在后台运行一个service维持长连接，与这些第三方推送相比，GCM不额外的耗电）</li>

	<li>GCM只传递的数据（可以传递小于4kb的数据），对这些数据的处理可以全部由开发者控制</li>

	<li>对于Android4.04以上的系统使用GCM没有任何限制，Android2.2以上的系统需要安装Google Play Store，Android2.2以下的系统不能够使用GCM</li>

	<li>对于Android3.0以前的系统，需要在设备上设置google账号</li>

	<li>可以同时向1000部设备发送消息</li>

</ol>

GCM使用流程如下图所示：



[caption id="attachment_93" align="aligncenter" width="300"]<img class="size-medium wp-image-93 aligncenter" title="GCM" src="http://huicheng-wordpress.stor.sinaapp.com/uploads/2012/11/QQ20121127-1-300x241.png" alt="gcm" width="300" height="241" />                       gcm使用流程图[/caption]



App是运行在Android手机上的应用程序，GCM Server 是Google的GCM服务器，Our Server是第三方应用服务器。具体流程如下：

<ol>

	<li>App发送 SenderID到GCM Server注册接收推送信息(SendID是开发者在Google的网站开通GCM服务时，创建项目的项目号)。</li>

	<li>GCM Server 向App返回RegId(RegId是GCM服务器通过一定算法生产的，可以唯一确定某一部手机上的某一个应用，这个RegId很重要)。</li>

	<li>App向Our Server发送RegId(推送消息的时候要使用RegId，GCM服务器是使用RegId来确定某一部手机上的某一个应用接收消息的，所以第三方服务器需要保存它，需要注意的是RegId很长，比如可能有183位，存数据库时需要注意字段长度)</li>

	<li>Our Server向GCM Server发送消息,传递appkey和RegId(appkey分为Oauth api key和simple api key)</li>

	<li>GCM把消息推送给App</li>

</ol>

更多信息，请访问<a title="GCM官方网站" href="https://developer.android.com/guide/google/gcm/gs.html" target="_blank">Android开发网站</a>，上面有快速开发指导，客户端与服务端集成介绍，和Demo下载。