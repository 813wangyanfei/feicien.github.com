---
layout: post
title: "Struts2+Hibernate+Spring+Webservice 项目从Tomcat到WebLogic遇到问题的解决方法"
date: 2012-04-26 20:41
comments: true
categories: Java
---
公司一个SSH+Webservice项目开发时是在Tomcat下运行的。却不能部署到WebLogic上。

下面是遇到的问题和相应的解决办法

####一、异常

```
Could not load user defined filter in web.xml: org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter.
ClassNotFoundException:org.apache.velocity.app.VelocityEngine
```
**解决方法：添加2个jar包**

* velocity-tools-2.0.jar 
* velocity-1.7.jar

####二、异常

```
[com.sun.xml.ws.policy.jaxws.BuilderHandler] getPolicySubjects 
严重: [failed to localize] WSP_1014_POLICY_REFERENCE_DOES_NOT_EXIST(zip:D:/bea/weblogic1034/webdomain/servers/AdminServer/tmp/_WL_user/_appsdir_WebServicePro_dir/achpn5/war/WEB-INF/lib/webservices-rt.jar!/WEB-INF/wsdl/wsat.wsdl#Addressing_policy) 
[com.sun.xml.ws.policy.jaxws.PolicyWSDLParserExtension] finished 
严重: [failed to localize] WSP_1014_POLICY_REFERENCE_DOES_NOT_EXIST(zip:D:/bea/weblogic1034/webdomain/servers/AdminServer/tmp/_WL_user/_appsdir_WebServicePro_dir/achpn5/war/WEB-INF/lib/webservices-rt.jar!/WEB-INF/wsdl/wsat.wsdl#Addressing_policy) 
[com.sun.xml.ws.policy.jaxws.PolicyWSDLParserExtension] finished 
严重: [failed to localize] WSP_1018_POLICY_EXCEPTION_WHILE_FINISHING_PARSING_WSDL() 
com.sun.xml.ws.policy.PolicyException: [failed to localize] WSP_1014_POLICY_REFERENCE_DOES_NOT_EXIST(zip:D:/bea/weblogic1034/webdomain/servers/AdminServer/tmp/_WL_user/_appsdir_WebServicePro_dir/achpn5/war/WEB-INF/lib/webservices-rt.jar!/WEB-INF/wsdl/wsat.wsdl#Addressing_policy) 
at com.sun.xml.ws.policy.jaxws.BuilderHandler.getPolicies(BuilderHandler.java:93)
```

**解决办法：删除以下5个jar包**

* webservices-extra-api.jar
* webservices-extra.jar
* webservices-rt.jar
* webservices-tools.jar
* wsdl4j-1.5.1.jar

####三、ClassNotFoundException: org.hibernate.hql.ast.HqlToken(hibernate3 weblogic antrl 问题
**解决办法：在applicationContext.xml中的，设置hibernate.query.factory_class**

```
<property name="hibernateProperties">
<pros>

<prop key="hibernate.query.factory_class">org.hibernate.hql.classic.ClassicQueryTranslatorFactory</prop>
</pros>
</property>
```

####四、日志问题：　