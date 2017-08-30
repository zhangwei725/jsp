## JSP指令

## 一、概要

> JSP指令（directive）是为JSP引擎而设计的，它们并不直接产生任何可见输出，而只是告诉引擎如何处理JSP页面中的其余部分。
>
> JSP指令的基本语法格式：<%@ 指令 属性名="值" %> 
>
> 在JSP2.0规范中共定义了三个指令：page指令  Include指令 taglib指令

## 二、page指令

### 1、定义

> page指令用于定义JSP页面的各种属性，无论page指令出现在JSP页面中的什么地方，它作用的	都是整个JSP页面，为了保持程序的可读性和遵循良好的编程习惯，page指令最好是放在整个JSP页面的起始位置。

### 2、完整语法

> <%@ page 
> [ language="java" ] 
> [ extends="package.class" ] 
> [ import="{package.class | package.*}, ..." ] 
> [ session="true | false" ] 
> [ buffer="none | 8kb | sizekb" ] 
> [ autoFlush="true | false" ] 
> [ isThreadSafe="true | false" ] 
> [ info="text" ] 
> [ errorPage="relative_url" ] 
> [ isErrorPage="true | false" ] 
> [ contentType="mimeType [ ;charset=characterSet ]" | "text/html ; charset=UTF-8" ] 
> [ pageEncoding="characterSet | UTF-8"" ] 
> [ isELIgnored="true | false" ] 
> %>

### 3、指令详解

#### 3.1、contentType

1. 说明

   > 设置页面的编码

2. 语法格式

   ```
   <%@ page contentType=""%>
   ```

3. 示例代码1

   ```
   <%@ page contentType="text/html;charset=utf-8"%>
   这个指令的作用就相当于response.setContentType("text/html;charset=utf-8");
   ```

4. 示例代码2

   ```
   <%@ page pageEncoding="utf-8"%>
   这个指令的作用pageEncoding是jsp文件本身的编码
   ```

5. 区别

   > 1、pageEncoding是jsp文件本身的编码
   >
   > 2、contentType的charset是指服务器发送给客户端时的内容编码
   >
   > 3、执行过程
   > JSP要经过两次的“编码”，第一阶段会用pageEncoding，第二阶段会用utf-8至utf-8，第三阶段就是由Tomcat出来的网页， 用的是contentType。
   > 第一阶段是jsp编译成.java，它会根据pageEncoding的设定读取jsp，结果是由指定的编码方案翻译成统一的UTF-8 JAVA源码（即.java），如果pageEncoding设定错了，或没有设定，出来的就是中文乱码。
   > 第二阶段是由JAVAC的JAVA源码至java byteCode的编译，不论JSP编写时候用的是什么编码方案，经过这个阶段的结果全部是UTF-8的encoding的java源码。
   > JAVAC用UTF-8的encoding读取java源码，编译成UTF-8 encoding的二进制码（即.class），这是JVM对常数字串在二进制码（java encoding）内表达的规范。
   > 第三阶段是Tomcat（或其的application container）载入和执行阶段二的来的JAVA二进制码，输出的结果，也就是在客户端见到的，这时隐藏在阶段一和阶段二的参数contentType就发挥了功效

#### 3.2、import

1. 说明

   ```
   通过page指令导入java包
   ```

2. 语法格式

   ```
   <%@ page import=""%>
   ```

3. 示例代码

   ```
   可以在一条page指令的import属性中引入多个类或包，其中的每个包或类之间使用逗号分隔
   <%@ page import="java.util.Date,java.sql.,java.io."%>
   ```

#### 3.3、buffer属性

1. 说明

   > 使用这个属性是指定out对象的缓存大小，关于out对象，我们后面会说到，比如我们这里设置了out的缓冲区是4kb

2. 示例代码

   ```
   <%@ page buffer="4kb" %>
   ```

#### 3.4、session

1. 说明

   > 是指不能在本页使用session.也就是在本页面禁用了session
   >
   > 就是在将Jsp翻译成Servlet的时候不会传递Session对象了

2. 示例代码

   ```
   <%@ page session="false|true"%> 默认值是true
   ```

#### 3.5、errorPage

1. 说明

   > 这个属性是设置服务器端出错之后的错误页面的，比如我们在编写服务器代码的时候，突然抛出异常，那么会返回一个500，这样给用户的感觉就很恶心，所以我们要做的人性化一点，

2. 示例代码

   ```
   通过这个属性值，设置一个错误页面，当服务器发生错误的时候都会跳转到这个页面中
   <%@ page errorPage="/error.jsp"%>
   ```

3. 配置全局的错误界面

   > 1. web.xml文件中做配置
   >
   > 2. 错误页面问题同时jsp中的errorPage命令的优先级比较高
   >
   >    ```
   >     <error-page>  
   >        <exception-type>Exception</exception-type>  
   >        <location>/error.jsp</location>  
   >     </error-page> 
   >     
   >     <error-page>  
   >       <error-code>404</error-code>  
   >       <location>/error.jsp</location>  
   >     </error-page>  
   >     
   >     <error-page> 
   >     	<error-code>500</error-code>  
   >     	<location>/error.jsp</location> 
   >     </error-page> 
   >    ```

#### 3.6、 isErrorPages

1. 说明

   > 是否开启错误页面，就是控制上面的errorPage属性的作用开关的默认值是true

2. 语法格式

   ```
   <@ page isErrorPages="false|true"%> 
   ```

3. 示例代码

   ```
   //禁用错误界面
   <@ page isErrorPages="false" %> 
   ```

#### 3.7、isELIgnored

1. 说明

   > 是否在该页面中忽视EL表达式的功能，这个我们一般都不去做设置，因为我们在页面中肯定会使用到EL表达式的

2. 语法格式

   ```
   <@ page isELIgnored="false|true"%> 默认值是false
   ```

3. 示例代码

   ```
   禁用el表达式
   <@ page isELIgnored="true"%>
   ```

#### 3.8、注意

>  如果一个指令有多个属性，这多个属性可以写在一个指令中，也可以分开写。

## 三. Include指令

> include指令很简单，就是实现页面包含的，我们之前在介绍request的时候，说到使用代码进行页面包含，那时候我们说到了，这个代码来实现页面包含是动态包含，而使用include指令来实现页面包含是静态包含，关于静态包含和动态包含

## 四.taglib指令

> 这个指令作用也是很简单的，就是引入标签 jstl