# JavaScript

## 简介

JavaScript简称js，是一种脚本语言，可以实现浏览器中页面和用户交互。

* 特点：
<<<<<<< HEAD
  
=======

>>>>>>> remotes/mdnote/master
  1. 一种脚本语言，不能独立运行（没有主函数），依托于浏览器
  2. 逻辑语法结构和java类似
  3. 包含各种内置式对象：Date，Array，Reg
  4. 操作HTML元素，实现DOM编程
  5. 操作浏览器事件，实现BOM编程

* Demo:
<<<<<<< HEAD
  
  ```html
  <script type="text/javascript">
      alert("hello js!");
=======

  ```html
  <script type="text/javascript">
  			alert("hello js!");
>>>>>>> remotes/mdnote/master
  </script>
  ```

## JS基础语法

* JS脚本位置
<<<<<<< HEAD
  
  1. `<head></head>`标签中，使用`<script></script>`标签实现
  
  2. `<body></body>`标签中，使用`<script></script>`标签实现
  
  3. 在JS文件中，使用`<script type="text/javascript" src=""></script>`标签引入JS文件
  
  4. 事件驱动，放置在标签的事件中
     
     ```html
     <!DOCTYPE html>
     <html>
      <head>
          <meta charset="utf-8">
          <title></title>
     
          <!-- head标签中 -->
          <script>
              alert("head中警告框");
          </script>
      </head>
      <body>
     
          <!-- body标签中 -->
          <script>
              alert("body中的警告框");
          </script>
     
          <!-- 引入外部JS文件 -->
          <script src="script.js"></script>
     
          <!-- 放置在标签的事件中 -->
          <input type="button" value="点击" onclick="alert('我被点击了')"/>
      </body>
     </html>
     ```

* 语法内容
  
  * JS中的变量
    
    * js是一个弱类型的脚本语言
      
      ```javascript
       var i = 10;
       var name = "yuer";
       var arr = [2, 43, 'abc'];
      ```
  
  * JS中的数据类型
    
    * 数字类型
      
      ```javascript
       var i = 10;
      ```
    
    * 字符串类型
      
      ```javascript
       var s1 = "abc";
       var s2 = "123";
       //js中允许单引号和双引号互相嵌套
      ```
    
    * 数组类型
      
      ```javascript
       var arr1 = [1, 2, 3, 4, 5];
       var arr2 = new Array();
       arr[0] = 12;
       //如果数组的某个位置未赋值，那么其内容是空字符串
      ```
    
    * 日期类型
      
      ```javascript
       var date = new Date();
      ```
    
    * 对象类型
      
      ```javascript
       var user = {
           "id":1,
           "username":"yuer",
           "password":"123456",
       }
       alert(user.id);
       alert(user.username);
       alert(user.password);
      ```
  
  * JS中的语句
    
    * 条件判断语句
      
      ```javascript
       var age = 14;
       if(age >= 18){
           alert("已成年");
       } else if (age >= 14){
           alert("青少年");
       } else {
           alert("少年");
       }
       switch(age){
           case 14:
               alert("14岁");
               break;
           case 15:
               alert("15岁");
               break;
           default:
               alert("hhh");
               break;
       }
      ```
    
    * 循环语句
      
      ```javascript
       var arr = [1, 2, 3, 4, 5, 'abc', "123"];
       for (var i = 0; i < arr.length; i++) {
           alert(arr[i]);
       }
       while(i < arr.length){
           alert(arr[i]);
           i++;
       }
      ```
    
    * 其他语法
      
      * 两种数据类型：null,undefined
        * null：当直接操作一个未定义变量时，实际上在JS中属于null变量，操作null变量会报错
        * undefined：当定义一个变量但未进行初始化的时候，该变量是一个undefined变量，undefined不会报错
  
  * JS函数
    
    * JS中自定义函数
      
      ```javascript
       function 函数名 (参数列表) {
           函数体;
           [return]
       }
       //定义的函数在浏览器往往不能直接运行，需要通过调用或者事件驱动执行
      ```
    
    * 事件驱动
      
      * HTML所有的元素都包含事件属性，表示html元素被执行的操作，比如按钮有点击事件，输入框有修改事件等等。当发生事件时可以驱动执行对应的js函数

## DOM编程

* JS操作普通标签（非表单标签）
  
  * document提供了按照元素的属性获取HTML元素对象的方法
  1. 获取标签
     
     ```javascript
     //根据id获取元素
     var p1 = document.getElementById("p1");
     //根据class名称获取元素
     var p2 = document.getElementByClassName("p2");
     //根据标签名获取元素，获取所有的p标签
     var tag_p = document.getElementByTagName("p");
     ```
  
  2. 操作普通标签显示的内容
     
     ```javascript
     var  p = document.getElementById("p1");
     var context = p.innerText;
     var context2 = p.innerHTML;
     //通过innerHTML或者innerText实现对标签内容的操作。
     //区别是innerText不会解析HTML代码，而innerHTML可以解析HTML代码。
     p.innerText = "<a href='www.baidu.com'>更新一下内容</a>";
     p.innerHTML = "<a href='www.baidu.com'>更新一下内容</a>";
     ```
  
  3. 普通标签样式的操作
     
     ```html
     <script type="text/javascript">
         function fun(){
         var p = document.getElementById("p1");
     		p.style.color = "red";
             p.style.fontSize = "40px";
             p.style.textAlign = "center";
     }
     </script>
     <p id = "p1">
         这是一个段落
     </p>
     ```
     
  4. 给标签增加子标签
  
     ```html
     <p id="p1">
         <a href="dom_普通标签.html">p中的第一个超链接</a>
     </p>
     <input type="button" value="add" onclick="add()"/>
     <script>
         function add(){
             //创建一个a标签
             var a = document.createElement("a");
             a.href = "www.baidu.com";
             //创建一个文本节点
             var context = document.createTextNode("p中的第二个超链接");
             //将文本节点增加到创建的a标签中
             a.appendChild(context);
             //将创建的a标签放置到p标签中
             var p = document.getElementById("p1");
             p.appendChile(a);
         }
     </script>
     ```
  
* JS操作表单标签

  1. 表单标签内容的操作

     ```javascript
     function fun(){
         var username = document.getElementById("username");
         var username_value = username.value;
         alert(username_value);
         //修改输入框中的内容
         var v = username_value + "哈哈哈";
         username_value = v;
     }
     ```

  2. 单选框和复选框的操作

     ```javascript
     function fun () {
         var hobby = document.getElementByClassName("hobby");
         for (var i = 0; i < hobby.length; i++) {
             if (hobby[i].checked) {
                 hobby[i].checked = false;
             } else {
                 hobby[i].checked = true;
             }
         }
     }
     ```

  3. 下拉列表的操作

     ```javascript
     function fun1 () {
         var address = document.getElementById("address");
         alert(address.value);
     }
     function fun2 () {
         var op = document.createElement("option");
         var context = document.createTextNode("台湾");
         op.appendChild(context);
         var address = document.getElementById("address");
         address.appendChild(op);
     }
     ```

* JS实现表单数据验证

  * 表单数据验证就是对用户所填写的表单数据进行校验的一个过程，校验的目的是为了用户填写的数据符合要求

  * 数据的常见校验

    * 必填校验，长度校验，规则校验

    1. 必填校验：某元素必须填写

       ```javascript
       var username = document.getElementById("username").value;
       if(username.length == 0){
           alert("请填写用户名");
       }
       ```

    2. 长度校验：某个表单的内容必须符合要求的长度

       ```javascript
       var password = document.getElementById("password").value;
       function checkPassword () {
           if(password.length < 8) {
               alert("密码不能低于8位");
           }
       }
       ```

    3. 规则校验

       * 验证内容是否符合规则（邮箱规则，省份证号规则等）
       * 特殊规则可以使用正则表达式体现出规则，判断内容是否符合正则表达式

       ```javascript
       var zip_code = document.getElementById("zip_code").value;
       var reg = /^\d$/;
       if (reg.test(zip_code)) {
           alert(符合要求);
       } else {
           alert("不符合要求");
       }
       ```

       * 正则表达式

         ```txt
         [ ] 表示某个字符的范围，比如[0-9]:该位置上只能放数字，[A-Z]:该位置上只能放置大写字母，[a-z]该位置上只能放小写字母，[0-9A-Za-z]:该位置上可以放字母和数字。
         { }:表示重复次数，{6}: 表示某个范围的内容限定的长度是6位，比如[0-9]{6}:表示6位数字。{6,}: 表示某个范围的内容至少6位，{6，8}：表示某个范围的内容的长度是6-8(含)位。
         (): 表示一组分组，可以匹配不同的分组
         特殊字符：
         \d: 数字。
         \D: 非数字
         \w: 普通字符，字母数字下划线
         \W: 小写的w的反义。
         \b: 表示退格
         \s: 表示空白字符
         \S: 表示非空白字符
         . :  表示除换行符以外的任意字符
         关于重复次数的特殊符号：
         *：表示可以重复0次或多次
         ？：表示0次或1次
         +： 表示1次或多次
         关于首尾限制的符号：
         ^: 表示开始
         $: 表示结束
         ```

## BOM编程
=======

   1. `<head></head>`标签中，使用`<script></script>`标签实现
   2. `<body></body>`标签中，使用`<script></script>`标签实现
   3. 在JS文件中，使用`<script type="text/javascript" src=""></script>`标签引入JS文件
   4. 事件驱动，放置在标签的事件中

   ```html
   <!DOCTYPE html>
   <html>
   	<head>
   		<meta charset="utf-8">
   		<title></title>
           
           <!-- head标签中 -->
   		<script>
   			alert("head中警告框");
   		</script>
   	</head>
   	<body>
           
           <!-- body标签中 -->
   		<script>
   			alert("body中的警告框");
   		</script>
           
           <!-- 引入外部JS文件 -->
   		<script src="script.js"></script>
           
           <!-- 放置在标签的事件中 -->
   		<input type="button" value="点击" onclick="alert('我被点击了')"/>
   	</body>
   </html>
   ```

* 语法内容

   * JS中的变量

      * js是一个弱类型的脚本语言

         ```javascript
         var i = 10;
         var name = "yuer";
         var arr = [2, 43, 'abc'];
         ```

   * JS中的数据类型

      * 数字类型

         ```javascript
         var i = 10;
         ```

      * 字符串类型

         ```javascript
         var s1 = "abc";
         var s2 = "123";
         //js中允许单引号和双引号互相嵌套
         ```

      * 数组类型

         ```javascript
         var arr1 = [1, 2, 3, 4, 5];
         var arr2 = new Array();
         arr[0] = 12;
         //如果数组的某个位置未赋值，那么其内容是空字符串
         ```

      * 日期类型

         ```javascript
         var date = new Date();
         ```

      * 对象类型

         ```javascript
         var user = {
             "id":1,
             "username":"yuer",
             "password":"123456",
         }
         alert(user.id);
         alert(user.username);
         alert(user.password);
         ```

   * JS中的语句

      * 条件判断语句

         ```javascript
         var age = 14;
         if(age >= 18){
             alert("已成年");
         } else if (age >= 14){
             alert("青少年");
         } else {
             alert("少年");
         }
         switch(age){
             case 14:
                 alert("14岁");
                 break;
             case 15:
                 alert("15岁");
                 break;
             default:
                 alert("hhh");
                 break;
         }
         ```

      * 循环语句

         ```javascript
         var arr = [1, 2, 3, 4, 5, 'abc', "123"];
         for (var i = 0; i < arr.length; i++) {
             alert(arr[i]);
         }
         while(i < arr.length){
             alert(arr[i]);
             i++;
         }
         ```

      * 其他语法

         * 两种数据类型：null,undefined
            * null：当直接操作一个未定义变量时，实际上在JS中属于null变量，操作null变量会报错
            * undefined：当定义一个变量但未进行初始化的时候，该变量是一个undefined变量，undefined不会报错

   * JS函数

      * JS中自定义函数

         ```javascript
         function 函数名 (参数列表) {
             函数体;
             [return]
         }
         //定义的函数在浏览器往往不能直接运行，需要通过调用或者事件驱动执行
         ```

      * 事件驱动

         * HTML所有的元素都包含事件属性，表示html元素被执行的操作，比如按钮有点击事件，输入框有修改事件等等。当发生事件时可以驱动执行对应的js函数

## DOM编程

## BOM编程

>>>>>>> remotes/mdnote/master
