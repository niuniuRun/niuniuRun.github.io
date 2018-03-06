---
title: Spring学习一
date: 2016-12-06 15:35:26
tags:
- spring 
- J2EE
categories: 
- J2EE
---

## Spring 基础学习一

spring在J2EE中应用最多的属于WebApplicationContext类，此类是spring IOC容器ApplicaionContext的子类，专门为web应用准备，它允许从相对于web根目录的路径中装载配置文件完成初始化工作。从WebApplicationContext中可以获得ServletContext的引用，而且WebApplicationContext对象也将作为属性放到ServletContext中，可以使用WebApplicationContextUtil的getWebApplicationContext（ServletContex sc）方法获得WebApplicationContext的实例。

	WebApplicationContext wac=（WebApplicationContext）servletContext.getAttribute(WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE)；

此方法可以从ServletContext属性中通过，WebApplicationContext类定义的常量ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE来获得相应的实例，此方法也是WebApplicationContextUtil获得Web应用上下文实例的内部实现方法。

### WebApplicationContext初始化

WebApplicationContext的初始化需要ServletContext的引用，所以必须在拥有Web容器的前提下才能完成启动工作。

因此可以通过定义自启动Servlet或者定义Web容器监听器来，完成WebApplicationContext的初始化工作。

Spring提供了ContextLoaderServlet或者ContextLoaderListener来辅助WebApplicationContext的初始化，这两类中都实现了启动WebApplicationContext实例的逻辑，我们只需要在Web.xml中配置就可以了。

	<!--指定配置文件-->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>
			/WEB-INF/baobaotao-dao.xml,/WEB-INF/baobaotao-service.xml
		</param-value>
	</context-param>

	<!--声明Web容器监听器-->
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>

ContextLoaderListener通过Web容器上下文参数contextConfigLocation获取Spring配置文件位置，可以指定多个配置文件，用逗号，空格，或冒号分开均可，对于未带资源类型前缀的配置文件路径，会默认这些路径相对于Web应用部署的根路径，对于配置了资源类型前缀的路径，和上面是等效的，例如 `classpath*：/baobaotao-*.xml`

##### 通过自启动servlet来引导

	<!--指定配置文件-->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>
			/WEB-INF/baobaotao-dao.xml,/WEB-INF/baobaotao-service.xml
		</param-value>
	</context-param>

	<!--声明自启动Servlet-->
	<servlet>
		<servlet-name>springContextLoaderServlet</listener-class>
		<servlet-class>org.springframework.web.context.ContextLoaderServlet</servlet-class>
		<load-on-startup>1</load-on-startup>
	</servlet>


---