---
layout: post
title: "Android Http Client"
date: 2012-03-18 21:13
comments: true
categories: Android
---
大部分Android应用程序联网都是通过HTTP来发送和接受数据的。Android有2个Http客户端：HttpURLConnection和Apache Http Client.它们都支持Https,流媒体的上传下载，配置连接超时，ipv6和连接池。

那么在我们的程序中选择哪一个好呢？

通过Android的官方博客：[blogspot博客，需要翻墙](http://android-developers.blogspot.com/2011/09/androids-http-clients.html) 如果你的应用api版本2.3及以上HttpURLConnection是更好的选择。

下面通过代码来演示HttpURLConnection的使用。


```
//获取html源代码
    public static String getHtml(String path) throws Exception {
        URL url=new URL(path);
        HttpURLConnection conn=(HttpURLConnection)url.openConnection();
        
        conn.setConnectTimeout(5000);
        conn.setRequestMethod("GET");
        
        if(conn.getResponseCode()==200){
            InputStream is=conn.getInputStream();
            byte[] data=read(is);
            String html=new String(data,"UTF-8");
            return html;
        }
        return null;
    }
    
```



```

//将输入流读到内存中
    public static byte[] read(InputStream is) throws IOException {
        ByteArrayOutputStream baos=new ByteArrayOutputStream();
        
        byte[] buffer =new byte[1024];
        int len=0;
        while((len=is.read(buffer))!=-1){
            baos.write(buffer, 0, len);
        }
        is.close();
        return baos.toByteArray();
    }
    
```