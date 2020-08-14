## JSP

1. 指令

   * 作用：用于配置jsp页面，导入资源文件

   * 格式：

     ```jsp
     <%@ 指令名称 属性名1=属性值1 属性名2=属性名2 ...%>
     ```

   * 分类：
     1. page：配置jsp页面
        * contentType：等同于response.setContentType()
          1. 设置响应体的MIME类型和字符集
          2. 设置当前jsp页面的编码(只能是高级的IDE才能生效，如果使用低级工具，则需要设置pageEncoding的属性设置当前字符集)
        * import：导包
        * errorPage：当前页面发生异常后，会自动跳转到指定的错误页面
        * isErrorPage：标识当前页面是否是错误页面
          * true:是，可以使用内置对象exception
          * false：否，默认值，不可使用内置对象exception
     2. include：页面包含的，导入页面的资源文本
     3. taglib：导入资源

2. 注释：

   1. html注释：只能注释html代码片段

      ```html
      <!-- -->
      ```

      

   2. jsp注释：推荐使用(可以注释html和jsp代码片段)

      ```jsp
      <%-- --%>
      ```

      

3. 内置对象：

   * 在jsp页面中不需要创建，直接使用对象

   * 共有9个：

     | 变量名      | 真实类型            | 作用                                  |
     | :---------- | :------------------ | ------------------------------------- |
     | pageContext | PageContext         | 当前页面共享数据，获取其他8个内置对象 |
     | request     | HttpServletRequest  | 一次请求访问的多个资源共享数据(转发)  |
     | session     | HttpSession         | 一次对话的多个请求间共享数据          |
     | application | ServletContext      | 所有用户间共享数据                    |
     | response    | HttpServletResponse | 响应对象                              |
     | page        | Object              | 当前页面(Servlet)的对象 this          |
     | out         | JspWriter           | 输出对象，数据输出到页面上            |
     | config      | ServletConfig       | Servlet的配置对象                     |
     | exception   | Throwable           | 异常对象                              |

## MVC：开发模式

1. MVC：

   1. Mode：模型(JavaBean)
      * 完成具体的业务操作，如：查询数据库，封装对象
   2. View：视图(JSP)
      * 展示数据
   3. Controller：控制器(Servlet)
      * 获取用户输入(获取请求参数)
      * 调用模型
      * 将数据交给视图进行展示

   * 优点和缺点：
     * 优点：
       1. 耦合度低，方便维护，利于分工协作。
       2. 重用性高
     * 缺点;
       1. 项目架构变复杂，对开发人员要求高

## EL表达式

1. 概念： Expression Language 表达式语言
2. 作用：替换和简化JSP页面中Java代码的编写
3. 语法：${表达式}
4. 注意：
   * JSP默认值支持EL表达式，忽略EL表达式的方式
     1. 设置`isELIgnored="true"`忽略当前JSP页面
     2. 使用`\${表达式}`：忽略当前这个EL表达式
5. 使用：
   1. 运算：
      * 运算符：
        1. 算术运算符：`+ - * /(div) %(mod)`
        2. 比较运算符：`> < >= <= == !=`
        3. 逻辑运算符：`&&(and) ||(or) !(not) `
        4. 空运算符：`empty`
           * 功能：用于判断字符串，集合，数组对象是否为null且长度是否为0
           * `${empty list}`
   2. 获取值：
      1. EL表达式只能从域对象中获取值
      2. 语法：
         1. `${域名称：键名}`：从指定域中获取指定键的值
            * 域名称：
              1. pageScope：从pageContext对象获取值
              2. requestScope：从request中获取值
              3. sessionScope：从session中获取值
              4. applicationScope：从application(ServletContext)中获取值
            * 例：在request域中存储name=张三
              * 获取：${requestScope.name}
         2. `${键名}`：表示依次从最小的域中查找是否有该键对应的值，直到找到为止。
         3. 获取对象：List结合，Map集合的值
            1. 对象：`${域名称.键名.属性名}`
               * 本质上是调用对象的getter方法
            2. List集合：`${域名称.键名[索引]}`
            3. Map集合：
               * `${域名称.键名.key名称}`
               * `${域名称.键名["key名称"]}`
   3. 隐式对象：
      * el表达式中有11个隐式对象
      * pageContext：
        * 获取JSP其他八个内置对象
          * `${pageContext.request.contextPath}`：动态获取虚拟目录

## JSTL

1. 概念：JavaServer Page Tag Library	JSP标准标签库
   * 由Apache组织提供的开源的免费JSP标签
2. 作用：用于简化和替换JSP页面上的Java代码
3. 步骤：
   1. 导入JSTL相关的jar包
   2. 引入标签库：taglilb指令：`<%@ taglib %>`
   3. 使用标签
4. 常用的JSTL标签
   1. if：相当于java中if语句
      1. 属性：
         * test 必须属性，接收boolean表达式
           * 如果表达式为true，则显示if标签体内容，如果为false，则不显示标签体内容
           * 一般情况下，test属性值会结合el表达式一起使用
      2. 注意：
         * `c:if`标签没有else，可以再使用一个`c:if`标签
   2. choose
   3. foreach