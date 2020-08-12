# JSP

## JSP的运行原理

jsp的运行机制：Tomcat加载到JSP>>>转换（.jsp—.java）>>>编译（.java—.class）>>>响应。

通过JSP所转换的Java代码发现，JSP的本质是一个Servlet，只不过使用流的形式输出HTML代码的方式交给了Tomcat。

## JSP所包含的元素

1. 指令元素

   * 指令元素用于给JSP进行设置或者配置的元素，包括设置编码，转换的语言，引入的标签库，引入的其他资源等

   * 基本语法：

     ```jsp
     <%@ 指令 属性名1="属性值1" 属性名2="属性值2"... %>
     ```

     1. page指令
     2. 

## EL表达式

作用域大小：pageContext<request<session<application

## JSTL

JSTL：JSP standard Tag Library，起源于JavaEE5.0，为了减少JSP中的Java代码。

* 基础语法
  * 引入标签库所需jar包`standard.jar`，`jstl.jar`
  * JSP中使用taglib指令引入标签库
  * 