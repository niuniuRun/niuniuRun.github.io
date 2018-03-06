---
title: JSP-EL表达式
date: 2016-12-07 16:07:22
tags:
- EL表达式
- JSP
categories:
- JSP

---

#### 1）语法结构
	${expression}

#### 2）[]与.运算符
      
EL 提供.和[]两种运算符来存取数据。

当要存取的属性名称中包含一些特殊字符，如.或?等并非字母或数字的符号，就一定要使用 []。例如：

	${user.My-Name}应当改为${user["My-Name"] }

如果要动态取值时，就可以用[]来做，而.无法做到动态取值。例如：

	${sessionScope.user[data]}中data 是一个变量

#### 3）变量
EL存取变量数据的方法很简单，例如：${username}。它的意思是取出某一范围中名称为username的变量。

因为我们并没有指定哪一个范围的username，所以它会依序从Page、Request、Session、Application范围查找。

假如途中找到username，就直接回传，不再继续找下去，但是假如全部的范围都没有找到时，就回传null。

属性范围在EL中的名称

	Page            PageScope
	Request         RequestScope
	Session         SessionScope
	Application     ApplicationScope

#### 4）11内置对象

> pageScope : 用于获取page范围的属性值。
> 
> requestScope : 用于获取request范围的属性值。
> 
> sessionScope : 用于获取session范围的属性值。
> 
> applicationScope : 用于获取application范围的属性值。
> 
> param : 用于获取请求的参数值。该内置对象的类型是Map<String,String>，可以用来获取值为单值的请求参数，其中key指的是请求参数的名称，value指的是请求参数的值，使用param获取请求参数与request.getParameter()方法一样。
> 
> paramValues : 用于获取请求的参数值。与param区别在于，该对象用于获取属性值为数组的属性值。该内置对象的类型Map<String,String[]>，可以用来获取值为多值的请求参数，其中key是参数名，value是多个参数值组成的字符串数组。
> 
> header : 用于获取请求头的属性值。该内置对象的类型是Map<String,String>。用法${header.key}
> 
> headerValues : 用于获取请求头的属性值。与header区别在于，该对象用于获取属性值为数组的属性值。该内置对象的类型是Map<String,String[]>。用法${headerValues.key[0...]}
> 
> initParam : 用于获取Web应用的初始化参数。
> 
> cookie : 用于获取指定的cookie值。该内置对象的类型为Map<String,Cookie>。用法${cookie.key}可以获得cookie对象本身
然后获得通过cookie.value获得cookie里面存储的value，简化方法
${cookie.key.value}。
>
> pageContext : 代表该页面的pageContext对象，与JSP的pageContext内置对象相同。用法${pageContext.request}，类似pageContext.getRequest()方法，返回一个request对象。用法二${pageContext.request.contextPath}，获取当前工程的名字。
> >当然，使用pageContext内置对象还可以获取session对象的id值，如：${pageContext.session.id}。pageContext对象可以获取jsp的其他内置对象，所以通过pageContext对象可以获取其他内置对象的任意的属性值。

#### 5）EL表达式的自定义函数

**此方法可参考JSP自定义标签文档中例子**

步骤

1.开发函数处理类，函数处理类就是普通类，这个普通类包含若干个静态方法，每个静态方法都可以定义成一个函数。

2.使用标签库定义函数，定义函数的方法与定义标签的方法大致相似。在<taglib../>元素下增加<function../>元素用于定义自定义函数。

<function../>子元素

> name:指定自定义函数的名称
>
> funciton-class:指定自定义函数的处理类
>
> funciton-signature:指定自定义函数对应的方法。  

代码如下：

	<?xml version="1.0" encoding="GBK"?>
	<taglib xmlns="http://java.sun.com/xml/ns/j2ee"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee 
		web-jsptaglibrary_2_0.xsd" version="2.0">
		<tlib-version>1.0</tlib-version>
		<short-name>crazyit</short-name>
		<!-- 定义该标签库的URI -->
		<uri>http://www.crazyit.org/tags</uri>
		<!-- 定义第一个函数 -->
		<function>
			<!-- 定义函数名:reverse -->
			<name>reverse</name>
			<!-- 定义函数的处理类 -->
			<function-class>lee.Functions</function-class>
			<!-- 定义函数的实现方法-->
			<function-signature>
				java.lang.String reverse(java.lang.String)</function-signature>
		</function>
		<!-- 定义第二个函数: countChar -->
		<function>
			<!-- 定义函数名:countChar -->
			<name>countChar</name>
			<!-- 定义函数的处理类 -->
			<function-class>lee.Functions</function-class>
			<!-- 定义函数的实现方法-->
			<function-signature>int countChar(java.lang.String)
				</function-signature>
		</function>
	</taglib>

调用代码如下：

	......
	<%@ taglib prefix="crazyit" uri="http://www.crazyit.org/tags"%>
	......
		<table border="1" bgcolor="aaaadd">
			<tr>
			<td><b>表达式语言</b></td>
			<td><b>计算结果</b></td>
			<tr>
			<tr>
				<td>\${param["name"]}</td>
				<td>${param["name"]}&nbsp;</td>
			</tr>
			<!--  使用reverse函数-->
			<tr>
				<td>\${crazyit:reverse(param["name"])}</td>
				<td>${crazyit:reverse(param["name"])}&nbsp;</td>
			</tr>
			<tr>
				<td>\${crazyit:reverse(crazyit:reverse(param["name"]))}</td>
				<td>${crazyit:reverse(crazyit:reverse(param["name"]))}&nbsp;</td>
			</tr>
			<!-- 使用countChar函数 -->
			<tr>
				<td>\${crazyit:countChar(param["name"])}</td>
				<td>${crazyit:countChar(param["name"])}&nbsp;</td>
			</tr>
		</table>
	......
