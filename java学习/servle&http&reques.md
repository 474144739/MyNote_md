## web服务器软件
* 服务器：安装了服务器软件的计算机
* 服务器软件：接收用户请求，处理请求，做出响应
* web服务器软件：接收用户的请求，处理请求，做出响应
    * 在web服务器软件中，可以部署web项目，让用户通过浏览器访问这些项目
    * 也称为web容器
* 常见的java相关的web服务器软件：
    * webLogic：Oracle公司，大型javaEE服务器，支持所有的javaEE规范，收费
    * webSphere：IBM公司，大型javaEE服务器，支持所有javaEE规范，收费
    * JBOSS：JBOSS公司，大型javaEE服务器，支持所有javaEE规范，收费
    * Tomcat：Apache基金组织，中小型javaEE服务器，仅支持少量的JavaEE规范servlet/jsp，开源，免费
* JavaEE：Java语言在企业级开发中使用的技术规范总和，共规定了13项大型规范
* Tomcat：web服务器软件
	1. 下载：<a href="http://tomcat.apache.org/">Tomcat官网</a>
	2. 安装：解压压缩包即可
		* 注意：安装目录建议不要有中文和空格
	3. 卸载：删除目录就行了
	4. 启动：
		* bin/startup.bat ,双击运行该文件即可
		* 访问：浏览器输入：http://localhost:8080 回车访问自己 http://别人的ip:8080 访问别人
		* 可能遇到的问题：
			1. 黑窗口一闪而过：
				* 原因： 没有正确配置JAVA_HOME环境变量
				* 解决方案：正确配置JAVA_HOME环境变量
			2. 启动报错：
				1. 暴力：找到占用的端口号，并且找到对应的进程，杀死该进程
					* `netstat -ano`
				2. 温柔：修改自身的端口号
					* `conf/server.xml`
					* `<Connector port="8888" protocol="HTTP/1.1"connectionTimeout="20000"redirectPort="8445" />`
					* 一般会将tomcat的默认端口号修改为80。80端口号是http协议的默认端口号
						* 好处：在访问时，就不用输入端口号
	5. 关闭：
		1. 正常关闭：
			* bin/shutdown.bat
			* ctrl+c
		2. 强制关闭：
			* 点击启动窗口的 ×
	6. 配置:
		* 部署项目的方式：
			1. 直接将项目放到`webapps`目录下即可
				* /hello：项目的访问路径-->虚拟目录
				* 简化部署：将项目打成一个war包，再将war包放置到`webapps`目录下
					* war包会自动解压缩
			2. 配置`conf/server.xml`文件
			    * 在<Host>标签体中配置`<Context docBase="D:\hello" path="/hehe" />`
    				* `docBase`:项目存放的路径
          				* path：虚拟目录
			3. 在conf\Catalina\localhost创建任意名称的xml文件。在文件中编写`<Context docBase="D:\hello" />`
				* 虚拟目录：xml文件的名称
		* 静态项目和动态项目：
			* 目录结构
				* java动态项目的目录结构：
					* 项目的根目录
					    * 静态资源
						* WEB-INF目录：
							* web.xml`：web项目的核心配置文件
							* classes目录：放置字节码文件的目录
							* lib目录：放置依赖的jar包
		* 将Tomcat集成到IDEA中，并且创建`JavaEE`的项目，部署项目
## IDEA和Tomcat相关配置
1. IDEA会为每一个Tomcat部署的项目单独创建一份配置文件
    - 查看控制台的log：`Using CATALINA_BASE:"C:\Users\Yuer\.IntelliJIdea2019.2\system\tomcat\_yuer"`
2. 工作空间项目 和 Tomcat部署的web项目
    - Tomcat真正访问的是"Tomcat部署的web项目"，"Tomcat部署的web项目"对应着"工作空间项目"的web目录下的所有资源
    - WEB-INF下的资源不能被浏览器直接访问
3. 断点调试：使用debug模式启动
## Servlet： server applet | Servlet基本使用
1. 概念：运行在服务器端的小程序
    - Servlet就是一个接口，定义了Java类被浏览器访问到(Tomcat识别)的规则
    - 自定义类，实现Servlet接口，复写方法
2. 快速入门
    1. 创建`JavaEE`项目
    2. 定义一个实现类，实现`Servler`接口
    3. 实现接口中的抽象方法
    4. 配置Servlet
        * 在web.xml中配置
        ```xml
        <!-- 配置Servlet -->
        <servlet>
            <servlet-name>demo1</servlet-name>
            <servlet-class>cn.yuer.web.servlet.ServletDemo1</servlet-class>
        </servlet>
        <servlet-mapping>
            <servlet-name>demo1</servlet-name>
            <url-pattern>/demo1</url-pattern>
        </servlet-mapping>
        ```
3. 执行原理：
    1. 当服务器接受到客户端浏览器的请求后，会解析请求URL路径，获取访问的Servlet的资源路径
    2. 查找web.xml文件，是否有对应的`<url-pattern>`标签体的内容
    3. 如果有，则在找到对应的`<servlet-calss>`全类名
    4. Tomcat会将字节码文件加载进内存，并且创建其对象
    5. 调用其方法
4. Servlet中的生命周期方法：
    1. 被创建：执行init方法，只执行一次
        - Servlet什么时候被创建：
            - 默认情况下，第一次访问时，Servlet被创建
            - 可以配置Servlet，指定Servlet创建的时机
                1. 第一次被访问时，创建
                    - `<load-on-startup>`的值为负整数(默认值-1)
                2. 在服务器启动时，创建
                    - `<load-on-startup>`的值为正整数
        - Servlet的`init`方法只执行一次，说明一个Servlet在内存中只存在一个，Servlet是单例的
            - 多个用户同时访问时，可能存在线程安全问题
            - 解决：尽量不要在Servlet中定义成员变量，若必须定义成员变量，不要对其修改值
    2. 提供服务：执行service方法，执行多次
        - 每次访问Servlet时，service方法都会被调用一次
    3. 被销毁：执行`destory`方法，只执行一次
        - Servlet被销毁执行时，服务器关闭时，Servlet被销毁
        - 只有服务器正常关闭时，才会执行`destory`方法
        - `destory`方法在Servlet被销毁之前执行，一般用于释放资源
5. Servlet 3.0：
    - 优点：
        - 支持注解配置，可以不需要web.xml
    - 步骤：
        1. 创建`JavaEE`项目，选择Servlet的版本3.0以上，可以不用创建web.xml
        2. 定义一个类，实现Servlet接口
        3. 复写方法
        4. 在类上使用一个注解进行配置
            * `@WebServlet("资源路径")`
6. Servlet体系结构
    ```
    Servlet -- 接口
        ↓
    GenericServlet -- 抽象类
        ↓
    HttpServlet -- 抽象类
    ```
    - `GenericServlet`：将Servlet接口中的其他方法做了默认空实现，只将service()方法作为抽象
        - 将来定义Servlet类时，可以继承`GenericServlet`，实现service()方法即可
    - `HttpServlet`：对http协议进行封装，简化操作
        1. 定义类继承`HttpServlet`
        2. 复写 `doGet/doPost` 方法
7. Servlet相关配置
    1. `urlPartten：Servlet`的访问路径
        1. 一个Servlet可以定义多个访问路径：`@WebServlet({"/demo3", "de3"})`
        2. 路径的定义规则：
            1. `/xxx`：路径匹配
            2. `/xxx/xxx`：多层路径，目录结构
            3. `*.do`:扩展名匹配
## HTTP
- 概念:Hyper Text Transfer Protocol超文本传输协议
    - 传输协议：定义了客户端和服务端通信时，发送数据的格式
    - 特点：
        1. 基于TCP/IP的高级协议
        2. 默认端口号：80
        3. 基于请求/响应模型的，一次请求对应一次响应
        4. 无状态：每次请求之间相互独立，不能交互数据
    - 历史版本(了解)：
        - 1.0：每次请求都会建立新的连接
        - 1.1：复用连接
- 请求消息数据格式
    1. 请求行
        - 请求方式 请求`url` 请求协议/版本
            -  `GET /login.html HTTP/1.1`
        - 请求方式：
            - HTTP协议中有7种请求方式，常用的有两种
                - GET：
                    1. 请求参数在请求行中，在`url`后
                    2. 请求的`url`长度有限制
                    3. (参数)不安全
                - POST：
                    1. 请求参数在请求体中
                    2. 请求的`url`长度没有限制
                    3. (参数)相对安全
    2. 请求头：客户端浏览器向服务器传递的一些信息
        - 请求头名称：请求头值
        - 常见的请求头：
            1. User-Agent：浏览器向服务器说明访问时使用的浏览器版本信息
                * 可以在服务器端获取该头的信息，解决浏览器的兼容性问题
            2. `Refererhttp：localhost//location/location`
                * 向服务器说明，当前请求的来源
                    * 作用：
                        1. 防盗链
                        2. 统计工作
    3. 请求空行
    4. 请求
    - 字符串格式
        ```
        POST /demo3 HTTP/1.1
        Host: localhost
        Connection: keep-alive
        Content-Length: 12
        Cache-Control: max-age=0
        Origin: http://localhost
        Upgrade-Insecure-Requests: 1
        DNT: 1
        Content-Type: application/x-www-form-urlencoded
        User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3941.4 Safari/537.36
        Sec-Fetch-User: ?1
        Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
        Sec-Fetch-Site: same-origin
        Sec-Fetch-Mode: navigate
        Referer: http://localhost/login.html
        Accept-Encoding: gzip, deflate, br
        Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
        Cookie: Idea-9388bfb8=c99a37b7-0e28-43af-baaa-c98d1486b096; Webstorm-290a7e11=11ef8705-133d-4363-9a4f-4b6a88c12e7c
        ```
## Request
1. Request对象和response对象原理
    1. request和response对象由服务器创建，程序员可以使用
    2. request对象用于获取请求消息，response对象用于设置响应消息
2. request对象继承体系结构：
    ```
    ServletRequest  -- 接口
        ↓ -- 继承
    HttpServletRequest  -- 接口
        ↓ -- 实现
    org.apache.catalina.connector.RequestFacade  -- 类(Tomcat)
    ```
3. request功能：
    1. 获取请求消息数据
        1. 获取请求行数据
            * `GET /day14/demo1?name=zhangsan HTTP/1.1`
            * 方法：
                1. 获取请求方式：GET
                    * `String getMethod()`
                2. **获取虚拟目录**：/day14
                    * `String getContextPath()`
                3. 获取Servlet路径：/demo1
                    * `String getServletPath()`
                4. 获取GET方式请求参数：`name=zhangsan`
                    * `String getQueryString()`
                5. **获取请求URI**：/day14/demo1
                    * `String getRequestURI()`：`/day14/demo1`
                    * `StringBuffer getRequestURL()`：`http://localhost/day14/demo1`
                    * URI：统一资源定位符：`/day14/demo1`
                    * URL：统一资源标识符：`http://localhost/day14/demo1`
                6. 获取协议及版本：HTTP/1.1
                    * `String getProtocol()`
                7. 获取客户机的IP地址：
                    * `String getRemoteAddr()`
        2. 获取请求头数据
    		* 方法：
    			* **`String getHeader(String name)`:通过请求头的名称获取请求头的值**
    			* `Enumeration<String> getHeaderNames()`:获取所有的请求头名称
    	3. 获取请求体数据:
    	    * 请求体：只有POST请求方式，才有请求体，在请求体中封装了POST请求的请求参数
    			* 步骤：
    				1. 获取流对象
    					*  `BufferedReader getReader()`：获取字符输入流，只能操作字符数据
    					*  `ServletInputStream getInputStream()`：获取字节输入流，可以操作所有类型数据
    				2. 再从流对象中拿数据
    2. 其他功能：
        1. 获取请求参数通用方式：get和post请求方式都可以使用下列方法来获取请求参数
			1. `String getParameter(String name)`:根据参数名称获取参数值    username=zs&password=123
			2. `String[] getParameterValues(String name)`:根据参数名称获取参数值的数组  hobby=xx&hobby=game
			3. `Enumeration<String> getParameterNames()`:获取所有请求的参数名称
			4. `Map<String,String[]> getParameterMap()`:获取所有参数的map集合
			* 中文乱码问题：
			    * get方式：Tomcat 8 已经将get方式的乱码问题解决了
			    * post方式：会乱码
			        * 解决：在获得参数以前，设置request的编码`request.setCharacterEncoding("utf-8");`
		2. 请求转发
		    1. 步骤：
		        1. 通过request对象获取请求转发器对象：`RequestDispatcher getRequestDispatcher(String path)`
		        2. 使用`RequestDispatcher`对象进行转发：`forward(ServletRequest request, ServletResponse response)`
		    2. 特点：
		        1. 浏览器地址栏路径不变
		        2. 只能转发到当前的服务器内部资源中
		        3. 转发是一次请求
		3. 共享数据：
		    * 域对象：一个有作用范围的对象，可以在范围内共享数据
		    * request域：代表一次请求的范围，一般用于请求转发的多个资源中共享数据
		    * 方法：
		        1. `void setAttribute(String name,Object obj)`:存储数据
				2. `Object getAttitude(String name)`:通过键获取值
				3. `void removeAttribute(String name)`:通过键移除键值对
		4. 获取`ServletContext`
		   
		    * `ServletContext getServletContext()`