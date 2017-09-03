# JSP中9大隐式对象

## 一、简介

> 每个JSP页面在第一次被访问时，WEB容器都会把请求交给JSP引擎（即一个Java程序）去处理。JSP引擎先将JSP翻译成一个\_jspServlet\(实质上也是一个servlet\)，然后按照servlet的调用方式进行调用。
>
> 由于JSP第一次访问时会翻译成servlet，所以第一次访问通常会比较慢，但第二次访问，JSP引擎如果发现JSP没有变化，就不再翻译，而是直接调用，所以程序的执行效率不会受到影响。
>
> JSP引擎在调用JSP对应的\_jspService时，会传递或创建9个与web开发相关的对象供\_jspService使用。JSP技术的设计者为便于开发人员在编写JSP页面时获得这些web对象的引用，特意定义了9个相应的变量，开发人员在JSP页面中通过这些变量就可以快速获得这9大对象的引用。内置对象的特点是
>
> 1. 由JSP规范提供,不用编写者实例化。
> 2. 通过Web容器实现和管理
> 3. 所有JSP页面均可使用
> 4. 只有在脚本元素的表达式或代码段中才可使用\(&lt;%=使用内置对象%&gt;或&lt;%使用内置对象%&gt;\)

## 二、分类

1. 结构图

   ![](http://opzv089nq.bkt.clouddn.com/17-8-31/30001260.jpg)

   | 隐含对象 | 所属的类 | 说明 |
   | :--- | --- | --- |
   | request | javax.servlet.http.HttpServletRequest | 客户端的请求信息 |
   | response | javax.servlet.http.HttpServletResponse | 网页传回客户端的响应 |
   | session | javax.servlet.http.HttpSession | 与请求有关的会话 |
   | out | javax.servlet.jsp.JSPWriter | 向客户端浏览器输出数据的数据流 |
   | application | javax.servlet.ServletContext | 提供全局的数据，一旦创建就保持到服务器关闭 |
   | pageContext | javax.servlet.jsp.PageContext | JSP页面的上下文，用于访问页面属性 |
   | page | java.lang.Object | 同Java中的this，即JSP页面本身 |
   | config | javax.servlet.servletConfig | Servlet的配置对象 |

## 三、详解

### 1、request 对象\(重点\)

#### 1.1、概要

> request 对象包含所有请求的信息，如：请求的来源、标头、cookies和请求相关的参数值等等。
>
> request 对象实现javax.servlet.http.HttpServletRequest接口的，所提供的方法可以将它分为四大类：

#### 1.2、API介绍

1. 储存和取得属性方法;

   > void setAttribute\(String name, Object value\)设定name属性的值为value
   >
   > Enumeration getAttributeNamesInScope\(int scope\)取得所有scope 范围的属性
   >
   > Object getAttribute\(String name\)取得name 属性的值
   >
   > void removeAttribute\(String name\)移除name 属性的值

2. 取得请求参数的方法

   > String getParameter\(String name\) 取得name 的参数值Enumeration
   >
   > getParameterNames\( \) 取得所有的参数名称String\[\]
   >
   > getParameterValues\(String name\) 取得所有name 的参数值
   >
   > Map getParameterMap\( \)取得一个要求参数的Map

3. 能够取得请求HTTP 标头的方法

   > String getHeader\(String name\)取得name 的标头
   >
   > Enumeration getHeaderNames\(\)取得所有的标头名称
   >
   > Enumeration getHeaders\(String name\)取得所有name 的标头
   >
   > int getIntHeader\(String name\)取得整数类型name 的标头
   >
   > long getDateHeader\(String name\) 取得日期类型name 的标头
   >
   > Cookie \[\] getCookies\( \) 取得与请求有关的cookies

4. 其他的方法

   > String getContextPath\( \)取得Context 路径\(即站台名称\)
   >
   > String getMethod\( \)取得HTTP 的方法\(GET、POST\)
   >
   > String getProtocol\( \)取得使用的协议 HTTP/1.1、HTTP/1.0 \)
   >
   > String getQueryString\( \)取得请求的参数字符串，不过，HTTP的方法必须为GET
   >
   > String getRequestedSessionId\( \) 取得用户端的Session ID
   >
   > String getRequestURI\( \)取得请求的URL，但是不包括请求的参数字符串
   >
   > String getRemoteAddr\( \)取得用户的IP 地址
   >
   > String getRemoteHost\( \)取得用户的主机名称
   >
   > int getRemotePort\( \)取得用户的主机端口
   >
   > String getRemoteUser\( \) 取得用户的名称
   >
   > void setCharacterEncoding\(String encoding\)设定编码格式，用来解决窗体传递中文的问题

#### 1.3、示例代码

1. 获取客服端参数

   ```jsp
   <%-- reqeust1.jsp核心代码 --%>
   <form action="request2.jsp" method="post">
       姓名:<input name="username"><br>
       密码:<input type="password" name="password"><br>
       <input type="submit" />
   </form>

   <%-- reqeust2.jsp核心代码 --%>
   <%@ page contentType="text/html;charset=UTF-8" pageEncoding="utf-8" %>
   <%
       String username = request.getParameter("username");
       String password = request.getParameter("password");
   %>
   <html>

   <head>
       <title>Title</title>
   </head>
   <body>

   <p>
       <%=username %>
   </p>
   <p>
       <%=password %>
   </p>

   </body>
   </html>
   ```

2. 其它方法

   ```
   <%-- reqeust2.jsp核心代码 --%>
   <%@ page contentType="text/html;charset=UTF-8" pageEncoding="utf-8" %>
   <%
       String contextPath = request.getContextPath();
   %>
   <html>

   <head>
       <title>Title</title>
   </head>
   <body>

   <p>
       根目录:<%=contextPath %>
   </p>
   <p>
       请求路径:<%=request.getRequestURL() %>
   </p>

   <% if (cookies != null && cookies.length > 0) {
       for (Cookie cookie : cookies) { %>
   <span><%=cookie.getName()%></span>:<span><%=cookie.getValue()%></span><br>
   <%
       }
     }
   %>
   <pre>
   返回HTTP 请求信息中使用的方法名称:<%=request.getMethod()%><br>

   返回请求信息中调用Servlet 的URL 部分:<%=request.getServletPath()%><br>

   返回HTTP GET 请求信息中URL 之后的查询字符串:<%=request.getQueryString()%><br>

   返回请求实体的MIME类型:<%=request.getContentType()%><br>

   返回请求信息中的协议名名字和版本号:<%=request.getProtocol()%><br>

   有关任何路径信息:<%=request.getPathInfo()%><br>

   返回接受请求的服务器主机:<%=request.getServerName()%><br>

   返回服务器的端口号:<%=request.getServerPort()%><br>

   返回提交请求的客户机的规范名字:<%=request.getRemoteHost()%><br>

   返回提交请求的客户机的IP地址:<%=request.getRemoteAddr()%><br>
   返回请求中使用的模式（协议）名字:<%=request.getScheme()%><br>
   </pre>
   </body>
   </html>
   ```

### 2、response 对象

#### 2.1、概要

> response 对象主要将JSP对象 处理数据后的结果传回到客户端。  
> ​response 对象是实现javax.servlet.http.HttpServletResponse 接口。response对象所提供的方法。

#### 2.2、API介绍

1. 设定表头的方法

   > void addCookie\(Cookie cookie\)新增cookie
   >
   > void addDateHeader\(String name, long date\)新增long类型的值到name标头
   >
   > void addHeader\(String name, String value\)新增String类型的值到name标头
   >
   > void addIntHeader\(String name, int value\)新增int类型的值到name标头
   >
   > void setDateHeader\(String name, long date\)指定long类型的值到name标头
   >
   > void setHeader\(String name, String value\)指定String类型的值到name标头
   >
   > void setIntHeader\(String name, int value\)指定int类型的值到name标头

2. 设定响应状态码的方法

   > void sendError\(int sc\)传送状态码\(status code\)
   >
   > void sendError\(int sc, String msg\)传送状态码和错误信息
   >
   > void setStatus\(int sc\)设定状态码

3. 用来URL 重写\(rewriting\)的方法

   > String encodeRedirectURL\(String url\)对使用sendRedirect\( \)方法的URL予以编码

#### 2.3、示例代码

1. 重定向

   ```
   response1.jsp

   <%@ page contentType="text/html;charset=UTF-8" pageEncoding="UTF-8" %>
   <html>
   <head>
       <meta charset="UTF-8">
       <title>response对象</title>
   </head>
   <body>
   <form action="response2.jsp">
       <select name="city">
           <option value="1">北京</option>
           <option value="2">上海</option>
       </select>
       <input type="submit">
   </form>
   </body>
   </html>

   response2.jsp
   <%@ page contentType="text/html;charset=UTF-8" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
   <%
       String city = request.getParameter("city");
       if ("1".equals(city)) {
           response.sendRedirect("shanghai.jsp");
       } else if ("2".equals(city)) {
           response.sendRedirect("beijing.jsp");
       } else {
           out.println("没有进行页面重定向");
       }
   %>
   </body>
   </html>
   ```

### 3、page 对象

​    page对象有点类似于Java编程中的this指针，就是指当前JSP页面本身。page是java.lang.Object类的对象。

### 4、config 对象

```
config 对象里存放着一些Servlet 初始的数据结构。

config 对象实现于javax.servlet.ServletConfig 接口，它共有下列四种方法：

public String getInitParameter(name)

public java.util.Enumeration getInitParameterNames( )

public ServletContext getServletContext( )

public Sring getServletName( )
```

### 5、out 对象

```
out 对象能把结果输出到网页上。

out主要是用来控制管理输出的缓冲区(buffer)和输出流(output stream)。

void clear( )清除输出缓冲区的内容

void clearBuffer( )清除输出缓冲区的内容

void close( )关闭输出流，清除所有的内容

int getBufferSize( )取得目前缓冲区的大小(KB)

int getRemaining( )取得目前使用后还剩下的缓冲区大小(KB)

boolean isAutoFlush( )回传true表示缓冲区满时会自动清除;false表示不会自动清除并且产生异常处理
```

### 6、session 对象\(重点\)

> session对象表示目前个别用户的会话\(session\)状况。
>
> session对象实现javax.servlet.http.HttpSession接口，HttpSession接口所提供的方法
>
> long getCreationTime\(\)取得session产生的时间，单位是毫秒
>
> String getId\(\)取得session 的ID
>
> long getLastAccessedTime\(\)取得用户最后通过这个session送出请求的时间
>
> long getMaxInactiveInterval\(\)取得最大session不活动的时间，若超过这时间，session 将会失效
>
> void invalidate\(\)取消session 对象，并将对象存放的内容完全抛弃
>
> boolean isNew\(\)判断session 是否为"新"的
>
> void setMaxInactiveInterval\(int interval\)设定最大session不活动的时间，若超过这时间，session 将会失效

### 7、application对象\(重点\)

#### 7.1 概要

> application对象最常被使用在存取环境的信息。  
> 因为环境的信息通常都储存在ServletContext中，
>
> 所以常利用application对象来存取ServletContext中的信息。

#### 7.2 API介绍

> application 对象实现javax.servlet.ServletContext 接口，ServletContext接口容器所提供的方法
>
> int getMajorVersion\( \)取得Container主要的Servlet API版本
>
> int getMinorVersion\( \)取得Container次要的Servlet API 版本
>
> String getServerInfo\( \)取得Container的名称和版本
>
> String getMimeType\(String file\)取得指定文件的MIME 类型
>
> ServletContext getContext\(String uripath\)取得指定Local URL的Application context
>
> String getRealPath\(String path\)取得本地端path的绝对路径
>
> void log\(String message\)将信息写入log文件中
>
> void log\(String message, Throwable throwable\)将stack trace 所产生的异常信息写入log文件中

#### 7.3  说明

1. 在整个Web应用的多个JSP、Servlet之间共享数据

2. 访问Web应用的配置参数

   ​

### 8、pageContext对象

#### 8.1、概要

​    pageContext对象能够存取其他隐含对象。

#### 8.2、API介绍

1. pageContext对象存取其他隐含对象属性的方法，此时需要指定范围的参数

   > Object getAttribute\(String name, int scope\)
   >
   > Enumeration getAttributeNamesInScope\(int scope\)
   >
   > void removeAttribute\(String name, int scope\)
   >
   > void setAttribute\(String name, Object value, int scope\)
   >
   > 范围参数有四个，分别代表四种范围：PAGE\_SCOPE、REQUEST\_SCOPE、SESSION\_SCOPE、APPLICATION\_SCOPE

2. pageContext对象取得其他隐含对象的方法

   > Exception getException\( \)回传目前网页的异常，不过此网页要为error page，
   >
   > JspWriter getOut\( \)回传目前网页的输出流，例如：out
   >
   > Object getPage\( \)回传目前网页的Servlet 实体\(instance\)，例如：page
   >
   > ServletRequest getRequest\( \)回传目前网页的请求，例如：request
   >
   > ServletResponse getResponse\( \)回传目前网页的响应，例如：response
   >
   > ServletConfig getServletConfig\( \)回传目前此网页的ServletConfig 对象，例如：config
   >
   > ServletContext getServletContext\( \) 回传目前此网页的执行环境\(context\)，例如：application
   >
   > HttpSession getSession\( \)回传和目前网页有联系的会话\(session\)，例如：session

3. PageContext对象提供取得属性的方法

   > Object getAttribute\(String name, int scope\)回传name 属性，范围为scope的属性对象，回传类型为Object
   >
   > Enumeration getAttributeNamesInScope\(int scope\)回传所有属性范围为scope 的属性名称，回传类型为Enumeration
   >
   > int getAttributesScope\(String name\)回传属性名称为name 的属性范围
   >
   > void removeAttribute\(String name\)移除属性名称为name 的属性对象
   >
   > void removeAttribute\(String name, int scope\)移除属性名称为name，范围为scope 的属性对象
   >
   > void setAttribute\(String name, Object value, int scope\)指定属性对象的名称为name、值为value、范围为scope
   >
   > Object findAttribute\(String name\)寻找在所有范围中属性名称为name 的属性对象

### 9、exception对象

#### 9.1、概要

> 被调用的错误页面的结果,只有在错误页面中才可使用,
>
> 即在页面指令中设置:&lt;%@page isErrorPage=“true”%&gt;

#### 9.2、API介绍

> getMessage\( \)
>
> getLocalizedMessage\( \)、
>
> printStackTrace\(new java.io.PrintWriter\(out\)\)



