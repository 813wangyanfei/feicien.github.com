---
layout: post
title: "Tomcat 配置JDBC数据源datasource"
date: 2012-05-08 18:07
comments: true
categories: Java
---
####1. 编辑Tomcat安装目录conf文件夹中server.xml,在</Host>标签前添加

```
<Context  path="/chaoshi" 
　　　　　　docBase="chaoshi"
　　　　　　debug="5" 
　　　　　　reloadable="true" 
　　　　　　crossContext="true"   
　　　　　　workDir="">
             <Resource  name="jdbc/mydatasource"
                        auth="Container"
　　　　　　　　　　　　　　type="javax.sql.DataSource"
                 　 　　 maxActive="100" 
　　　　　　　　　　　　　　maxIdle="30" 
　　　　　　　　　　　　　　maxWait="10000"
                  　　　 username="root" 
　　　　　　　　　　　　　　password="123456"
　　　　　　　　　　　　　　driverClassName="com.mysql.jdbc.Driver"
　　　　　　　　　　　　　　url="jdbc:mysql://localhost:3306/mydatabase"/>
</Context>
```
####2. 在应用的WEB-INF目录下web.xml文件中添加如下配置

```
<resource-ref>
　　<description>DB Connection</description>
　　<res-ref-name>jdbc/mydatasource</res-ref-name>
　　<res-type>javax.sql.DataSource</res-type>
　　<res-auth>Container</res-auth>
</resource-ref>
```
####3. 在JSP或Servlet或JavaBean中用如下Java代码获得数据库连接

```
Context initial = new InitialContext(); 
//其中mysql为数据源jndi名称 
DataSource ds = (DataSource)initial.lookup("java:comp/env/jdbc/mydatasource");
Connection con=ds.getConnection();
```