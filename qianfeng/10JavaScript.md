# JavaScript

## 简介

JavaScript简称js，是一种脚本语言，可以实现浏览器中页面和用户交互。

* 特点：

  1. 一种脚本语言，不能独立运行（没有主函数），依托于浏览器
  2. 逻辑语法结构和java类似
  3. 包含各种内置式对象：Date，Array，Reg
  4. 操作HTML元素，实现DOM编程
  5. 操作浏览器事件，实现BOM编程

* Demo:

  ```html
  <script type="text/javascript">
  			alert("hello js!");
  </script>
  ```

## JS基础语法

* JS脚本位置

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

