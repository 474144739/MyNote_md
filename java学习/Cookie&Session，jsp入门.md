## 会话技术

1. 会话：一次会话包含多次请求和响应。
   * 一次会话：浏览器第一次给服务器资源发送请求，会话建立，知道有有一方断开为止
2. 功能：在一次会话的范围内的多次请求间，共享数据
3. 方式：
   1. 客户端会话技术：Cookie
   2. 服务端会话技术：Session

## Cookie对象

1. 概念：客户端会话技术，将数据保存到客户端

2. 快速入门：

   * 步骤：
     1. 创建Cookie对象，绑定数据
        * `new Cookie(String name, String value)`
     2. 发送Cookie对象
        * `response.addCookie(Cookie cookie)`
     3. 获取Cookie对象，取数据
        * `Cookie[] request.getCookies()`

3. 原理：

   * 基于响应头set-cookie和请求头cookie实现

4. cookie细节

   1. 一次可以发送多个cookie对象，使用response调用多次`addCookie()`方法发送cookie即可
   2. cookie的保存时间：
      1. cookie默认在浏览器关闭后销毁
      2. 持久化存储：
         * `setMaxAge(int seconds)`参数:
           1. 正数：将cookie数据写到硬盘的文件中，持久化存储。值代表cookie存活时间单位: s
           2. 负数：默认值，浏览器关闭后销毁
           3. 零：删除cookie信息
   3. cookie存储中文数据：
      * Tomcat 8 版本之前，cookie无法直接存储中文数据
        * 需要将中文数据转码，一般采用URL编码(%E3)
      * Tomcat 8 版本之后，cookie支持存储中文数据(不支持特殊字符，可以使用URL编码)
   4. cookie共享：
      1. 在同一个Tomcat服务器中部署多个web项目，在这些web项目中的cookie，默认情况下不能共享
         * `setPath(String path)`：设置cookie的获取范围。默认情况下，参数设置为当前的虚拟目录
           * 参数`path`设置为`"/"`时，可以在当前服务器中共享cookie
      2. 在多台服务器中共享cookie
         * `setDomain(String name)`：如果一级域名相同，多个服务器之间可共享cookie
           * `serDomain(".baidu.com")`，那么`"tieba.baidu.com"`和`news.baidu.com`中的cookie可以共享

5. Cookie的特点和作用

   1. cookie存储数据在客户端浏览器(不安全)
   2. 浏览器对于单个cookie的大小有限制(4kb左右)以及对同一个域名下的总cookie数量有限制(20个左右)

   * 作用：
     1. cookie一般用于存储少量不太敏感的信息
     2. 在不登录的情况下，完成服务器对客户端身份识别



## 案例——利用cookie记录上次访问时间

```java
response.setContentType("text/html;charset=utf-8");
response.setCharacterEncoding("utf-8");
request.setCharacterEncoding("utf-8");
boolean flag = false;
Cookie[] cookies = request.getCookies();
SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss:SSS");
String lastTime = sdf.format(new Date());
lastTime = URLEncoder.encode(lastTime, "utf-8");
if (cookies != null) {
    for (Cookie cookie : cookies) {
        String name = cookie.getName();
        String value = cookie.getValue();
        if (name.equals("lastTime")) {
            flag = true;
            value = URLDecoder.decode(value, "utf-8");
            response.getWriter().write("<h1>欢迎回来，上次访问时间是：" + value + "</h1>");
            cookie.setValue(lastTime);
            response.addCookie(cookie);
            break;
        }
    }
}
if (cookies == null || cookies.length == 0 || !flag) {
    Cookie cookie = new Cookie("lastTime", lastTime);
    cookie.setMaxAge(60 * 60 * 24 * 30);
    response.addCookie(cookie);
    response.getWriter().write("<h1>欢迎光临，您是首次访问</h1>");
}
```

## JSP：入门学习

1. 概念：
	* Java Server Pages： java服务器端页面
		* 可以理解为：一个特殊的页面，其中既可以指定定义html标签，又可以定义java代码
		* 用于简化书写！！！
2. 原理
   * JSP本质上就是一个Servlet
3. JSP的脚本：JSP定义Java代码的方式
	1. <%  代码 %>：定义的java代码，在service方法中。service方法中可以定义什么，该脚本中就可以定义什么。
	2. <%! 代码 %>：定义的java代码，在jsp转换后的java类的成员位置。
	3. <%= 代码 %>：定义的java代码，会输出到页面上。输出语句中可以定义什么，该脚本中就可以定义什么。
4. JSP的内置对象：
   * 在JSP页面中不需要获取和创建，可以直接使用的对象
   * JSP一共有9个内置对象。
   * 今天学习3个：
   	* request
   	* response
   	* out：字符输出流对象。可以将数据输出到页面上，和`response.getWriter()`类似
   		* `response.getWriter()`和`out.write()`的区别：
   			* 在tomcat服务器真正给客户端做出响应之前，会先找response缓冲区数据，再找out缓冲区数据。
   			* `response.getWriter()`数据输出永远在`out.write()`之前
5. 案例:改造Cookie案例

## Session对象

1. 概念：服务器端会话技术，在一次会话的多次请求间共享数据，将数据保存在服务器端的对象中。`HttpSession`
2. 快速入门：
	1. 获取`HttpSession`对象：
		`HttpSession session = request.getSession();`
	2. 使用`HttpSession`对象：
		`Object getAttribute(String name)  `
		`void setAttribute(String name, Object value)`
		`void removeAttribute(String name)  `

3. 原理
	
* Session的实现是依赖于Cookie的。
	
4. 细节：
	1. 当客户端关闭后，服务器不关闭，两次获取session是否为同一个？
		* 默认情况下。不是。
		
		* 如果需要相同，则可以创建Cookie,键为JSESSIONID，设置最大存活时间，让cookie持久化保存。
	   	
	      ```java
   Cookie c = new Cookie("JSESSIONID",session.getId());
		   c.setMaxAge(60*60);
		   response.addCookie(c);
		   ```
		
	2. 客户端不关闭，服务器关闭后，两次获取的session是同一个吗？
		* 不是同一个，但是要确保数据不丢失。tomcat自动完成以下工作
			* session的钝化：
				* 在服务器正常关闭之前，将session对象序列化到硬盘上
			* session的活化：
				* 在服务器启动后，将session文件转化为内存中的session对象即可。
		
	3. session什么时候被销毁？
		1. 服务器关闭
		
		2. session对象调用invalidate() 。
	
		3. session默认失效时间 30分钟
			选择性配置修改	
			
		   ```xml
		   <session-config>
		        <session-timeout>30</session-timeout>
		    </session-config>
		   ```
	
 5. session的特点
	 1. session用于存储一次会话的多次请求的数据，存在服务器端
	 2. session可以存储任意类型，任意大小的数据

	* session与Cookie的区别：
		1. session存储数据在服务器端，Cookie在客户端
		2. session没有数据大小限制，Cookie有
		3. session数据安全，Cookie相对于不安全