# JSP指令

## 一、概要

> JSP指令用来设置整个JSP页面相关的属性，如网页的编码方式和脚本语言

## 二、JSP中的三种指令标签：

| **指令**                   | **描述**                         |
| ------------------------ | ------------------------------ |
| &lt;%@ page ... %&gt;    | 定义网页依赖属性，比如脚本语言、error页面、缓存需求等等 |
| &lt;%@ include ... %&gt; | 包含其他文件                         |
| &lt;%@ taglib ... %&gt;  | 引入标签库的定义                       |

## 三、page指令

### 1、概要

> 用于设置JSP页面的属性，这些属性将用于和JSP容器通信，控制所生成的servlet结构，page指令作用整个JSP页面，可以将怕个指令放在文档中任何地方，比如脚本语言、error页面、缓存需求等等
>
> 1. 可以在一个页面上使用多个page指令但是其中的属性能只能用一次\(import除外\)
> 2. 无论page指令放在jsp的什么地方,他的作用范围都是整个jsp界面,但是为了代码的可读性,最好养成一个号的习惯把他放在jsp文件的顶部

### 2、语法格式

> &lt;%@ page 属性1="属性值" 属性2="属性值1,属性值2"…  属性n="属性值n"%&gt;

### 3、其中常见的属性如下：

> | 属性           | 描述                          | 默认值                   |
> | ------------ | --------------------------- | --------------------- |
> | import       | 通过该属性来引用脚本语言中使用到的类文件        | 无                     |
> | contentType  | 用来指定JSP页面所采用的编码方式           | text/html; ISO-8859-1 |
> | session      | 指定JSP页面是否使用session          | true                  |
> | isELIgnored  | 指定是否执行EL表达式                 | true                  |
> | isErrorPage  | 指定当前页面是否可以作为另一个JSP页面的错误处理页面 | false                 |
> | errorPage    | 指定当JSP页面发生异常时需要转向的错误处理页面    | 无                     |
> | pageEncoding | 是jsp文件编译成servlet的编码格式       | 无                     |

### 4、指令详解

#### 1、import

> page指令中唯一容许在同一文档出现多次的属性。属性的值可以以逗号隔开。
>
> 指定jsp页面转换成servlet应该输入的包。对于没有明确指定包的类，将根据jsp页面所在的包\(生成的servlet的目录\)决定类的包的位置。

#### 2、language

> 用于指定在脚本元素中使用的脚本语言，默认java。在jsp2.0规范中，只能是java。

#### 3、contentType和pageEncodeing

> contentType属性设置发送到客户端文档的响应报头的MIME类型和字符编码。多个使用;号分开。pageEncodeing属性只用于更改字符编码。
>
> servlet默认MIME是text/plain，jsp默认MIME是text/html。

#### 4、session属性

> 控制页面是否参与会话
>
> 默认true。如果存在已有会话，则预定义session变量，绑定到已有会话中。否则创建新会话将其绑定到session。
>
> 对于高流量网站，设置false可以节省大量服务器内存。  
> 设置false表示不自动创建新会话，在jsp页面转换为servlet时，这时对变量session的访问导致错误。  
> 设置为false并不是禁用会话跟踪，它只是阻止jsp页面为不拥有会话的用户创建新会话。
>
> 对于不需要会话跟踪的页面那就设置为false；当设置为false时session对象是不可访问的。

#### 5、isELlgnored属性:

> 定义在jsp页面中是否执行或忽略EL表达式。true表示忽略，false表示执行。
>
> 默认值依赖于web.xml的版本。servlet2.3之前默认true，servlet2.4默认false。  
> 用于JSP版本不一致造成使用EL表达式出现的问题。使用:isELlgnored="true";

#### 6、errorPage和isErrorPage属性：

> 指定页面专用的错误页面。
>
> 1. errorPage属性用来指定一个jsp页面，由该页面来处理当前页面中抛出但没有捕获的任何异常。指定的页面可以通过exception变量访问异常信息。
> 2. isErrorPage属性表示当前页是否可以作为其他jsp页面的错误页面。true或false。
> 3. 错误页面应该放在WEB-INF目录下面，只让服务器访问，也不会生成转发的调用，客户端只能看到最初的请求页面URL,看不到错误页面的URL。
> 4. 如果为整个web应用程序指定错误页面，或为应用中不同类型的错误指定错误处理页面，使用web.xml中的error-page元素。如果一个页面通过该属性定义了专有的错误页面，那么在web.XML文件中定义的任何错误页面不会被使用。 只能够在错误处理页面中使用错误对象exception。

## 四、include指令

### 1、概要

> JSP可以通过include指令来包含其他文件。被包含的文件可以是JSP文件、HTML文件或文本文件。包含的文件就好像是该JSP文件的一部分，会被同时编译执行，如果您没有给文件关联一个路径，JSP编译器默认在当前路径下寻找

### 2、语法格式

> &lt;%@ include file="文件相对 url 地址" %&gt;

### 3、页面包含

#### 3.1、说明

> 要想把两个页面的内容放在一个客户端界面中展示，这里介绍两种方式：静态包含和动态包含

#### 3.2、静态包含

```java
// 通过<%@ include%>
<%@include file="head.jsp" %>
```

#### 3.3、动态包含：

```java
//通过jsp:include标签完成
<jsp:include page="split.jsp">
    <jsp:param value="值" name="变量名"/>
</jsp:include>
```

#### 3.4、两者的区别：

1. 静态包含是先包含，再编译，最终会生成一个.java文件和.class文件；静态包含时，一个页面上定义的java变量，在另一个页面上可以直接使用
2. 动态包含是先编译，再包含，最终会生成两个.java文件和.class文件；动态包含时，两个页面上定义的java变量，是不能共用的，要传递参数，需要通过jsp:param来传递​    

## 五、taglib指令

### 1、概要

> 声明用户使用自定义的标签，将标签库描述符文件导入到jsp页面。
>
> 一般配合jstl el等技术使用

### 2、语法格式

> &lt;%@ taglib \(uri="tigLibURL" 或 tagDir="tagDir"\) prefix="tagPrefix" %&gt;

### 3、属性详解

1. uri属性

   > 定位标签库描述符的位置。唯一标识和前缀相关的标签库描述符，可以使用绝对或相对URL。

2. tagDir属性：

   > 指示前缀将被用于标识在WEV-INF/tags目录下的标签文件。

3. prefix属性：

   > 标签的前缀，区分多个自定义标签。不可以使用保留前缀和空前缀，遵循XML命名空间的命名约定。

### 4、示例代码

1. 引入jstl

   ```
   <%@taglib  uri="http://java.sun.com/jstl/core" prefix="c"%>   
   ```


