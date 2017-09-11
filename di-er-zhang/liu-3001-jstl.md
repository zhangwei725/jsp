# JSTL

## 一、什么是JSTL

> JSTL全程（Java ServerPages Standard Tag Library) ，即JSP标准标签库，其主要功能时为JSP web开发人员提供一个通用的标签库，开发人员可以利用这些标签取代JSP页面上的java代码，从而提高代码的可读性和降低程序的维护难度！
>
> 它是apache对EL表达式的扩展，JSTL标签使用以来非常方便，它与JSP动作标签一样，只不过它不是JSP内置的标签，需要我们自己导包，以及指定标签库！
>
> **注意: JSTL依赖EL**

## 二、JSTL标签库分类

1. core：核心标签库(重点)
2. fmt：格式化标签库(掌握)
3. sql：数据库标签库(过时)
4. xml：xml标签库(过时)
5. JSTL 函数(了解)

## 三、JSTL版本要求

| **标准标签库**      | **JSTL版本** | **要求**                            | **下载地址**                                 |
| -------------- | ---------- | --------------------------------- | ---------------------------------------- |
| Standard 1.2.3 | JSTL 1.2   | Servlet 2.5, JavaServer Pages 2.1 | [download](http://tomcat.apache.org/download-taglibs.cgi) ([javadoc](http://tomcat.apache.org/taglibs/standard/apidocs/)) |
| Standard 1.1   | JSTL 1.1   | Servlet 2.4, JavaServer Pages 2.0 | [download](http://archive.apache.org/dist/jakarta/taglibs/standard/) |
| Standard 1.0   | JSTL 1.0   | Servlet 2.3, JavaServer Pages 1.2 |                                          |

## 四、JSTL常用API介绍

### 1、核心库介绍

1. 概要

   > JSTL的核心标签库标签共13个，使用这些标签能够完成JSP页面的基本功能

2. 分类

   - 表达式控制标签

     | 标签          | 说明                        |
     | ----------- | ------------------------- |
     | \<c:out>    | 用于在JSP中显示数据，就像<%= ... >   |
     | \<c:set>    | 用于保存数据                    |
     | \<c:remove> | 用于删除数据                    |
     | \<c:catch>  | 用来处理产生错误的异常状况，并且将错误信息储存起来 |

   - 流程控制标签

     | 标签             | 说明                                       |
     | -------------- | ---------------------------------------- |
     | \<c:if>        | 与我们在一般程序中用的if一样                          |
     | \<c:choose>    | 本身只当做\<c:when>和\<c:otherwise>的父标签        |
     | \<c:when>      | \<c:choose>的子标签，用来判断条件是否成立               |
     | \<c:otherwise> | \<c:choose>的子标签，接在\<c:when>标签后，当\<c:when>标签判断为false时被执行 |

   - 循环标签

     | 标签             | 说明                 |
     | -------------- | ------------------ |
     | \<c:forEach>   | 基础迭代标签，接受多种集合类型    |
     | \<c:forTokens> | 根据指定的分隔符来分隔内容并迭代输出 |

   - URL操作标签

     | 标签            | 说明                        |
     | ------------- | ------------------------- |
     | \<c:import>   | 检索一个绝对或相对 URL，然后将其内容暴露给页面 |
     | \<c:param>    | 用来给包含或重定向的页面传递参数          |
     | \<c:redirect> | 重定向至一个新的URL.              |
     | \<c:url>      | 使用可选的查询参数来创造一个URL         |

   ###  

### 2、格式化标签库

1. 概要

   > JSTL格式化标签用来格式化并输出文本、日期、时间、数字

2. 常用API

   | 标签              | 描述                |
   | --------------- | ----------------- |
   | \<formatNumber> | 使用指定的格式或精度格式化数字   |
   | \<formatDate>   | 用指定的风格或模式格式化日期和时间 |

## 五、使用详解

### 1、前期准备

1. 导包

   ```
      <dependency>
             <groupId>jstl</groupId>
             <artifactId>jstl</artifactId>
             <version>1.2</version>
       </dependency>
   注意: 
   1>JSTL 1.2之前需要导入 standard包
   2>JSTL 1.2之后只需要导一个包
   ```

2. 在使用标签的JSP页面中使用taglib指令导入标签库

   ```
   核心库
   <%@ taglib prefix="c"uri="http://java.sun.com/jstl/core" %> 
   格式化库
   <%@ taglib prefix="fmt" 
              uri="http://java.sun.com/jsp/jstl/fmt" %>
         
   1> prefix="c"：指定标签库的前缀，这个前缀可以随便给值，通常在实际开发中使用core标签库时指定前缀为c；
   2> uri="http://java.sun.com/jstl/core"：指定标签库的uri，它不一定是真实存在的网址，但它可以让JSP找到标签库的描述文件；
   ```

### 1、基本存储数据标签

#### 1.1、概要

> 标签用于把某一个对象存在指定的域范围内，或者将某一个对象存储到Map或者Bean对象中

#### 2、语法格式

1. 格式一 

   ```
   <c:set value=”值” var=”变量” [scope=”page|request|session|application”] />
   ```

2. 格式二 

   ```
   <c:set var=”变量” [scope=”page|request|session|application”]>
   　值
   </c:set>
   ```

3. 格式三 

   ```
   <c:set value=”值” target=”bean对象” property=”属性名”/>
   ```

4. 格式四 

   ```
   <c:set target=”bean对象” property=”属性名”>
   值
   </c:set>
   ```

   #### 3、属性

> | 属性名      | 是否支持EL | 属性类型   | 属性描述                                     |
> | -------- | ------ | ------ | ---------------------------------------- |
> | Value    | true   | Object | 用于指定属性值                                  |
> | var      | false  | String | 用于指定要设置的Web域属性的名称                        |
> | scope    | false  | String | 用于指定属性所在的Web域                            |
> | target   | true   | Object | 用于指定要设置属性的对象，这个对象必须是JavaBean对象或java.util.Map对象 |
> | property | true   | string | 用于指定当前要为对象设置的属性名称                        |

#### 4、示例代码

1. 往作用域中传值

   ```
   <%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
   <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>>
   <!DOCTYPE HTML>
   <html>
   <head>
   <title>set标签的使用</title>
   </head>
   <body>
   <h3>代码给出了给指定scope范围赋值的示例。</h3>
   <ul>
       <%--1. 通过<c:set>标签将data1的值放入page范围中。--%>
       <li>把一个值放入page域中:<c:set var="data" value=""data scope="page"/></li>
       <%--2.使用EL表达式从pageScope得到data1的值。--%>
       <li>从page域中得到值：${pageScope.data1}</li>
       <%--3.通过<c:set>标签将data2的值放入request范围中。--%>
       <li>把一个值放入request域中:<c:set var="data2" value="gacl" scope="request"/></li>
       <%--4.使用EL表达式从requestScope得到data2的值。--%>
       <li>从request域中得到值：${requestScope.data2}</li>
       <%--通过<c:set>标签将值name1的值放入session范围中。--%>
       <li>把一个值放入session域中。<c:set value="呵呵" var="name1" scope="session"></c:set></li>
       <%--使用EL表达式从sessionScope得到name1的值。--%>
       <li>从session域中得到值:${sessionScope.name1} </li>

       <%--把name2放入application范围中。 --%>
       <li>把一个值放入application域中。<c:set var="name2" scope="application">白虎神皇</c:set></li>
       <%--使用EL表达式从application范围中取值，用<c:out>标签输出使得页面规范化。 --%>
       <li>使用out标签和EL表达式嵌套从application域中得到值： 
            <c:out value="${applicationScope.name2}">未得到name的值</c:out>
       </li>
       <%--不指定范围使用EL自动查找得到值 --%> 
       <li>未指定scope的范围，会从不同的范围内查找得到相应的值：${data1}、${data2}、${name1}、${name2}</li>
   </ul>
   <hr/>
   ```

2. 往Bean中传递数据

   ```
   <h3>使用Java脚本实现以上功能</h3>
   <ul>
       <li>把一个值放入page域中。<%pageContext.setAttribute("data1","xdp");%></li>
       <li>从page域中得到值:<%out.println(pageContext.getAttribute("data1"));%></li>
       <li>把一个值放入request域中。<%request.setAttribute("data2","gacl");%></li>
       <li>从request域中得到值:<%out.println(request.getAttribute("data2"));%></li>

       <li>把一个值放入session域中。<%session.setAttribute("name1","");%></li>
       <li>从session中域得到值:<%out.println(session.getAttribute("name1"));%></li>
       <%--out.println()方法与<%=%>表达式输出功能一样 
       但使用表达式输出（<%=%>）明显要比使用out.println()输出更好。
       --%>
       <li><%=session.getAttribute("name1") %></li>
       <li>把另一个值放入application域中。<%application.setAttribute("name2","");%></li>
       <li> 从application域中得到值：<%out.println(application.getAttribute("name2"));%></li>
       <li><%=application.getAttribute("name2")%></li>

       <li>未指定scope的范围，会从不同的范围内查找得到相应的值：
           <%=pageContext.findAttribute("data1")%>、
           <%=pageContext.findAttribute("data2")%>、
           <%=pageContext.findAttribute("name1")%>、
           <%=pageContext.findAttribute("name2")%>
       </li>
   </ul>
   <hr/>
   <h3>操作JavaBean，设置JavaBean的属性值</h3>
   <%--设置JavaBean的属性值，等同与setter方法，Target指向实例化后的对象，property指向要插入值的参数名。
   注意：使用target时一定要指向实例化后的JavaBean对象，也就是要跟<jsp:useBean>配套使用，
   也可以java脚本实例化，但这就失去了是用标签的本质意义。
   使用Java脚本实例化：
   <%@page import="javabean.Person"%
   <% Person person=new Person(); %>
    --%>
   <c:set target="${person}" property="name">孤傲苍狼</c:set>
   <c:set target="${person}" property="age">25</c:set>
   <c:set target="${person}" property="sex">男</c:set>
   <c:set target="${person}" property="home">中国</c:set>
   <ul>
       <li>使用的目标对象为：${person}</li>
       <li>从Bean中获得的name值为：<c:out value="${person.name}"></c:out></li>
       <li>从Bean中获得的age值为：<c:out value="${person.age}"></c:out></li>
       <li>从Bean中获得的sex值为：<c:out value="${person.sex}"></c:out></li>
       <li>从Bean中获得的home值为：<c:out value="${person.home}"></c:out></li>
   </ul>
   <hr/>
   <h3>操作Map</h3>
    <% 
       Map map = new HashMap();
       request.setAttribute("map",map);
    %>
    <%--将data对象的值存储到map集合中 --%>
   <c:set property="data" value="gacl" target="${map}"/>
       ${map.data}
   </body>
   </html>
   ```



### 2、out标签的功能

#### 1、概要

> 主要是**用来输出数据对象（字符串、表达式）的内容或结果**。
>
> 在使用Java脚本输出时常使用的方式为： <% out.println(“字符串”)%> 或者 <%=表达式%> ，在web开发中，尽量必要使用java代码，使用\<c:out>标签就可以实现以上功能
>
> <c:out value=”EL表达式”>
>
> <c:out value=”字符串”>

#### 2、语法格式

1. 格式一

   ```
   <c:out value=”要显示的数据对象” [escapeXml=”true|false”] [default=”默认值”]/>
   ```

2. 格式二  

   ```
   <c:out value=”要显示的数据对象” [escapeXml=”true|false”]>默认值</c:out>
   ```

#### 3、示例代码

1. 基本使用

   ```
   <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
   <%@ page contentType="text/html;charset=UTF-8" pageEncoding="UTF-8" %>
   <%@page isELIgnored="false" %>

   <html>
   <head>
       <title>out标签使用</title>
   </head>
   <body>
   <c:out value="输出字符串"></c:out>
   <c:out value="${EL表达式}"></c:out>
   <c:out value="${null}" default="这个是显示默认值"/></li>
   <c:out value="${null}"/>
   </body>
   </html>
   ```

### 3、if标签

#### 1、作用

> 与程序中的if语句作用相同，用来实现条件控制。

#### 2、语法格式

1. 格式1:

   ```
   <c:if test="testCondition" var="varName" [scope="{page|request|session|application}"]/>
   ```

2. 格式2

   ```
   <c:if test="testCondition" [var="varName"] [scope="{page|request|session|application}"]>标签体内容。</c:if>
   ```

3. 参数说明

- test属性用于存放判断的条件，一般使用EL表达式来编写。
- var属性用来存放判断的结果，类型为true或false。
- scopes属性用来指定var属性存放的范围。

#### 3、属性

| 属性名   | 是否支持EL | 属性类型    | 属性描述                               |
| ----- | ------ | ------- | ---------------------------------- |
| test  | true   | boolean | 决定是否处理标签体中的内容的条件表达式                |
| var   | false  | String  | 用于指定将test属性的执行结果保存到某个Web域中的某个属性的名称 |
| scope | false  | String  | 指定将test属性的执行结果保存到哪个Web域中           |

#### 4、示例代码

```
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%--引入JSTL核心标签库 --%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<!DOCTYPE HTML>
<html>
<head>
    <title>JSTL: --流程控制标签 if标签示例</title>
</head>

<body>
    <h4>if标签示例</h4>
    <hr>
    <form action="JSTL_if_tag.jsp" method="post">
        <input type="text" name="uname" value="${param.uname}"> 
        <input type="submit" value="登录">
    </form>

    <%--使用if标签进行判断并把检验后的结果赋给admin，存储在默认的page范围中。 --%>
    <c:if test="${param.uname=='admin'}" var="name">
    <%--可以把name的属性范围设置为session，这样就可以在其他的页面中得到adminchock的值，
使用<c:if text=”${name}”><c:if>判断，实现不同的权限。 --%>
        <c:out value="管理员欢迎您！"/>
    </c:if>
    <%--使用EL表达式得到adminchock的值，如果输入的用户名为admin将显示true。 --%>
    ${adminchock}
</body>
</html> 
```

### 4、choose标签、when标签、otherwise标签

#### 1、概要

> `<c:choose>`、`<c:when>`和`<c:otherwise>`标签的功能
>
> `<c:choose>`、`<c:when>`和`<c:otherwise>`这3个标签通常情况下是一起使用的，`<c:choose>`标签作为`<c:when>`和`<c:otherwise>`标签的父标签来使用。
>
> 使用`<c:choose>`，`<c:when>`和`<c:otherwise>`三个标签，可以构造类似 “if-else if-else” 的复杂条件判断结构

#### 2、语法格式

1.  格式一

   ```
   <c:choose>
        <c:when test="条件1">
                //业务逻辑1
        </c:when>
        <c:when test="条件2">
                //业务逻辑2
        </c:when>
        <c:when test="条件n">
                //业务逻辑n
        </c:when>
        <c:otherwise>
                //业务逻辑
        </c:otherwise>
   </c:choose>
   ```

#### 3、示例代码

1. 示例代码

   ```
   <%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
   <%--引入JSTL核心标签库 --%>
   <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
   <!DOCTYPE HTML>
   <html>
   <head>
       <title>JSTL: -- choose及其嵌套标签标签示例</title>
   </head>

   <body>
       <h4>choose及其嵌套标签示例</h4>
       <hr/>
       <%--通过set标签设定score的值为85 --%>
       <c:set var="score" value="85"/>

       <c:choose>

       <%--使用<c:when>进行条件判断。
       如果大于等于90，输出“您的成绩为优秀”；
       如果大于等于70小于90，输出“您的成绩为良好”；
       大于等于60小于70，输出“您的成绩为及格”；
       其他（otherwise）输出“对不起，您没能通过考试”。
    --%>
           <c:when test="${score>=90}">
           你的成绩为优秀！
           </c:when>
           <c:when test="${score>70 && score<90}">
           您的成绩为良好!
           </c:when>

           <c:when test="${score>60 && score<70}">
           您的成绩为及格
           </c:when>

           <c:otherwise>
           对不起，您没有通过考试！

           </c:otherwise>
       </c:choose>
   </body>
   </html>
   ```

   ​



### 5、forEach标签

#### 1、概要

> 该标签根据循环条件遍历集合（Collection）中的元素

#### 2、语法格式

1. 格式1

   ```
   <c:forEach var=”name” items=”Collection” 
   　　varStatus=”StatusName” begin=”begin” 
   　　end=”end” step=”step”>
   </c:forEach>
   ```

#### 3、属性说明

| 属性        | 说明                    |
| --------- | --------------------- |
| var       | 设定变量名用于存储从集合中取出元素     |
| items     | 指定要遍历的集合              |
| varStatus | 设定变量名，该变量用于存放集合中元素的信息 |
| begin、end | 用于指定遍历的起始位置和终止位置（可选   |
| step      | 指定循环的步长               |

#### 3、示例代码

```
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%--引入JSTL核心标签库 --%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@page import="java.util.ArrayList"%>
<!DOCTYPE HTML>
<html>
<head>
    <title>JSTL: -- forEach标签实例</title>
</head>

<body>
<h4><c:out value="forEach实例"/></h4>
    <% 
       List<String>list = new ArrayList<String>(); 
       list.add(0, "萝拉"); 
       list.add(1, "小泽"); 
       list.add(2, "空空"); 
       list.add(3, "波多"); 
       list.add(4, "小明"); 
       request.setAttribute("list", list); 
    %>
    <c:out value="不指定begin和end的迭代：" />
    <%--不使用begin和end的迭代，从集合的第一个元素开始，遍历到最后一个元素。 --%>

    <c:forEach var="ite" items="${list}">
        &nbsp;<c:out value="${ite}"/><br/>
    </c:forEach>

    <B><c:out value="指定begin和end的迭代：" /></B><br>
    <%--指定begin的值为1、end的值为3、step的值为2，
    从第二个开始首先得到萝拉每两个遍历一次，
    则下一个显示的结果为小泽，end为3则遍历结束。 --%>
    <c:forEach var="ite" items="${list}" begin="1" end="3" step="2">
       <c:out value="${ite}"/>
   </c:forEach>

    <B><c:out value="输出整个迭代的信息：" /></B><br>
        <%--指定varStatus的属性名为s，并取出存储的状态信息 --%>
        <c:forEach var="ite" items="${list}" begin="3" end="4" varStatus="s" step="1">
             <c:out value="${ite}" />的四种属性：<br>

           所在位置，即索引：<c:out value="${s.index}" /><br>
           总共已迭代的次数：<c:out value="${s.count}" /><br>
           是否为第一个位置：<c:out value="${s.first}" /><br>
           是否为最后一个位置：<c:out value="${s.last}" /><br>  
        </c:forEach>
</body>
</html>
```

### 6、URL标签

#### 6.1、概要

>  标签用于在JSP页面中构造一个URL地址，其主要目的是实现URL重写

#### 6.2、语法格式

1. 格式一

   ```
   指定一个url不做修改，可以选择把该url存储在JSP不同的范围中
   <c:url value=”value” [var=”name”][scope=”page|request|session|application”] [context=”context”]/>`
   ```

2. 格式二

   ```
   配合 <c:param>标签给url加上指定参数及参数值，可以选择以name存储该url
   <c:url value=”value” [var=”name”] [scope=”page|request|session|application”] [context=”context”] 
       <c:param name=”参数名” value=”值”>
   </c:url>`
   ```

#### 6.3、属性

| 属性名   | 是否支持EL | 属性类型   | 属性描述                      |
| ----- | ------ | ------ | ------------------------- |
| value | true   | String | 指定要构造的URL                 |
| var   | false  | String | 指定将构造出的URL结果保存到Web域中的属性名称 |
| scope | false  | String | 指定将构造出的URL结果保存到哪个Web域中    |

#### 6.4、示例代码

```
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%--引入JSTL核心标签库 --%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>

<!DOCTYPE HTML>
<html>
<head>
    <title>JSTL: -- url标签实例</title>
</head>

<body>
    <c:out value="url标签使用"></c:out>
    <h4>使用url标签生成一个动态的url，并把值存入session中.</h4>
    <hr/>
    <c:url value="http://www.baidu.com" var="url" scope="session">
    </c:url>
    <a href="${url}">百度首页(不带参数)</a>
    <hr/>
    <h4>
    配合 &lt;c:param&gt;标签给url加上指定参数及参数值，生成一个动态的url然后存储到paramUrl变量中
    </h4>
    <c:url value="http://www.baidu.com" var="paramUrl">
        <c:param name="userName" value="codingxiaxw"/>
        <c:param name="pwd">123456</c:param>
    </c:url>
    <a href="${paramUrl}">百度首页(带参数)</a>
</body>
</html>
```

### 7、格式化标签库常用标签

1.   格式化时间

   ```
   <%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>  
   ......  
   <%  
       Date date = new Date();  
       pageContext.setAttribute("d", date);  
   %>  
   <fmt:formatDate value="${d }" pattern="yyyy-MM-dd HH:mm:ss"/> 
   ```

2. 格式化

   ```
   <%  
       double d1 = 3.5;  
       double d2 = 4.4;   
       pageContext.setAttribute("d1", d1);  
       pageContext.setAttribute("d2", d2);  
   %>  
   <fmt:formatNumber value="${d1 }" pattern="0.00"/><br/>  
   <fmt:formatNumber value="${d2 }" pattern="#.##"/>  
   ```

   ​