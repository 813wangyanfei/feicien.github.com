---
layout: post
title: "JAVA实现微信公众账户主动群发功能"
date: 2012-12-22 13:52
comments: true
categories: Java
---

在上一篇博客中介绍了JAVA实现微信公众账号的自定义消息回复，发到一个技术群中后，有网友问如何实现消息的主动推送？是的，现在微信的公共帐号官方api里并没有提供主动推送功能。

**分析：**既然微信公众平台上可以查询用户列表，可以给用户发送消息，就可以用程序模拟登录，获取到cookie和用户列表，然后主动群发功能，这样还可以解决每天群发的限制。

不过这样是有风险的，可能会导致腾讯的封号。

下面是JAVA实现的原理：



1.获取cookie



微信公众平台的登录验证地址：http://mp.weixin.qq.com/cgi-bin/login?lang=zh_CN

请求方法：post

需要传递的参数：

username：用户名

pwd1:密码的0到15位的MD5值，

pwd2:密码的MD5值

f:json使用json数据

请求这个地址会得到一个名称叫：mp_sid 的cookie

在我测试的过程中发现光有这个这个cookie是不行的，还需要名称为pt2gguin，ts_uid，pgv_pvid，pgv_info=ssid，o_cookie，mp_user的cookie

具体是在那里设置的，我也没有去深究。就直接从浏览器中把它们拷贝并拼接到mp_sid前。



2.请求分组关注列表

请求地址：http://mp.weixin.qq.com/cgi-bin/contactmanagepage?t=wxm-friend&amp;lang=zh_CN&amp;pagesize=10&amp;pageidx=0&amp;type=0&amp;groupid=0

pagesize=10 每页显示的关注人数 可以设置大一些，比如1000

groupid:分组id 0:未分组 1：黑名单 2：星标好友 如果你自己创建有好友，可以在浏览器地址栏看到分组的id



public static ListgetFans(SendHttpRequestter send,

String sessionId) {

//获取html页面

String fansHtml = send.sendPost(FANS_URL, null, sessionId);

//解析json数据

Matcher matcher = Pattern

.compile("\\&lt;script id=\"json-friendList\" type=\"json/text\"\\&gt;(.*?)\\&lt;/script\\&gt;")

.matcher(fansHtml);

String json = "";

while (matcher.find()) {

json = matcher.group(1);

}

Gson gson = new Gson();

return gson.fromJson(json, new TypeToken&lt;List&gt;() {

}.getType());



}

WeiXinFans 类字段

private String fakeId; //发送消息时使用到这个id

private String nickName;//昵称

private String remarkName;//备注名

private int groupId;//分组id

3.向指定的fakeId发送文字消息



static void sendMsg(SendHttpRequestter send, String sessionId, String content,

String fakeId) {



HashMapmap = new HashMap();

map.put("tofakeid", fakeId);//tofakeid 第二步中获取的id

map.put("content", content);//要发送的内容

map.put("error", "false");//

map.put("type", "1");//

map.put("ajax", "1");//



String html = send.sendPost(SEND_MSG_URL, map, sessionId);

System.out.println(html);

}

发送地址SEND_MSG_URL：http://mp.weixin.qq.com/cgi-bin/singlesend?t=ajax-response&amp;lang=zh_CN

4.群发

既然可以发送一条，使用一个for循环，不就可以群发了。

结束语：SendHttpRequestter 这个类里面有个人信息，源码就不提供了，里面有2个方法。

一个获取cookie的方法，一个发送post请求的方法，网上都可以找到的。