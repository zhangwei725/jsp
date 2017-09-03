# JSP

## 一、简介

> **JSP**（全称**J**ava**S**erver **P**ages）是由Sun公司倡导和许多公司参与共同创建的一种使软件开发者可以响应客户端请求，而动态生成HTML、XML或其他格式文档的Web网页的技术标准。JSP技术是以Java语言作为脚本语言的，JSP网页为整个服务器端的Java库单元提供了一个接口来服务于HTTP的应用程序。
>
> JSP使Java代码和特定的预定义动作可以嵌入到静态页面中。JSP句法增加了被称为JSP动作的XML标签，它们用来调用内建功能。另外，可以创建JSP标签库，然后像使用标准HTML或XML标签一样使用它们。标签库提供了一种和平台无关的扩展服务器性能的方法。
>
> JSP被JSP编译器编译成Java Servlets。一个JSP编译器可以把JSP编译成JAVA代码写的servlet然后再由JAVA编译器来编译成机器码，也可以直接编译成二进制码
>
> JSP中共包含了page指令，小脚本，表达式，申明，静态标签，注释六个部分。

​	

## 二、为什么使用JSP？

> JSP程序与CGI程序有着相似的功能，但和CGI程序相比，JSP程序有如下优势：
>
> - 性能更加优越，因为JSP可以直接在HTML网页中动态嵌入元素而不需要单独引用CGI文件。
> - 服务器调用的是已经编译好的JSP文件，而不像CGI/Perl那样必须先载入解释器和目标脚本。
> - JSP 基于Java Servlet API，因此，JSP拥有各种强大的企业级Java API，包括JDBC，JNDI，EJB，JAXP等等。
> - JSP页面可以与处理业务逻辑的 Servlet 一起使用，这种模式被Java servlet 模板引擎所支持。
>
> 最后，JSP是Java EE不可或缺的一部分，是一个完整的企业级应用平台。这意味着JSP可以用最简单的方式来实现最复杂的应用。

## 三、JSP的优势

> 以下列出了使用JSP带来的其他好处：
>
> - 与ASP相比：JSP有两大优势。首先，动态部分用Java编写，而不是VB或其他MS专用语言，所以更加强大与易用。第二点就是JSP易于移植到非MS平台上。
> - 与纯 Servlet 相比：JSP可以很方便的编写或者修改HTML网页而不用去面对大量的println语句。
> - 与SSI相比：SSI无法使用表单数据、无法进行数据库链接。
> - 与JavaScript相比：虽然JavaScript可以在客户端动态生成HTML，但是很难与服务器交互，因此不能提供复杂的服务，比如访问数据库和图像处理等等。
> - 与静态HTML相比：静态HTML不包含动态信息。

## 四、JSP 生命周期

### 1、编译阶段

servlet容器编译servlet源文件，生成servlet类

编译的过程包括三个步骤：

1. 解析JSP文件。
2. 将JSP文件转为servlet。
3. 编译servlet。

### 2、初始化阶段

加载与JSP对应的servlet类，创建其实例，并调用它的初始化方法

容器载入JSP文件后，它会在为请求提供任何服务前调用jspInit()方法。如果您需要执行自定义的JSP初始化任务，复写jspInit()方法就行了

> ```
> public void jspInit(){
>   // 初始化代码
> }
> ```

一般来讲程序只初始化一次，servlet也是如此。通常情况下您可以在jspInit()方法中初始化数据库连接、打开文件和创建查询表

### 3、执行阶段

> 调用与JSP对应的servlet实例的服务方法

### 4、销毁阶段

> 调用与JSP对应的servlet实例的销毁方法，然后销毁servlet实例

### 5、示例代码

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>

<%!
    private int initVar = 0;
    private int serviceCount = 0;
    private int destoryCount = 0;
%>

<%!
    public void jspInit() {
        initVar++;
        System.out.println(initVar);
    }

    public void jspDestroy() {
        destoryCount++;
        System.out.println("jspDestroy(): JSP被销毁了" + destoryCount + "次");
    }
%>
<%
    serviceCount++;
    String content1 = "初始化次数 : " + initVar;
    String content2 = "响应客户请求次数 : " + serviceCount;
    String content3 = "销毁次数 : " + destoryCount;
%>
<h1>js算个p 我们有jsp</h1>
<p><%=content1 %></p>
<p><%=content2 %></p>
<p><%=content3 %></p>

</body>
</html>
```

## 五、执行过程

![](http://opzv089nq.bkt.clouddn.com/17-9-3/28025680.jpg)







