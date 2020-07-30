# Tomcat

Tomcat目录：

* bin：二进制目录，Tomcat程序运行时需要加载的二进制文件，也包含可执行文件
* **conf**：Tomcat的配置文件目录
* lib：Tomcat是Java开发，lib中包含Tomcat运行时需要加载的jar包工具
* logs：Tomcat运行日志文件夹
* temp：临时文件夹，一般用不上
* webapps：部署在Tomcat上的项目所在文件夹
* work：Javaweb应用启动后生成的工作目录，往往存放jsp的处理文件

# Servlet

Servlet无法独立运行，需要通过服务器接收到请求后再执行Servlet的程序来处理请求

* Servlet demo

  ![image-20200730170001963](img/image-20200730170001963.png)

  * src：放置Java代码

  * build：构建项目文件夹，程序员无需操作

  * WebContent：放置Web资源（比如HTML，CSS，JS，JSP，图片等其他多媒体资源）的目录
    * META-INF：项目元数据信息，程序员无需操作
    * WEB-INF：web项目相关信息及资源，包含lib文件夹（用于放置项目所需的jar包）
      * **web.xml：这是个web项目的入口文件，配置了web项目的各种信息，往往由Tomcat容器去读取web.xml，从而加载到web项目的信息**

## Servlet详解

### HTTP协议

* HTTP协议的组成部分

  * HTTP制定的是超文本传输协议，往往应用于互联网应用，互联网应用的功能分为请求与响应两个部分。

  * 请求分为四部分：请求头，请求行，请求正文，空行

    * 请求行：包含请求路径（url）

    * 请求头：包含请求方式，请求主机，客户端设定的接收的内容类型

    * 请求正文：包含请求的数据

    * 空行：表示请求头的内容结束了，用于分隔请求头和正文

    * HTTP的请求方式

      ![image-20200731113814276](img/image-20200731113814276.png)

  * 响应组成部分：响应行，响应头，响应正文

    * 响应行：主要包含响应状态码（响应状态码决定了响应的内容成功与否）
    * 响应头：包含响应的内容类型，编码格式等
    * 响应正文：包含响应的具体内容

### ServletAPI

* Servlet生命周期：

  * **初始化**：当浏览器想Servlet第一次发送请求时（或者配置了load-on-startup，从而使Servlet随着Tomcat容器的启动而启动），Tomcat会调用Servlet的初始化方法，完成对Servlet的初始化，一旦初始化完成，只要该Servlet没有被销毁，那么Tomcat不会再对Servlet进行初始化。即初始化方法只执行一次。
  * **销毁**：Servlet会在Tomcat服务器停止或者重新加载启动时被销毁。
  * **处理请求**：service方法是ServletAPI中提供给Tomcat处理请求的方法，GenericServlet本身提供了service方法用于接受请求并处理。用户可自定义service方法，实现个性化请求处理。而HTTP协议下有多种请求方式，所以在service的方法的基础上由HTTPServlet增加了HTTP协议的处理方式。
  * **处理流程**：请求交给service方法，由service方法根据请求本身的请求方式，调用用户的Servlet中doGet或doPost方法。

* Servlet中提供了ServletConfig接口，该接口中提供了Servlet配置信息的方法，包含对于Servlet初始化参数的操作，以及获取Servlet上下文对象（ServletContext）的方法

  ```xml
  <servlet>
    	<servlet-name>api</servlet-name>
    	<servlet-class>servlet.APIServlet</servlet-class>
    	<!-- 初始化参数，可以在servlet中预存一些常量，类似于预存的数据 -->
    	<init-param>
    		<param-name>username</param-name>
    		<param-value>admin</param-value>
    	</init-param>
    	<!-- load-on-startup表示随着容器启动而初始化，配置的值是数字，从1开始，数字越小，越先被初始化 -->
    	<load-on-startup>1</load-on-startup>
    </servlet>
  ```

  * ServletContext对象

    Servlet上下文对象，实际表示的是整个web项目，可以利用ServletContext实现长时间保存数据，（Servlet的其中一个作用域对象）
  
  * HTTPServletRequest对象
  
    HTTPServletRequest对象由tomcat接收到请求以后，组成HTTPServletRequest对象并放置在处理请求的service方法参数中
  

