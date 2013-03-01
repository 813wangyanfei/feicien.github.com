---
layout: post
title: "Android使用Loader加载数据"
date: 2012-12-27 10:49
comments: true
categories: Android
---


从android3.0开始，android引入了很多新的api,像fragment，Loader,ViewPager,ActionBar等。今天我通过一个小demo,讲解一下我在使用Loader的一些开发经验，抛砖引玉，激发大家开发Android4.0+应用的兴趣。



这是一个省、市、县三级联动的Demo。



Loader一般是配合ContentProvider一起使用的。



如果你有Java Web的编程经验，你可以把ContentProvider看成DAO(数据访问层)，你提供一个Uri,它给你返回一个Cursor(可以把Cursor理解成一个装了很多数据的集合，类似JDBC中ResultSet).至于数据是放在文件中、数据库中、还是互联网上，你不用关心。



在Activity的onCreate()方法中调用



getLoaderManager.initLoader(int id,Bundle args,LoaderCallbacks&lt;Cursor&gt; callback)便可以初始化一个Loader.



参数说明：一个loader有一个id,如果id相同，便认为是同一个loader.在一个activity中可以创建多个Loader.在demo中，我创建了3个Loader,分别用来加载省份、城市、县的数据。这三个Loader就是用不同的id来标示的。



args 需要传递的参数。比如在demo中选择不通的省份，加载相应城市，我们需要把省份的code,传递到加载城市的Loader中。



callback 回调接口，该接口中，有3个方法需要我们实现。分别是onCreateLoader(),onLoaderFinished(),onLoaderReset(),系统初始化Loader后，会调用首先调用onCreateLoader()
