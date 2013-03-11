---
layout: post
title: "Hibernate4.1.2配置"
date: 2012-04-20 16:08
comments: true
categories: Java
---

```

<?xml version='1.0' encoding='utf-8'?>
<!DOCTYPE hibernate-configuration PUBLIC
        "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">

<hibernate-configuration>
    <session-factory>
        <!-- 数据库的配置 -->
        <property name="hibernate.bytecode.use_reflection_optimizer">false</property>
        <property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
        <property name="hibernate.connection.url">jdbc:mysql://localhost:3306/hibernate</property>
        <property name="hibernate.connection.username">root</property>
        <property name="hibernate.connection.password">123456</property>
        <!-- SQL dialect 数据库方言，这里我们用MySQL -->
        <property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect</property>
        <!-- 设置show_sql为true表示让hibernate将生成sql语句在控制台打印出来 -->
        <property name="show_sql">true</property>
        <!-- Drop and re-create the database schema on startup 是否让hibernate自动为我们创建表 -->
        <property name="hbm2ddl.auto">update</property>
        <property name="javax.persistence.validation.mode">none</property>
        <mapping class="com.loveplusplus.bean.Category"/>
        <mapping class="com.loveplusplus.bean.Goods"/>
    </session-factory>

</hibernate-configuration>

```
