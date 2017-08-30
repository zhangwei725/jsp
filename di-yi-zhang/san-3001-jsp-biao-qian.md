# JSP动作元素

## 一、简介

> 虽然我们希望JSP页面仅用作数据显示模块，不要嵌套任何java代码引入任何业务逻辑，但在实际开发中不引入一点业务逻辑是不可能的，但引入业务逻辑会导致页面出现难看java代码，怎么办？
>
> lSun公司允许用户开发自定义标签封装页面的java代码，以便jsp页面不出现一行java代码。当然Sun公司在jsp页面中也内置了一些标签(这些标签叫做jsp标签/动作)，开发人员使用这些标签可以完成页面的一些常用业务逻辑。
>
> JSP标签也称之为JspAction(JSP动作)元素，它用于在JSP页面中提供业务逻辑功能
>
> 语法格式 <jsp:action_name attribute="value" />

## 二、常用动作元素

### 2.1、 <jsp:include>

#### 1、作用

> <jsp:include>动作元素用来包含静态和动态的文件。该动作把指定文件插入正在生成的页面。

#### 2、语法格式

```
<jsp:include page="相对 URL 地址" flush="true" />
```

#### 3、属性

> | 属性    | 说明                    |
> | ----- | --------------------- |
> | page  | 包含在页面中的相对URL地址。       |
> | flush | 布尔属性，定义在包含资源前是否刷新缓存区。 |

#### 4、示例代码

1. title.jsp

   ```
   <%@ page language="java" contentType="text/html; charset=UTF-8"
       pageEncoding="UTF-8"%>
   <p>
     一颗赛艇,GOGOGO!!!
   </p>
   ```

2. home.jsp

   ```
   <%@ page language="java" contentType="text/html; charset=UTF-8"
       pageEncoding="UTF-8"%>
   <!DOCTYPE html>
   <html>
   <head>
   <meta charset="utf-8">
   <title>首页</title>
   </head>
   <body>
   <h2>澳门首家线上赌场上线啦</h2>
   <jsp:include page="title.jsp" flush="true" />
   </body>
   </html>
   ```

#### 5、<jsp:include>与include指令

1. <jsp:include>标签是动态引入， <jsp:include>标签涉及到的2个JSP页面会被翻译成2个servlet，这2个servlet的内容在执行时进行合并。 
2. 而include指令是静态引入，涉及到的2个JSP页面会被翻译成一个servlet，其内容是在源文件级别进行合并。
3. 不管是<jsp:include>标签，还是include指令，它们都会把两个JSP页面内容合并输出，所以这两个页面不要出现重复的HTML全局架构标签，否则输出给客户端的内容将会是一个格式混乱的HTML文档。

### 2.2、<jsp:forward>标签  

#### 1、作用

> 用于把请求转发给另外一个资源。

#### 2、语法

> <jsp:forward page="相对 URL 地址" />

#### 3、示例代码

1. title.jsp

   ```
   <%@ page language="java" contentType="text/html; charset=UTF-8"
       pageEncoding="UTF-8"%>
   <p>
     一颗赛艇,GOGOGO!!!
   </p>
   ```

2. home.jsp

   ```
   <%@ page language="java" contentType="text/html; charset=UTF-8"
       pageEncoding="UTF-8"%>
   <!DOCTYPE html>
   <html>
   <head>
   <meta charset="utf-8">
   <title>首页</title>
   </head>
   <body>
   <h2>澳门首家线上赌场上线啦</h2>
   <jsp:forward page="title.jsp" />
   </body>
   </html>
   ```

### 2.3、<jsp:param>标签 

#### 1、作用

> 当使用<jsp:include>和<jsp:forward>标签引入或将请求转发给其它资源时，可以使用<jsp:param>标签向这个资源传递参数。

#### 2、语法格式1：

```
  <jsp:include page="relativeURL |<%=expression%>">
  <jsp:paramname="parameterName" value="parameterValue|<%= expression%>" />
  </jsp:include>
```

#### 3、语法格式2

```
<jsp:forward page="relativeURL |<%=expression%>">
<jsp:paramname="parameterName" value="parameterValue" />
</jsp:include> 
```

#### 4、说明

```
<jsp:param>标签的name属性用于指定参数名，value属性用于指定参数值。在<jsp:include>和<jsp:forward>标签中可以使用多个<jsp:param>标签来传递多个参数
```

## 三、总结

> JSP还是Servlet，虽然都可以用于开发动态web资源。但由于这2门技术各自的特点，一般在开发中我们把servlet作为web应用中的控制器组件来使用，而把JSP技术作为数据显示模板来使用。
>
> 主要原因有以下几点
>
> 1. 让jsp既用java代码产生动态数据，又做美化会导致页面难以维护。
> 2. 让servlet既产生数据，又在里面嵌套html代码美化数据，同样也会导致程序可读性差，难以维护。
> 3. 因此最好的办法就是根据这两门技术的特点，让它们各自负责各的，servlet只负责响应请求产生数据，并把数据通过转发技术带给jsp，数据的显示jsp来做。

