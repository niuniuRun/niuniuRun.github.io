---
title: JSP基础学习一
date: 2016-07-11 16:23:55
tags: "JSP"
categories: "JSP"
---

# JSP基础语法学习
---

## JSP的4种基本语法

##### JSP注释

	<%-- 注释内容 --%>   ---JSP注释语法
	<!-- 注释内容 --%>   ---HTML注释语法
JSP注释标志着JSP容器应该忽略的文本或者语句，可以隐藏相应的JSP语句段，当你不想让其出现在html页面中。当使用浏览器查看源码功能时，html注释内容会显示在浏览器中，而JSP注释这不会，会被隐藏。

##### JSP声明

	<%! declaration; [ declaration; ]+ ... %>
    等价于XML格式
	<jsp:declaration>
       code fragment
	</jsp:declaration> 
JSP声明用于声明变量和方法。相当于JAVA内容的成员变量和成员方法的声明。可以使用public，private等访问控制修饰，也可以使用static修饰，但是**不能使用abstract修饰**，因为抽象方法会导致JSP对应的Servlet变成抽象类，从而导致无法实例化。

##### JSP表达式输出

	<%= expression %>   ---expression后面不能有分号
	等价于XML格式
	<jsp:expression>
       expression
	</jsp:expression>

JSP 表达式元素包含一个脚本语言表达式，该表达式被赋值，转换成一个字符串，并插入到表达式出现在 JSP 文件中的位置。其等价于使用了out.println输出语句。 

根据 JAVA 语言规范，表达式元素可以包含任何有效的表达式，但你不能使用分号来结束一个表达式。

##### JSP脚本

	<% code fragment %>
	等价于XML格式
	<jsp:scriptlet>
	   code fragment
	</jsp:scriptlet>

JSP脚本中的变量应该为局部变量，相当于_jspService的方法内部来执>行JSP脚本片段，所以不能在里面定义方法和成员变量，但是可以使用方法与成员变量。






