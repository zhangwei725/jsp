# EL表达式

## 一、简介

> EL（Expression Language） 是为了使JSP写起来更加简单。表达式语言的灵感来自于 ECMAScript 和 XPath 表达式语言，它提供了在 JSP 中简化表达式的方法，让Jsp的代码更加简化。
>
> EL表达式是JSP 2.0规范中的一门技术 。因此，若想正确解析EL表达式，需使用支持Servlet2.4/JSP2.0技术的WEB服务器。
>
> 1. 升级成tomcat6
> 2. 在JSP中加入&lt;%@ page isELIgnored="false" %&gt;

## 二、作用

### 1、获取数据

> EL表达式主要用于替换JSP页面中的脚本表达式，以从各种类型的web域 中检索java对象、获取数据。\(某个web域 中的对象，访问javabean的属性、访问list集合、访问map集合、访问数组\)

### 2、执行运算

> 利用EL表达式可以在JSP页面中执行一些基本的关系运算、逻辑运算和算术运算，以在JSP页面中完成一些简单的逻辑运算。

### 3、获取web开发常用对象

> EL 表达式定义了一些隐式对象，利用这些隐式对象，web开发人员可以很轻松获得对web常用对象的引用，从而获得这些对象中的数据。

### 4、调用Java方法

> EL表达式允许用户开发自定义EL函数，以在JSP页面中通过EL表达式调用Java类的方法。

## 三、基础语法

### 1、语法结构

> ${expression}

### 2、\[ \]与.运算符

1. 概要

   > EL 提供“.“和“\[ \]“两种运算符来存取数据。
   >
   > 1. 当要存取的属性名称中包含一些特殊字符，如 . 或 ? 等并非字母或数字的符号，就一定要使用“\[ \]“。
   > 2. 如果要动态取值时，就可以用“\[ \]“来做，而“.“无法做到动态取值。

2. 示例代码

   ```
   <%@ page contentType="text/html;charset=UTF-8" isELIgnored="false" %>
   <html>
   <head>
       <title>EL-基础语法</title>
   </head>
   <body>
       ${user.user-name}应当改为${user["user-name"]}
       ${user.user-name}应当改为${user["user-name"]}
   </body>
   </html>
   ```

### 3、变量

1. 说明

   > EL存取变量数据的方法很简单，例如：${username}。它的意思是取出某一范围中名称为username的变量
   >
   > 如果没有指定哪一个范围的username，所以它会依序从Page、Request、Session、Application范围查找。  
   > 假如途中找到username，就直接回传，不再继续找下去，但是假如全部的范围都没有找到时，就回传空字符串

2. 示例代码

   1. 示例1

      ```
      <%@ page contentType="text/html;charset=UTF-8" isELIgnored="false" %>
      <html>
      <head>
          <title>EL-基础语法</title>
      </head>
      <%
          pageContext.setAttribute("username", "萝拉1");
          request.setAttribute("username", "萝拉2");
          session.setAttribute("username", "萝拉3");
          application.setAttribute("username", "萝拉4");
      %>

      <h1>${username}</h1>
      </body>
      </html>
      ```

   2. 示例2

      ```
      <%
          User user = new User();
          user.setUid(1);
          user.setName("老李");
          user.setAge(18);
          pageContext.setAttribute("user", user);

      %>
      <table>
          <tr>
              <th>id</th>
              <th>用户名</th>
              <th>年龄</th>
          </tr>
          <tr>
              <td>${user.uid}</td>
              <td>${user.name}</td>
              <td>${user.age}</td>
          </tr>
      </table>
      ```

   3. 示例代码

      ```
      <%@ page import="java.util.*" pageEncoding="UTF-8" %>
      <%@ page import="com.werner.web.bean.User" %>
      <%@ page import="com.werner.web.bean.Address" %>

      <!DOCTYPE HTML>
      <html>
      <head>
          <title>el表达式获取数据</title>
      </head>
      <body>
      <%
          request.setAttribute("name", "隔壁小张");
      %>
      <hr>
      <!-- 在jsp页面中，使用el表达式可以获取bean的属性 -->
      <%
          User user = new User();
          user.setAge(12);
          request.setAttribute("user", user);
      %>
      使用el表达式可以获取bean的属性：${user.age}
      <hr>
      <!-- 在jsp页面中，使用el表达式可以获取bean中的。。。。。。。。。的属性 -->
      <%
          User user1 = new User();
          user.setName("隔壁小张");
          Address address = new Address();
          address.setCity("武汉");
          user1.setAddress(address);
          request.setAttribute("user1", user1);
      %>
      ${user1.address.city}
      <hr>
      <!-- 在jsp页面中，使用el表达式获取list集合中指定位置的数据 -->
      <%
          User u1 = new User();
          u1.setName("空空");
          User u2 = new User();
          u2.setName("萝拉");

          List<User> list = new ArrayList<>();
          list.add(u1);
          list.add(u2);
          request.setAttribute("list", list);
      %>

      <!-- 取list指定位置的数据 -->
      ${list[1].name}
      <!-- 迭代List集合 -->

      <hr>
      <!-- 在jsp页面中，使用el表达式获取map集合的数据 -->
      <%
          Map<String, String> map = new LinkedHashMap<>();
          for (int i = 0; i < 10; i++) {
              map.put("key" + i, "value" + i);
          }
          request.setAttribute("map", map);
      %>

      <!-- 根据关键字取map集合的数据 -->
      ${map.key1}
      ${map["key1"]}
      <hr>
      <hr>
      </body>
      </html>
      ```

      ​

### 4、文字常量

1. 说明

   > 一个EL表达式包含变量、文字常量、操作符。文字常量主要包括字符串、数字和布尔值、还有NULL

   | 文字 | 文字的值 |
   | --- | --- |
   | Boolean | true 和 false |
   | Integer | 与 Java 类似。可以包含任何正数或负数，例如 100、-200、10000 |
   | Floating Point | 与 Java 类似。可以包含任何正的或负的浮点数，例如 -1.6E-45、3.1415 |
   | String | 任何由单引号或双引号限定的字符串。对于单引号、双引号和反斜杠，使用反斜杠字符作为转义序列。必须注意，如果在字符串两端使用双引号，则单引号不需要转义。 |
   | Null | null |

2. 示例代码

   ```
   <body>
     ${true} 
     ${10}
     ${3.1415f} 
     ${“xiaoming”} 
     ${null}
   <body>
   ```

### 5、操作符

1. 操作符列表

   | 操作符 | 功能和作用 |
   | --- | --- |
   | . | 访问一个 bean 属性或者 Map entry |
   | \[\] | 访问一个数组或者链表元素 |
   | \(\) | 对子表达式分组，用来改变赋值顺序 |
   | ? : | 条件语句，比如：条件 ?ifTrue:ifFalse如果条件为真，表达式值为前者，反之为后者 |
   | + | 数学运算符，加操作 |
   | - | 数学运算符，减操作或者对一个值取反 |
   | \* | 数学运算符，乘操作 |
   | / 或 div | 数学运算符，除操作 |
   | % 或 mod | 数学运算符，模操作 \( 取余 \) |
   | == 或 eq | 逻辑运算符，判断符号左右两端是否相等，如果相等返回 true ，否则返回 false |
   | != 或 ne | 逻辑运算符，判断符号左右两端是否不相等，如果不相等返回 true ，否则返回 false |
   | &lt; 或 lt | 逻辑运算符，判断符号左边是否小于右边，如果小于返回 true ，否则返回 false |
   | &gt; 或 gt | 逻辑运算符，判断符号左边是否大于右边，如果大于返回 true ，否则返回 false |
   | &lt;= 或 le | 逻辑运算符，判断符号左边是否[小于]()或者等于右边，如果小于或者等于返回 true ，否则返回 false |
   | &gt;= 或 ge | 逻辑运算符，判断[符号]()左边是否大于或者等于右边，如果大于或者等于返回 true ，否则返回 false |
   | && 或 and | 逻辑运算符，与操作赋。如果左右两边同为 true 返回 true ，否则返回 false |
   | \|\| 或 or | 逻辑运算符，或操作赋。如果左右两边有任何一边为 true 返回 true ，否则返回 false |
   | ! 或 not | 逻辑运算符，非操作赋。如果对 true 取运算返回 false ，否则返回 true |
   | empty | 用来对一个空变量值进行判断 : null 、一个空 String 、空数组、 空 Map 、没有条目的 Collection 集合 |

2. 算术运算符

   > ${6+6} 。
   >
   > 注意：在EL表达式中的‘+’只有数学运算的功能，没有连接符的功能，它会试着把运算符两边的操作数转换为数值类型，进而进行数学加法运算，最后把结果输出。若出现${'a'+'b'}则会出现异常。
   >
   > ${10-8}
   >
   > ${3\*3}
   >
   > ${12/4}

3. 关系运算符

   > &gt; 或者 gt， 例如：${110&gt;9}  或者 ${110 gt 9 }
   >
   > &gt;= 或者 ge， 例如：${45&gt;=9} 或者 ${45 ge 9 }
   >
   > &lt; 或者 lt， 例如：${4&lt;9} 或者 ${4 lt 9 }
   >
   > &lt;= 或者 le， 例如：${9&lt;=8} 或者 ${9 le 8 }
   >
   > == 或者 eq， 例如：${5=4} 或者 ${4 eq 4 }
   >
   > != 或者 ne， 例如：${10!=3} 或者 ${10 ne 3 }

4. 逻辑运算符

   > && 或者 and， 例如：${false && false} 或者 ${false and false }
   >
   > \|\| 或者 or， 例如：${true \|\| false} 或者 ${true or false }
   >
   > ! 或者 not，例如：${!true}（相当于${false}） 或者 ${not true }

5. 三元运算符

   > ? :     例如：${ 3&gt;2 ?  '是' : '不是' }

6. empty

   ```
   <body>
     username:${param.username }
     empty处理结果：${empty param.username }
     ==null处理结果：${param.username == null }
   </body>

   http://127.0.0.1:8080/jsp/el02.jsp
   http://127.0.0.1:8080/jsp/el02.jsp?username=
   在EL中empty对""和null的处理都返回true，而==null对""返回false，对null返回true
   ```

## 四、隐含对象

1. JSP有9个隐含对象，而EL也有自己的隐含对象。EL隐含对象总共有11 个

   | 隐含对象 | 类型 | 说明 |
   | --- | --- | --- |
   | PageContext | javax.servlet.ServletContext | 表示此JSP的PageContext |
   | PageScope | java.util.Map | 取得Page范围的属性名称所对应的值 |
   | RequestScope | java.util.Map | 取得Request范围的属性名称所对应的值 |
   | sessionScope | java.util.Map | 取得Session范围的属性名称所对应的值 |
   | applicationScope | java.util.Map | 取得Application范围的属性名称所对应的值 |
   | param | java.util.Map | 如同ServletRequest.getParameter\(String name\)。回传String类型的值 |
   | paramValues | java.util.Map | 如同ServletRequest.getParameterValues\(String name\)。回传String\[\]类型的值 |
   | header | java.util.Map | 如同ServletRequest.getHeader\(String name\)。回传String类型的值 |
   | headerValues | java.util.Map | 如同ServletRequest.getHeaders\(String name\)。回传String\[\]类型的值 |
   | cookie | java.util.Map | 如同HttpServletRequest.getCookies\(\) |
   | initParam | java.util.Map | 如同ServletContext.getInitParameter\(String name\)。回传String类型的值 |

   > **注意: 字符串要加双引号，不然的话EL会默认把你认为的常量当做一个变量来处理，这时如果这个变量在4个声明范围不存在的话会输出空，如果存在则输出该变量的值。**

2. 示例代码

   ```
   <%@ page import="java.util.*" pageEncoding="UTF-8" %>
      <!DOCTYPE HTML>
      <html>
      <head>
          <title>el隐式对象</title>
      </head>
      <body>
      <br/>---------------1、pageContext对象：获取JSP页面中的pageContext对象------------------------<br/>
      ${pageContext}
      <br/>---------------2、pageScope对象：从page域(pageScope)中查找数据------------------------<br/>
      <%
          pageContext.setAttribute("name", "小泽");  //map
      %>
      ${pageScope.name}
      <br/>---------------3、requestScope对象：从request域(requestScope)中获取数据------------------------<br/>
      <%
          request.setAttribute("name", "萝拉");  //map
      %>
      ${requestScope.name}
      <br/>---------------4、sessionScope对象：从session域(sessionScope)中获取数据------------------------<br/>
      <%
          session.setAttribute("user", "xiaoming");  //map
      %>
      ${sessionScope.user}
      <br/>---------------5、applicationScope对象：从application域(applicationScope)中获取数据------------------------<br/>
      <%
          application.setAttribute("user", "xiaohong");  //map
      %>
      ${applicationScope.user}
      <br/>--------------6、param对象：获得用于保存请求参数map，并从map中获取数据------------------------<br/>
      <!-- http://localhost:8080/JavaWeb_EL_Study_20140826/ELDemo03.jsp?name=aaa  -->
      ${param.name}
      <!-- 此表达式会经常用在数据回显上 -->
      <form action="${pageContext.request.contextPath}/servlet/RegisterServlet" method="post">
          <input type="text" name="username" value="${param.username}">
          <input type="submit" value="注册">
      </form>
      <br/>--------------7、paramValues对象：paramValues获得请求参数 //map{"",String[]}------------------------<br/>
      <!-- http://localhost:8080/JavaWeb_EL_Study_20140826/ELDemo03.jsp?like=aaa&like=bbb -->
      ${paramValues.like[0]}
      ${paramValues.like[1]}
      <br/>--------------8、header对象：header获得请求头------------------------<br/>
      ${header.Accept}<br/>
      <%--${header.Accept-Encoding} 这样写会报错
           测试headerValues时，如果头里面有“-” ，例Accept-Encoding，则要headerValues[“Accept-Encoding”]
      --%>
      ${header["Accept-Encoding"]}
      <br/>--------------9、headerValues对象：headerValues获得请求头的值------------------------<br/>
      <%--headerValues表示一个保存了所有http请求头字段的Map对象，它对于某个请求参数，返回的是一个string[]数组
      例如：headerValues.Accept返回的是一个string[]数组 ，headerValues.Accept[0]取出数组中的第一个值
      --%>
      ${headerValues.Accept[0]}<br/>
      <%--${headerValues.Accept-Encoding} 这样写会报错
           测试headerValues时，如果头里面有“-” ，例Accept-Encoding，则要headerValues[“Accept-Encoding”]
           headerValues["Accept-Encoding"]返回的是一个string[]数组，headerValues["Accept-Encoding"][0]取出数组中的第一个值
      --%>
      ${headerValues["Accept-Encoding"][0]}
      <br/>--------------10、cookie对象：cookie对象获取客户机提交的cookie------------------------<br/>
      <!-- 从cookie隐式对象中根据名称获取到的是cookie对象,要想获取值，还需要.value -->
      ${cookie.JSESSIONID.value} //保存所有cookie的map
      <br/>--------------11、initParam对象：initParam对象获取在web.xml文件中配置的初始化参数------------------------<br/>
      <%--
       <!-- web.xml文件中配置初始化参数 -->
      <context-param>
         <param-name>chaset</param-name>
         <param-value>UTF-8</param-value>
      </context-param>
      <context-param>
         <param-name>root</param-name>
         <param-value>/root</param-value>
      </context-param>
       --%>
      <%--获取servletContext中用于保存初始化参数的map --%>
      ${initParam.name}<br/>
      ${initParam.root}
      </body>
      </html>
   ```

   ​



