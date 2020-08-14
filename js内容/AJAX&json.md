# AJAX

##### 概念

1. Ajax 即“Asynchronous Javascript And XML”（异步 JavaScript 和 XML），是指一种创建交互式、快速动态网页应用的网页开发技术，无需重新加载整个网页的情况下，能够更新部分网页的技术。
2. 通过在后台与服务器进行少量数据交换，Ajax 可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。

##### 实现方式

1. 原生JS的实现方式（了解）

   1. 

2. JQuery实现方式

   1. `$.ajax()`
      - 语法：`$ajax({键值对});`
   2. `$.get()`发送get请求
      - 语法：`$get(url, [data], [callback], [type])`[ ]为可选
        - 参数：
          - url：请求路径
          - data：请求参数
          - callback：回调函数
          - type：响应结果的类型
   3. `$.post()`发送post请求
      - 语法：`$post(url, [data], [callback], [type])`[ ]为可选
        - 参数：
          - url：请求路径
          - data：请求参数
          - callback：回调函数
          - type：响应结果的类型

# Json

   ##### 概念

   - JavaScript Object Notation， JavaScript对象的表示法
   - Json现在多用于存储和交换文本信息的语法，类似xml，进行数据传输
   - 比xml更小，更快，更易解析

   ##### 例子

   ```JavaScript
   var p = {"name":"张三", "age":"23", "gender":"男"};
   ```

   ##### 语法

   1. 基本规则

      1. 数据在名称/值对中：Json是由键值对构成的

         - 键：用引号(单双都行)引起来，也可以不使用引号
         - 值的取值类型：
           1. 数字(整数或者浮点数)
           2. 字符串(在双引号中)
           3. 逻辑值(true 或者 false)
           4. 数组(在方括号中)
           5. 对象(在花括号中)
           6. null
         - 对可以相互嵌套，例如：
           1. `{"address":{"province":"陕西"...}}`
           2. `{"persons":[{},{}]}`

         2. 数据由逗号分离：多个键值对由逗号分隔
         3. 花括号保存对象：使用{}定义json格式
         4. 方括号保存数组：[ ]

   2. 获取数据

      1. josn对象.键名
      2. json对象["键名"]
      3. 数组对象[索引]

   3. Json数据和Java对象的相互转换

      - Json解析器：
        - 常见的解析器：Jsonlib，Gson，fastjson，jackson

      1. Json转换为Java对象

         1. 导入Jackson的相关jar包

         2. 创建jackson核心对象ObjectMapper

         3. 调用ObjectMapper的相关方法进行转换

            * readValue(Json字符串数据, Class)

              ```java
            @Test
              public void test5() throws Exception{
            // 1. 初始化Json字符串
                  String json = "{\"gender\":\"男\",\"name\":\"张三\",\"age\":23}";
            
                  // 2. 创建一个ObjectMapper对象
            ObjectMapper mapper = new ObjectMapper();
                  // 3. 转换为Java对象，Person对象
            Person person = mapper.readValue(json, Person.class);
                  System.out.println(person);
              }
            // 输出：Person{name='张三', age=23, gender='男', birthday=null}
              ```

      2. Java对象转换为Json
      
         1. 使用步骤：
      
            1. 导入Jackson的相关jar包
      
            2. 创建jackson核心对象ObjectMapper
      
            3. 调用ObjectMapper的相关方法进行转换
      
               1. 转换方法
      
                  - writeValue(参数1, obj)
      
                    - 参数1：
      
                      1. File：将obj对象转换为Json字符串，并保存到指定的文件中
      
                      2. Writer：将obj对象转换为Json字符串，并将json数据填充到字节输出流中
                3. OutputStream：将obj对象转换为Json字符串，并将json数据填充到字节输入流中
         - writeValueAsString(obj):将对象转为json字符串
           ```java
           // 1. 创建Person对象
           Person p = new Person();
           p.setName("张三");
           p.setAge(23);
           p.setGender("男");
             
           // 2. 创建Jackson的核心对象 ObjectMapper
           ObjectMapper mapper = new ObjectMapper();
               
           // 3. 转换
           String json = mapper.writeValueAsString(p);
           // {"name": "张三", "age": 23, "gender", "男"}
           // System.out.println(json);
               
           // writeValue,将数据写到 f://a.txt文件中
           // mapper.writeValue(new File("f://a.txt"), p);
           mapper.writeValue(new FileWriter("f://b.txt"), p);
           ```
         
         2. 注解：
            
                  1. `@JsonIgnore`：排除属性（忽略属性）
                  2. `@JsonFormat`：属性值的格式化
                     * `@JsonFormat(pattern = "yyyy-MM-dd")`
              
               3. 复杂Java对象转换
              
                  1. List：数组
                  
                     ```java
                     //[{"name":"张三","age":23}, {"name":"李四","age":24}] 
                     ```
                  
                     Map：和对象格式一致
                  
                     ```java
                     //{"gender":"男","name":"张三","age":23}
                     ```
                  

# 案例

* 校验用户名是否存在

  1. 服务器响应的数据，在客户端使用时，想要当做Json数据格式使用

     1. `$.get(type)`：将最后一个参数的type指定为`"json"`

     2. 在服务器端设置MIME类型

        ```java
        response.setContentType("application/json;charset=utf-8");
        ```

        

* FindUserServlet.java

  ```java
  package cn.yuer.web.servlet;
  
  import com.fasterxml.jackson.databind.ObjectMapper;
  
  import javax.servlet.ServletException;
  import javax.servlet.annotation.WebServlet;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  import java.io.IOException;
  import java.util.HashMap;
  import java.util.Map;
  
  @WebServlet("/findUserServlet")
  public class FindUserServlet extends HttpServlet {
      protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
          // 1. 获取用户名
          String username = request.getParameter("username");
  
          // 2. 调用service层判断用户名是否存在
          // 期望服务器响应回的格式：  {"userExsit":true, "msg":"此用户名太受欢迎，请更换一个"}
          //                        {"userExsit":false, "msg":"用户名可用"}
  
          // response.setContentType("text/html;charset=utf-8");
          // 设置响应的数据格式为json
          response.setContentType("application/json;charset=utf-8");
          Map<String, Object> map = new HashMap<>();
          if ("tom".equals(username)) {
              //存在
              map.put("userExsit", true);
              map.put("msg", "此用户名太受欢迎，请更换一个");
          } else {
              //不存在
              map.put("userExsit", false);
              map.put("msg", "用户名可用");
          }
  
          // 将map转为Json，并且传到客户端
          ObjectMapper mapper = new ObjectMapper();
          mapper.writeValue(response.getWriter(), map);
      }
  
      protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
          this.doPost(request, response);
      }
  }
  
  ```

* regist.html

  ```html
  <!DOCTYPE html>
  <html lang="zh">
  <head>
      <meta charset="UTF-8">
      <title>注册页面</title>
      <script type="text/javascript" src="js/jquery-3.4.1.min.js"></script>
  
      <script type="text/javascript">
          // 在页面加载完成后
          $(function () {
              // 给username绑定blur事件
              $("#username").blur(function () {
                  // 获取username文本输入框的值
                  let username = $(this).val();
                  //发送ajax请求
                  //期望服务器响应回的格式：  {"userExsit":true, "msg":"此用户名太受欢迎，请更换一个"}
                  //                        {"userExsit":false, "msg":"用户名可用"}
                  $.get("findUserServlet", {username: username}, function (data) {
                      //判断userExsit是否为true
                      let span = $("#s_username");
                      if (data.userExsit) {
                          // 用户名存在
                          span.css("color", "red");
                          span.html(data.msg);
                      } else {
                          // 用户名不存在
                          span.css("color", "green");
                          span.html(data.msg);
                      }
                  }, "json");
              });
          });
  
      </script>
  </head>
  <body>
  <form>
  
      <label>
          <input id="username" type="text" name="username" placeholder="请输入用户名">
          <span id="s_username"></span>
      </label>
      <br>
      <label>
          <input id="password" type="password" name="password" placeholder="请输入密码">
      </label>
      <br>
      <input type="submit" value="注册">
  
  </form>
  </body>
  </html>
  ```

  