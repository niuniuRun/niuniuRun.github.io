---
title: JSP基础学习二
date: 2016-07-11 17:47:43
tags: "JSP"
categories: "JSP"
---

# JSP基础语法学习
---

## JSP的3个编译指令  

<table border="1" cellspacing="0"><tr><th width="30%">指令</th ><th width="70%">描述</th ></tr><tr><td width="30%">&lt; %@ page ... % &gt;</td><td width="70%">page指令是针对当前页面的指令，比如脚本语言，错误页面和缓冲要求。</td></tr><tr><td width="30%">&lt; %@ include  ... % &gt;</td><td width="70%">include指令用于指定包含另一个页面</td></tr><td width="30%">&lt; %@ taglib   ... % &gt;</td><td  width="70%">taglib指令用于定义和访问自定义标签</td></tr></table>  
  
  

#### 1.Page编译指令  

   page编译指令常用于JSP页面顶端，用于定义页面一些属性，一个页面可以有多个page指令。

	<%@page
	[language="java"]
	[extends="package.class"]
	[import="package.class | package.* , ..."]
	[session="ture | false"]
	[buffer=" 8kb | none | size kb "]
	[autoFlush="true | false"]
	[isThreadSafe="true | false"]
	[info="text]
	[errorPage="relativeURL"]
	[contentType="mimeType[;charset=characterSet]" | "text/html;charSet=ISO-8859-1"]
	[pageEnconding="ISO-8859-1"]
	[isErrorPage="true | false"]
	%>

* language:声明JSP页面使用的脚本语言环境的种类。默认是JAVA；
* extands：制定JSP页面编译所产生的JAVA类所继承的父类，或者所实现的接口；
* import：用来导入包；默认已经导入的包有：java.lang.\*,javax.servlet.\*,javax.servlet.jsp.\*,javax.servlet.http.\*;
* session:设定是否需要HTTP SESSION；
* buffer：制定缓冲区大小。输出缓冲区的JSP内部对象：out用于缓存JSP页面对客户浏览器的输出，默认为8KB，可以自己设定大小，也可以设置none；
* autoFlush：当输出缓冲区即将溢出时，是否需要强制输出缓冲区的内容；
* info：设置JSP页面信息，可以通过Servlet.getServletInfo()方法获取该值。如果在JSP页面可以直接调用getServletInfo()方法获取；
* errorPage：指定错误处理页面，如果本页面产生了异常或者错误，而该JSP页面没有对应的错误处理代码，则会自动调用该属性所指定的JSP页面，如果不设定，那么系统会把异常信息直接呈现给客户端浏览器；
* isErrorPage:设置本JSP页面是否为错误处理程序页面。如果属性值为true，那么errorPae属性不用指定；
* contentType：用于设定生成网页的文件格式和编码字符集，默认MIMI类型为text/html，字符集为ISO-8859-1；
* pageEnconding：指定生成网页的编码字符集；  

#### 2.Include编译指令  

include指令可以将一个外部文件键入到当前JSP文件中，同时解析这个JSP页面。**但是这是个静态的include语句，他会把目标页面的其他编译指令也包含进来**，如果包含的编译指令冲突，那么页面会出错，静态指令会把目标页面的所有内容都包含进来合成新的页面，因此目标页面可以不需要是完整的页面。

	<%@include file="relativeURLSpec"%>

**注意区别于JSP动作指令中的<jsp:include>，动态包含指令，这个指令不会嵌入目标页面的编译指令**

#### 3.Taglib编译指令 

taglib指令用于定义和访问自定义标签  

## JSP的7个动作指令

JSP动作指令和编译指令不同，编译指令是在JSP变成成servlet是起作用，而动作指令指在运行时动作，其实际只是JSP脚本片段的标准化写法。

* jsp:forward:执行请求转发动作，将请求的处理转发到下一个页面；
* jsp:param：用于传递参数，必须与其他支持参数的动作指令一起使用；
* jsp:include:动态引入一个新的JSP页面；
* jsp:plugin:用于下载JavaBean或Applet到客户端执行;
* jsp:useBean:创建一个JavaBean的实例;
* jsp:setProperty:设置JavaBean实例的属性值;
* jsp:getProperty:输出JavaBean实例的属性值;

#### 1.forward指令

forward指令用于将页面响应转发到另外的页面，浏览器请求地址栏不会改变，页面会转发到目标页面，所以这是一个请求转发。他只是采用了新页面来对用户生成响应，实际http请求只有一次，和重定向的多次请求不同，无论转发多少次，但是该次的请求参数，请求属性都不会改变。

	<jsp:forward page="{relativeURL | <%=expreesion%>}">
	     [<jsp:param name="parameterName" value="patameterValue"/>]
	</jsp:forward>

forward指令可以添加param指令，设置请求参数，然后在转发的目标页面可以用request内置对象调用getParamter()方法获得请求参数的属性值。

#### 2.include指令

include指令是一个动态include指令，可以包含某个页面，但是不同于静态include编译指令，它只包含目标页面的body部分。

	<jsp:include page="{relativeURL | <%=expreesion%>}" flush="true | false">
	     [<jsp:param name="parameterName" value="parameterValue"/>]
	</jsp:include>

flush属性用于指定输出缓存是否转移到被导入文件中。

include也可以对添加请求参数，同forward指令一样，在指定的<font color='red'>**被包含页面**</font>可以调用request内置对象调用getParamter()方法获得请求参数的属性值。

>**无论是include动作指令还是forward动作指令，其实际效果是在servlet的service方法中调用了相应的方法，forward指令调用的是_jspx_page_context的forward()方法来引入目标页面，include指令是调用JspRuntimeLibrary的include()方法来引入目标页面。区别在于，被forward的页面将完全替代原有页面，被include的页面只是插入到原有页面。**

##### 静态include指令和动态include指令的区别

* 静态导入是将被导入页面的代码完全融入，两个页面融合成一个整体Servlet；而动态导入则在Servlet中使用include方法来引入被导入页面的内容；
* 静态导入时被导入页面的编译指令会起作用；而动态导入时被导入页面的编译指令则失去作用，只是插入被导入页面的body内容；
* 动态包含还可以添加额外的参数；

#### 3.useBean,setProperty,getProperty指令

useBean是实例化一个javaBean对象，setProperty是给javaBean对象属性设置值，getProperty是输出javaBean对象的属性值。

useBean语法如下：  

	<jsp:useBean id="name" class="className" scope="page | request | session | application"/>

* id：javaBean的实例名；  
* class：确定javaBean的实现类；  
* scope：用于指定javeBean实例的作用范围，等同于这些方法：pageContext.setAttribute("key命名",javaBean实例化对象);//放入page范围，request.setAttribute("key命名",javaBean实例化对象);//放入request范围，session.setAttribute("key命名",javaBean实例化对象);//放入session范围，application.setAttribute("key命名",javaBean实例化对象);//放入application范围；


>* page：该JavaBean实例紧在该页面有效。
>* request：该JavaBean实例在本次请求有效。
>* session：该JavaBean实例在本次session有效。
>* application：该JavaBean实例在本应用内一直有效。

setProperty语法如下： 
 
	<jsp:setProperty name="BeanName" property="porpertyName" value="value"/>

* name:确定需要设定属性值的javaBean的实例名，等同于useBean指令中id属性的值。
* property:确定需要设定的属性名。
* value：确定需要设定的属性值。

getProperty语法如下： 
 
	<jsp:getProperty name="BeanName" property="porpertyName"/>

* name:确定需要获得属性值的javaBean的实例名，等同于useBean指令中id属性的值。
* property:确定需要获得的属性名。

*关于setProperty和getProperty指令实际上是调用了javaBean类中的setter和getter两个方法，这两个指令他们都需要根据属性名来操作javaBean的属性，其实不然，他们与java类中定义的属性有一定区别，比如setProperty需要使用name属性，但javaBean中是否真正定义了这个成员name属性并不重要，重要的是一定要存在setName这个方法。*

#### 4.plugin指令

plugin指令主要用于下载服务器端的JavaBean或Applet到客户端执行，由于程序在客户端执行，因此客户端必须安装虚拟机，此指令用的场景不多。

#### 5.param指令

param指令必须和forward，include，plugin指令联合使用，用于传递参数。

	<jsp:param name="parameterName" value="parameterValue"/>


   