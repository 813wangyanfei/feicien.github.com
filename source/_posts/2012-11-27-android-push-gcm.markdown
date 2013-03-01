---
layout: post
title: "Android服务器推送之GCM"
date: 2012-11-27 11:56
comments: true
categories: Android 
---


GCM(Google Cloud Message for Android)是Google发布的Android服务器推送（push）技术。之前的C2DM(Android Cloud to Device Messaging)已与2012年6月26日被正式弃用，使用GCM,需要申请开通Google apis,Google apis包括了所有Google服务的api,比如Google Map、Google+、Analytics、YouTube等等，申请地址为：
[Google API 网站](https://code.google.com/apis/console)



###GCM有以下特点：
1. 可以使用第三方应用服务器向Android应用推送消息
2. GCM不保证发送的消息的顺序，也不保证消息一定能够推送到手机（恩，谁也不能保证100%）
3. Android应用不需要运行就可以接收消息（是的，你没有看错，因为gcm被集成到系统中了，目前一些第三方的推送是在后台运行一个service维持长连接，与这些第三方推送相比，GCM不额外的耗电）
4. GCM只传递的数据（可以传递小于4kb的数据），对这些数据的处理可以全部由开发者控制（Google不对数据进行任何处理，仅仅转发一下而已）
5. 对于Android4.04以上的系统使用GCM没有任何限制（国行手机也可以使用，截止2013年02月04日，android4.0+的份额为42.6%，随着4.0+版本的提升，Android的推送不再成为一个问题 [Android版本分布](https://developer.android.com/about/dashboards/index.html)），Android2.2以上的系统需要安装Google Play Store，Android2.2以下的系统不能够使用GCM
6. 对于Android3.0以前的系统，需要在设备上设置google账号
7. gcm一次最多只能向1000部设备发送消息，没有提供向所有用户发送的接口（可能google认为向应用程序推送的消息都是与该用户相关的，如果你非要实现群发，一次发送1000个用户，多发送几次就行了）


###GCM使用流程如下图所示：


![gcm使用流程图](http://huicheng-wordpress.stor.sinaapp.com/uploads/2012/11/QQ20121127-1-300x241.png) 

**App**是运行在Android手机上的应用程序，**GCM Server**是Google的GCM服务器，**Our Server**是第三方应用服务器。具体流程如下：

1. **App**发送 **SenderID**到**GCM Server**注册接收推送信息(SendID是开发者在Google的网站开通GCM服务时，创建项目的项目号)。
2. **GCM Server** 向**App**返回RegId(RegId是GCM服务器通过一定算法生产的，可以唯一确定某一部手机上的某一个应用，这个RegId很重要)。
3. **App**向**Our Server**发送RegId(推送消息的时候要使用RegId，GCM服务器是使用RegId来确定某一部手机上的某一个应用接收消息的，所以第三方服务器需要保存它，需要注意的是RegId很长，比如可能有183位，存数据库时需要注意字段长度)
4. **Our Server**向**GCM Server**发送消息,传递appkey和RegId(appkey分为Oauth api key和simple api key)
5. **GCM Server**把消息推送给**App**



更多信息，请访问[Android开发网站](https://developer.android.com/guide/google/gcm/gs.html)，上面有快速开发指导，客户端与服务端集成介绍，和Demo下载。