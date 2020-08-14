## HTTP协议中的Response

* 响应消息：服务器端发送给客户端的数据

  * 数据格式：

    1. 响应行
       1. 组成：协议/版本 响应状态码 状态码描述
       2. 响应状态码：服务器告诉客户端浏览器本次请求和响应的状态。
          1. 状态码都是3位数字
          2. 分类：
             1. 1xx：服务器接收客户端的消息，但没有接收完成，等待一段时间后，发送1xx的状态码
             2. 2xx：成功，代表：200
             3. 3xx：重定向，代表：302(重定向)
             4. 4xx：客户端错误
                1. 代表：404(请求路径没有对应资源)
                2. 代表：405(请求方式无对应的doGet或者getPost方法等)
             5. 5xx：服务器端错误，代表：500(服务器内部出现异常)
    2. 响应头：
       1. 格式：头名称：值
       2. 常见响应头：
          1. Context-type：服务器向客户端说明本次响应数据的格式和编码格式
          2. Context-disposition：服务器向客户端说明打开响应体所用的格式
             * 值：
               * in-line：默认值，在当前页面打开
               * attachment;filename=xxx：以附件的形式打开响应体(文件下载)
       3. 响应空行
       4. 响应体：传输的数据

    * 响应字符串的格式

      ```
      HTTP/1.1 200 
      Content-Type: text/html;charset=UTF-8
      Content-Length: 91
      Date: Thu, 05 Dec 2019 11:19:49 GMT
      Keep-Alive: timeout=20
      Connection: keep-alive
      ```

      ```html
      <html>
      <head>
          <title>Response</title>
      </head>
      <body>
      Hello Response!
      </body>
      </html>
      ```

## Response对象

* 功能：设置响应消息

  1. 设置响应行
     1. 格式：`HTTP/1.1 200 ok`
     2. 设置状态码：`setStatus(int sc)`
  2. 设置响应头：`setHeader(String name, String value)`
  3. 设置响应体：
     * 使用步骤：
       1. 获取输出流
          * 字符输出流：`PrintWrite getWrite()`
          * 字节输出流：`ServletOutputStream getOutputStream()`
       2. 使用输出流，将数据输出到浏览器

* 案例：

  1. 重定向

     * 资源跳转的方式

     * 代码实现：

       ```java
       //设置状态码为302
       response.setStatus(302);
       //设置响应头location
       response.setHeader("location", "/day15/responseDemo2");
       ```

       ```java
       //简单的重定向方法(常用)
       response.sendRedirect("/day15/responseDemo2");
       ```

     * **重定向的特点(面试点)**：

       1. 重定向地址栏发生变化
       2. 重定向可以访问其他站点(服务器)的资源
       3. 重定向是两次请求，无法使用request对象共享数据

     * **转发的特点(面试点)**：

       1. 转发地址栏路径不变
       2. 转发只能访问当前服务器下的资源
       3. 转发是一次请求，可以使用request对象来共享数据
  
     * `forward`和`redirect`区别

     * 路径写法：

       1. 路径分类：
          1. 相对路径：通过相对路径不可以确定唯一资源
             * 如：`./index.html`
             * 不以 / 开头，以 . 开头的路径
             * 规则：找到访问当前资源和目标资源之间相对位置的关系
               * `./`：当前目录
               * `../`：后退一级目录
          2. 绝对路径：通过绝对路径可以确定唯一资源
             * 如：`http://location/day15/responseDemo2`→`/day15/responseDemo2`
             * 以`/`开头的路径 
             * 规则：判断定义的路径的使用者，判断请求的来源
               * 给客户端浏览器使用：需要增加虚拟目录(项目的访问路径 `/day15`)
                 * 建议**动态获取虚拟目录**：`request.. getContextPath()`
                 * `<a> ， <from>`，重定向等
               * 给服务器使用：不用加虚拟目录(`/day15`)
                 * 转发路径
  
  2. 服务器输出字符数据到浏览器
  
     * 步骤：
  
       1. 获取字符输出流
       2. 输出数据
  
     * 注意：
  
       * 乱码问题：
  
         1. `PrintWriter pw = response.getWriter();`获取的流的默认编码是ISO-8859-1
         2. 设置该流的默认编码
         3. 告诉浏览器响应体使用的编码
  
         ```java
         //response.setHeader("context-type", "text/html;charset=utf-8");
         response.setContentType("text/html;charset=utf-8");
         //获取流对象之前，设置流的默认编码：ISO-8859-1 设置为：GBK
         response.setCharacterEncoding("utf-8");
         ```
  
  3. 服务器输出字节数据到浏览器
  
     ```java
      //设置浏览器解析编码
      response.setContentType("text/html;charset=utf-8");
      //1. 获取字符流输出数据
      ServletOutputStream sos = response.getOutputStream();
      //2. 输出数据
      sos.write("你好".getBytes(StandardCharsets.UTF_8));
     ```
  
  4. 验证码

## ServletContext对象

1. 概念：代表整个web应用，可以和程序的容器(服务器)通信

2. 获取：

   1. 通过request对象获取
      * `request.getServletContext();`
   2. 通过HttpServlet获取
      * `this.getServletContext();`

3. 功能：

   1. 获取MIME类型：

      * MIME类型：互联网通信过程中定义的一种文件数据类型
        * 格式： 大类型/小类型 `text/html` `image/jpeg`

   2. 域对象：共享数据

      1. `setAttribute(String name, Object value)`
      2. `getAttribute(String name)`
      3. `removeAttribute(String name)`

      * ServletContext对象范围：所有用户请求的数据(服务器关闭时释放，较占内存)

   3. 获取文件的真实(服务器)路径：

      1. 方法：`String getRealPath(Sting path)`

         ```java
         String b = context.getRealpath("/b.txt");//web目录下的资源访问
         
         String c = context.getRealpath("/WEB-INF/c.txt");//WEB-INF目录下的资源访问
         
         String a = context.getRealpath("/WEB-INF/classes/a.txt");//src目录下的资源访问
         ```

         

## 案例-文件下载

* 文件下载需求：
	1. 页面显示超链接
	2. 点击超链接后弹出下载提示框
	3. 完成图片文件下载
* 分析：
  1. 超链接指向的资源如果能够被浏览器解析，则在浏览器中展示，如果不能解析，则弹出下载提示框。不满足需求
  2. 任何资源都必须弹出下载提示框
  3. 使用响应头设置资源的打开方式：
  	* content-disposition:attachment;filename=xxx
* 步骤：
  1. 定义页面，编辑超链接href属性，指向Servlet，传递资源名称filename
  2. 定义Servlet
  	1. 获取文件名称
  	2. 使用字节输入流加载文件进内存
  	3. 指定response的响应头： content-disposition:attachment;filename=xxx
  	4. 将数据写出到response输出流
* 问题：
  * 中文文件问题
  	* 解决思路：
  		1. 获取客户端使用的浏览器版本信息
  		2. 根据不同的版本信息，设置filename的编码方式不同