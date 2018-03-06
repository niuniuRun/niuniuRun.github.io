---
title: JSP自定义标签
date: 2016-12-07 13:56:01
tags:
- tag
- 标签
categories:
- JSP

---

# JSP自定义标签
---

### JSP自定义标签一：

JSP自定义标签分为三步

> 1.开发自定义标签处理类；
> 
> 2.建立一个\*.tld文件，每个\*.tld文件对应一个标签库，每个标签库可包含多个标签；
> 
> 3.在JSP文件中使用自定义标签；

首先我们需要大致了解开发自定义标签所涉及到的接口与类的层次结构(其中SimpleTag接口与SimpleTagSupport类是JSP2.0中新引入的)。

![](/images/2011122517261812.png)

####1.1开发自定义标签类

自定义标签类应该继承一个父类：SimpleTagSupport类

> 如果标签类包含属性，每个属性都要有对应的getter和setter方法

> 重写doTag()方法，这个方法负责生成页面内容。

	public class HelloWordTag extends SimpleTagSupport
	{
		//重写doTag方法，该方法负责生成页面内容
		public void daTag() throws JspException,IOException
		{
			//获取页面输出流，并输出字符串
			getJspContext().getOut().write("Hello Word!");
		}
	}

####1.2建立TLD文件

	<?xml version="1.0" encoding="GBK"?>
	<taglib xmlns="http://java.sun.com/xml/ns/j2ee"
			xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
			xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee web-jsptaglibrary_2_0.xsd"
			version="2.0">
		<tlib-version>1.0</tlib-version>
		<short-name>mytaglib</short-name>
		<!-- 定义该标签库的URI -->
		<uri>http://www.crazyit.org/mytaglib</uri>

		<!-- 定义第一个标签 -->
		<tag>
			<!-- 定义标签名 -->
			<name>helloWorld</name>
			<!-- 定义标签处理类 -->
			<tag-class>lee.HelloWorldTag</tag-class>
			<!-- 定义标签体为空 -->
			<body-content>empty</body-content>
		</tag>

		<!-- 定义第二个标签 -->
		<tag>
			<!-- 定义标签名 -->
			<name>query</name>
			<!-- 定义标签处理类 -->
			<tag-class>lee.QueryTag</tag-class>
			<!-- 定义标签体为空 -->
			<body-content>empty</body-content>
			<!-- 配置标签属性:driver -->
			<attribute>
				<name>driver</name> 
				<required>true</required>
				<fragment>true</fragment>
			</attribute>
			<!-- 配置标签属性:url -->
			<attribute>
				<name>url</name> 
				<required>true</required>
				<fragment>true</fragment>
			</attribute>
			<!-- 配置标签属性:user -->
			<attribute>
				<name>user</name> 
				<required>true</required>
				<fragment>true</fragment>
			</attribute>
			<!-- 配置标签属性:pass -->
			<attribute>
				<name>pass</name> 
				<required>true</required>
				<fragment>true</fragment>
			</attribute>
			<!-- 配置标签属性:sql -->
			<attribute>
				<name>sql</name> 
				<required>true</required>
				<fragment>true</fragment>
			</attribute>
		</tag>

		<!-- 定义第三个标签 -->
		<tag>
			<!-- 定义标签名 -->
			<name>iterator</name>
			<!-- 定义标签处理类 -->
			<tag-class>lee.IteratorTag</tag-class>
			<!-- 定义标签体不允许出现JSP脚本 -->
			<body-content>scriptless</body-content>
			<!-- 配置标签属性:collection -->
			<attribute>
				<name>collection</name> 
				<required>true</required>
				<fragment>true</fragment>
			</attribute>
			<!-- 配置标签属性:item -->
			<attribute>
				<name>item</name> 
				<required>true</required>
				<fragment>true</fragment>
			</attribute>
		</tag>

		<!-- 定义接受动态属性的标签 -->
	    <tag>
	        <name>dynaAttr</name>
	        <tag-class>lee.DynaAttributesTag</tag-class>
	        <body-content>empty</body-content>
			<!-- 指定支持动态属性 -->
	        <dynamic-attributes>true</dynamic-attributes>
	    </tag>

	    <tag>
	        <name>fragment</name>
	        <tag-class>lee.FragmentTag</tag-class>
	        <body-content>empty</body-content>
	        <attribute>
	            <name>fragment</name>
	            <required>true</required>
	            <fragment>true</fragment>
	        </attribute>
	    </tag>
	</taglib>

调用方法如下：

	<h2>下面显示的是自定义标签中的内容</h2>
	<!-- 使用标签 ，其中mytag是标签前缀，根据taglib的编译指令，mytag前缀将由http://www.crazyit.org/mytaglib的标签库处理 -->
	<mytag:helloWorld/><br/>

taglib下的子元素

> Element：Description
> 
> tlib-version：Tag库的版本
> 
> jsp-version：Tag库所需要的jsp的版本
> 
> <font color='red'>short-name：助记符，tag的一个别名（可选）</font>
> 
> <font color='red'>uri：用于确定一个唯一的tag库(重要)</font>
> 
> display-name：被可视化工具（诸如Jbuilder）用来显示的名称（可选）
> 
> small-icon：被可视化工具（诸如Jbuilder）用来显示的小图标（可选）
> 
> large-icon：被可视化工具（诸如Jbuilder）用来显示的大图标（可选）
> 
> description：对tag库的描述（可选）
> 
> listener：一个tag库可能定义一些类做为它的事件侦听类，这些类在TLD中被称为listener 元素，jsp服务器将会实例化这些侦听类，并且注册它们。Listener元素中有一个叫listener-class的子元素，这个元素的值必须是该侦听类的完整类名。
> 
> tag：参见下面tag 元素

tag元素下子元素

><font color='red'> name：该标签库的名称，JSP页面中就是根据这个名称来使用此标签。(重要)</font>
> 
> <font color='red'>tag-class：Tag标签对应的tag处理类(重要)</font>
> 
> tei-class：javax.servlet.jsp.tagext.TagExtraInfo的子类，用于表达脚本变量（可选）
> 
> <font color='red'>body-content：Tag标签body的类型，指定标签体的内容，可以是如下几个(重要)</font>
> > tagdependent-指定标签处理类自己负责处理标签体
> 
> > empty-指定该标签体为空，只能作为空标签使用
> 
> > scriptless-指定该标签体可以试静态HTML元素，表达式语言，但不允许出现JSP脚本
> 
> > JSP-指定该标签的标签体可以使用JSP脚本
> 
> > dynamic-attributes-指定该标签是否支持动态属性，只有当定义动态属性标签时才需要该子元素’(重要)</font>
> 
> display-name：被可视化工具（诸如Jbuilder）用来显示的名称（可选）
> 
> small-icon：被可视化工具（诸如Jbuilder）用来显示的小图标（可选）
> 
> large-icon：被可视化工具（诸如Jbuilder）用来显示的大图标（可选）
> 
> description：此tag标签的描述
> 
> variable：提供脚本变量的信息（同tei-class）(可选)
> 
> <font color='red'>attribute：Tag标签的属性名,有如下子元素(重要)</font>
> > name : 设置属性名，子元素的值是字符串内容。
> 
> > required : 设置该属性是否为必须属性，该子元素的值是true或false。
> 
> > fragment : 设置该属性是否支持JSP脚本，表达式等动态内容，子元素的值是true或false。

#### 1.3使用标签库

1.使用taglib编译指令导入标签库。

2.使用标签，在JSP页面中使用自定义标签。

引入自定义标签语法如下

	<%@ taglib uri="tagliburi" prefix="tagPrefix"%>

使用自定义标签语法如下

	<tagPrefix:tagName tagAttribute="tagValue" ...>
		<tagBody/>
	</tagPrefix:tagName>

	<!--没有标签体的-->
	<tagPrefix:tagName tagAttribute="tagValue" .../>


###### 带属性的标签调用方法如下

	<h2>下面显示的是查询标签的结果</h2>
	<!-- 使用标签 ，其中mytag是标签前缀，根据taglib的编译指令，mytag前缀将由http://www.crazyit.org/mytaglib的标签库处理 -->
	<mytag:query
		driver="com.mysql.jdbc.Driver"
		url="jdbc:mysql://localhost:3306/javaee"
		user="root"
		pass="32147"
		sql="select * from news_inf"/><br/>

###### 带有标签体的处理类

	//省略了其他内容
	......
	
	//标签的处理方法，简单标签处理类只需要重写doTag方法
	public void doTag() throws JspException, IOException
	{
		//从page scope中获取属性名为collection的集合
		Collection itemList = (Collection)getJspContext().
			getAttribute(collection);
		//遍历集合
		for (Object s : itemList)
		{
			//将集合的元素设置到page 范围
			getJspContext().setAttribute(item, s );
			//输出标签体
			getJspBody().invoke(null);
		}
	}

	......

上面代码每次遍历都调用了getJspBody()方法，该方法返回该标签所包含的标签体：JspFragment对象，执行该对象的invoke()方法，即可输出标签体内容。

###### 以页面片段作为属性的标签

> 标签处理类中定义了类型为JspFragment的属性，该属性代表了‘页面片段’。

> 使用标签库时，通过<jsp:attribute../>动作指令为标签库属性指定值。

参考如下代码

	public class FragmentTag extends SimpleTagSupport 
	{
		private JspFragment fragment;
		
		//fragment属性的setter和getter方法
		public void setFragment(JspFragment fragment)
		{
			this.fragment = fragment;
		}
		public JspFragment getFragment()
		{
			return this.fragment;
		}
		@Override
		public void doTag() throws JspException, IOException
		{
			JspWriter out = getJspContext().getOut();
			out.println("<div style='padding:10px;border:1px solid black'>");
			out.println("<h3>下面是动态传入的JSP片段</h3>");
			//调用、输出“页面片段”
			fragment.invoke( null );
			out.println("</div");
		}
	}

tld文件如下

	......
	<tag>
		<!--定义标签名-->
        <name>fragment</name>
		<!--定义标签处理类-->
        <tag-class>lee.FragmentTag</tag-class>
		<!--指定该标签不支持标签体-->
        <body-content>empty</body-content>
        <attribute>
            <name>fragment</name>
            <required>true</required>
            <fragment>true</fragment>
        </attribute>
    </tag>
	......

标签调用代码如下：

	......
	<h2>下面显示的是自定义标签中的内容</h2>
	<mytag:fragment>
		<!-- 使用jsp:attribute标签传入fragment参数 -->
		<jsp:attribute name="fragment">
			<!-- 下面是动态的JSP页面片段 -->
			<mytag:helloWorld/>
		</jsp:attribute>
	</mytag:fragment>
	<br/>
	<mytag:fragment>
		<jsp:attribute name="fragment">
			<!-- 下面是动态的JSP页面片段 -->
			${pageContext.request.remoteAddr}
		</jsp:attribute>
	</mytag:fragment>
	......

###### 动态属性的标签

动态属性标签比普通标签多了如下两个要求

> 标签处理类还需要实DynamicAttributes接口，实现setDynamicAttribute方法。
>
> 配置标签时通过<dynamic-attribute../>子元素指定该标签支持动态属性。

代码如下

	public class DynaAttributesTag extends SimpleTagSupport implements DynamicAttributes
	{
		//保存每个属性名的集合
		private ArrayList<String> keys = new ArrayList<String>();
		//保存每个属性值的集合
		private ArrayList<Object> values = new ArrayList<Object>();
	
		@Override
		public void doTag() throws JspException, IOException
		{
			JspWriter out = getJspContext().getOut();
			//此处只是简单地输出每个属性
			out.println("<ol>");
			for( int i = 0; i < keys.size(); i++ )
			{
				String key = keys.get( i );
				Object value = values.get( i );
				out.println( "<li>" + key + " = " + value + "</li>" );
			}
			out.println("</ol>");
		}
		
		@Override
		public void setDynamicAttribute( String uri, String localName, 
			Object value ) 
			throws JspException
		{
			//添加属性名
			keys.add( localName );
			//添加属性值
			values.add( value );
		}
	}

tld文件如下：

	<!-- 定义接受动态属性的标签 -->
    <tag>
        <name>dynaAttr</name>
        <tag-class>lee.DynaAttributesTag</tag-class>
        <body-content>empty</body-content>
		<!-- 指定支持动态属性 -->
        <dynamic-attributes>true</dynamic-attributes>
    </tag>

调用方法如下：

	<h2>下面显示的是自定义标签中的内容</h2>
	<h4>指定两个属性</h4>
	<mytag:dynaAttr name="crazyit" url="crazyit.org"/><br/>
	<h4>指定四个属性</h4>
	<mytag:dynaAttr 书名="疯狂Java讲义" 价格="99.0" 出版时间="2008年" 描述="Java图书"/><br/>
   
<br/>

---

### JSP自定义标签二：

通过定义.tag文件定义标签

使用tag file无需定义标签处理类和tld文件，命名规则需要问tagName.tag,其中tagName文件名即为标签名，将赶文件放在WEB应用某个路径下，这个路径相当于标签库的URI名，可以统一放在/WEB-ING/tags目录下。

引入标签库的时候就可以按照 `<%@ taglib prefix="tags" tagdir="/WEB-ING/tags">`方式来引入标签库。

tag file具有如下5个编译指令

> taglib:用于导入其他标签库

> include:用于导入其他JSP或静态页面

> tag:作用类似于JSP文件中的page指令，有pageEncoding，body-content等属性，用于设置页面编码等属性

> attribute:用于设置自定义标签的属性，类似于自定义标签处理类中的标签属性。

> variavle:用于设置自定义标签的变量，这些变量将传给JSP页面使用。

示例代码如下：

文件名为iterator.tag

	<%@ tag pageEncoding="GBK" import="java.util.List"%>
	<!-- 定义了四个标签属性 -->
	<%@ attribute name="bgColor" %>
	<%@ attribute name="cellColor" %>
	<%@ attribute name="title" %>
	<%@ attribute name="bean" %>
	<table border="1" bgcolor="${bgColor}">
	<tr>
		<td><b>${title}</b></td>
	</tr>
	<%List<String> list = (List<String>)
		request.getAttribute("a");
	//遍历输出list集合的元素
	for (Object ele : list){%>
		<tr>
			<td bgcolor="${cellColor}">
			<%=ele%>
			</td>
		</tr>
	<%}%>
	</table>

调用代码如下：

	//使用自定义标签
	<tags:iterator bgColor="#99dd99" cellColor="#9999cc" title="迭代器标签" bean="a" />

<br/>

---

### EL表达式自定义函数：

该方法实际是EL表达式的自定义函数方法

SUN公司的`<%@ taglib uri="http://java.sun.com/jsp/jstl/functions" prefix="fn" %>`就是采用的这种方法，是用于在EL表达式中可以使用期定义的函数库。

引入方式示例：

	<%@ taglib prefix="fns" uri="/WEB-INF/tlds/fns.tld" %>

代码示例：

	<?xml version="1.0" encoding="UTF-8" ?>
	
	<taglib xmlns="http://java.sun.com/xml/ns/j2ee"
	  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	  xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-jsptaglibrary_2_0.xsd"
	  version="2.0">
	    
	  <description>JSTL 1.1 functions library</description>
	  <display-name>JSTL functions sys</display-name>
	  <tlib-version>1.1</tlib-version>
	  <short-name>fns</short-name>
	  <uri>http://java.sun.com/jsp/jstl/functionss</uri>
	
	  <!-- DictUtils -->
	  
	  <function>
	    <description>获取字典对象列表</description>
	    <name>getDictList</name>
	    <function-class>com.sdyy.base.sys.utils.DictUtils</function-class>
	    <function-signature>java.util.List getDictList(java.lang.String)</function-signature>
	    <example>${fns:getDictList(typeCode)}</example>  
	  </function>
	  
	  <function>
	    <description>获取字典对象列表</description>
	    <name>getDictListJson</name>
	    <function-class>com.sdyy.base.sys.utils.DictUtils</function-class>
	    <function-signature>java.lang.String getDictListJson(java.lang.String)</function-signature>
	    <example>${fns:getDictListJson(typeCode)}</example>  
	  </function>
	  
	  
	  <function>
	    <description>对象变json</description>
	    <name>toJSONString</name>
	    <function-class>com.alibaba.fastjson.JSON</function-class>
	    <function-signature>java.lang.String toJSONString(java.lang.Object)</function-signature>
	  </function>
	</taglib>

> function-class就是该方法的实体所在类路径，
> 
> function-signature就是该方法的方法名，值得一提的是，这个方法必须是个static方法。
> 
> example就是使用方法示例



