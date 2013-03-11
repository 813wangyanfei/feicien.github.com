---
layout: post
title: "Android使用style实现TextView圆角效果"
date: 2011-11-27 13:09
comments: true
categories: Android
---
先上效果图：

![demo](/images/blog/2011/11/textview_round.png)

新建一个Android项目
main.xml如下：

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:background="#ffffff"
    android:orientation="vertical" >

    <LinearLayout
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_marginLeft="10.0dip"
        android:layout_marginRight="10.0dip"
        android:layout_marginTop="10.0dip"
        android:background="@anim/shape_rounded_rectangle"
        android:orientation="vertical" >

        <RelativeLayout
            android:layout_width="fill_parent"
            android:layout_height="45.0dip"
            android:background="@anim/more_up"
            android:gravity="center_vertical" >

            <TextView
                style="@style/style_16_666666_BOLD"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_centerVertical="true"
                android:layout_marginLeft="15.0dip"
                android:text="账号管理" />

            <ImageView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_alignParentRight="true"
                android:layout_centerVertical="true"
                android:layout_marginRight="20.0dip"
                android:src="@drawable/arrow" />
        </RelativeLayout>

        <ImageView
            android:layout_width="fill_parent"
            android:layout_height="wrap_content"
            android:background="@drawable/line" />

        <RelativeLayout
            android:id="@+id/relMoreSet"
            android:layout_width="fill_parent"
            android:layout_height="45.0dip"
            android:background="@anim/more_down"
            android:gravity="center_vertical" >

            <TextView
                style="@style/style_16_666666_BOLD"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_centerVertical="true"
                android:layout_marginLeft="15.0dip"
                android:text="设置" />

            <ImageView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_alignParentRight="true"
                android:layout_centerVertical="true"
                android:layout_marginRight="20.0dip"
                android:src="@drawable/arrow" />
        </RelativeLayout>
    </LinearLayout>
</LinearLayout>

```

Demo下载地址
http://download.csdn.net/detail/love7323315/4785755