## web概念概述
* Javaweb：
    * 使用java语言开发基于互联网的项目
* 软件架构：
    1. C/S:Client/Server 客户端/服务器端
        * 在用户本地有一个客户端程序，在远程有一个服务器端程序
        * 如：QQ，迅雷...
        * 优点：
            1. 提高了用户体验
        * 缺点：
            1. 开发，安装，部署，维护 过程复杂
    2. B/S:Browser/Server 浏览器/服务器端
        * 只需要一个浏览器，用户通过不同网址(URL)，客户可以访问不同的服务器端程序
        * 优点：
            1. 开发，安装，部署，维护 过程简单
        * 缺点：
            1. 如果应用过大，用户的体验可能会受到影响
            2. 对硬件要求过高
* B/S架构详解：
    * 资源分类：
        1. 静态资源：
            * 使用静态网页开发技术发布的资源。
            * 特点：
                * 所有用户的访问，得到的结果是一样的。
                * 如：文本，图片，音频，视频，HTML，CSS，JavaScript
                * 如果用户请求的是静态资源，那么服务器会直接将静态资源发送给浏览器。浏览器中内置了静态资源的解析引擎，可以展示静态资源
        2. 动态资源：
            * 使用动态网页及时发布的资源
            * 特点：
                * 所有用户访问，得到的结果可能不一样
                * 如：jsp/servlet,php,asp...
                * 如果用户请求的是动态资源，那么服务器会执行动态资源，转换为静态资源再发送给浏览器
    * 静态资源：
        * HTML：用于搭建基础网页，展示页面内容
        * CSS：用于美化页面，布局页面
        * JavaScript：控制页面元素，让页面有一些动态效果
## HTML
1. 概念：是最基础的网页开发语言
    * Hyper Text Markup Language 超文本传输语言
        * 超文本：
            * 超文本是用超链接的方法，将各种不同空间的文字信息组织在一起的网状文本
        * 标记语言：
            * 由标签构成的语言 <标签名称> 如：HTML，XML
            * 标记语言不是编程语言
2. 快速入门：
    * 语法：
        1. 后缀名为.html或者.htm(两者没有区别)
        2. 标签分为
            1. 围堵标签：有开始和结束的标签。如：`<body></body>`
            2. 自闭合标签：开始标签和结束标签在一起。如：`<br/>`
        3. 标签可以嵌套：`<div><a></a></div>`
        4. 在开始标签中可以定义属性。属性由键值对构成，值需要用引号引起来
        5. HTML标签不区分大小写，建议使用小写
3. 标签：
    1. 文件标签：构成HTML最基本的标签
        * html：html文档的根标签
        * head：头标签。用于指定html文档的一些属性，引入外部资源
        * title：定义标题的标签
        * body：体标签
    2. 文本标签：和文本有关的标签
        * h1 to h6：标题标签
        * p：段落标签
        * br：换行标签
        * hr：水平线标签
            * color：颜色
            * width：宽度
            * size：大小(高度)
            * align：对齐方式
                * center：居中
                * left：左对齐
                * right：右对齐
        * b：字体加粗
        * i ：字体斜体
        * center：文本居中
        * 属性定义：
			* color：
				1. 英文单词：red,green,blue
				2. rgb(值1，值2，值3)：值的范围：0~255  如  rgb(0,0,255)
				3. #值1值2值3：值的范围：00~FF之间。如： #FF00FF
			* width：
				1. 数值：width='20' ,数值的单位，默认是 px(像素)
				2. 数值%：占比相对于父元素的比例
    3. 图片标签：
        * img：
            * src：指定图片的位置
    4. 列表标签：
        * 有序列表：
			* ol:有序列表
			* li:列表项
		* 无序列表：
			* ul:无序列表
		    * li:列表项
    5. 链接标签：
        * a:定义一个超链接
			* href：指定访问资源的URL(统一资源定位符)
			* target：指定打开资源的方式
				* _self:默认值，在当前页面打开
				* _blank：在空白页面打开
    6. div和span：
		* div:每一个div占满一整行。块级标签
    	* span：文本信息在一行展示，行内标签 内联标签
    7. 语义化标签：html5中为了提高程序的可读性，提供了一些标签。
		1. `<header>`：页眉
		2. `<footer>`：页脚
	8. 表格标签：
		* table：定义表格
			* width：宽度
			* border：边框
			* cellpadding：定义内容和单元格的距离
			* cellspacing：定义单元格之间的距离。如果指定为0，则单元格的线会合为一条、
			* bgcolor：背景色
			* align：对齐方式
		* tr：定义行
			* bgcolor：背景色
			* align：对齐方式
		* td：定义单元格
			* colspan：合并列
			* rowspan：合并行
		* th：定义表头单元格
		* `<caption>`：表格标题
		* `<thead>`：表示表格的头部分
		* `<tbody>`：表示表格的体部分
		* `<tfoot>`：表示表格的脚部分
## HTML表单
* 表单：
    * 概念：用于采集用户输入数据，用于和服务器进行交互
    * form：用于定义表单，可以定义一个范围，范围代表采集用户数据的范围
        * 属性：
            * action：指定提交数据的URL
            * method：指定提交方式
                * 分类：共7种，2种较为常用
                    * get：
                        1. 请求的参数会在地址栏中显示，会封装在请求行中
                        2. 请求参数的长度(大小)有限制
                        3. 不太安全
                    * post：
                        1. 请求的参数不会在地址栏中显示，会封装在请求体中
                        2. 请求参数的长度(大小)无限制
                        3. 较为安全
        * 表单项中的数据需要被提交，必须指定其`name`属性
        * 表单项标签：
            * input：可以通过type属性值，改变元素展示的样式
                * type属性：
                    * text：文本输入框
                        * placeholder：指定输入框的提示信息，当输入框内容发生变化，会清空提示信息
                    * password：密码输入框
                        * placeholder：指定输入框的提示信息，当输入框内容发生变化，会清空提示信息
                    * radio：单选框
                        * 注意：    
                            1. 多个单选框实现单选效果，多个单选框的`name`的值必须相同
                            2. 给单选框定义`value`属性，指定其被选中后提交的值
                            3. `checked`属性，可以指定默认值
                    * checkbox：复选框
                        * 注意：
                            1. 多个复选框为一组，多个复选框的`name`的值必须相同
                            2. 给复选框定义 value 属性，指定其被选中后提交的值
                            3. 3. checked属性，可以指定默认值
                    * file：文件选择框
                    * hidden：隐藏域，用于提交一些信息
                    * submit：提交按钮，可以提交表单
                    * button：普通按钮
                    * image：图片按钮，可以提交表单
                        * src：图片路径
                    * color：取色器
                    * date：日期
                    * datetime：日期(日期加时间)
                    * email：邮箱(有基本的邮箱校验)
                    * number：数字(只能输入数字)
                * label：指定输入项的文字描述信息
                    * 注意：
                        * label的`for`属性会和`input`的`id`属性值对应。一一对应后，点击`label`区域会让`input`输入框获取焦点
            * select：下拉列表
                * 子元素：`option`，指定列表项
            * textare：文本域
                * cols：列数
                * rows：默认行数
## CSS：页面美化和布局控制
1. 概念：Cascading Style Sheets 层叠样式表
    * 层叠：多个样式可以作用在同一个HTML的元素
2. 优点：
    1. 功能强大
    2. 将内容展示和样式控制分离
        * 降低耦合度
        * 提高开发效率
        * 分工协作方便
3. CSS的使用：css与HTML的结合方式
    1. 内联样式
        * 在标签使用style属性指定css代码
        * 如：`<div style="color:red;">你好</div>`
    2. 内部样式
        * 在`head`标签中，定义`style`标签，style标签的标签体内容就是css代码
        * 如：
        ```html
        <style type="text/css">
        div{
            color:blue;
        }
        </style>
        <div>blue!</div>
        ```
    3. 外部样式
        1. 定义css资源文件。
        2. 在`head`标签中，定义link标签，引入外部的资源文件
    * 注意：
        * 1,2,3种方式，css的作用范围越来越大
        * 1方式不常用，2,3常用
        * 第3种格式可以写为：
        ```html
        <style>
            @import "css/a.css";
        </style>
        ```
4. css语法：
    * 格式：
    ```html
        选择器{
            属性名1:属性值1;
            属性名2:属性值2;
            ...
        }
    ```
    * 选择器：筛选具有相似特征的元素
    * 注意：
        * 每一对属性需要使用`;`隔开
5. 选择器：筛选具有相似特征的元素
    * 分类：
        1. 基础选择器
            1. id选择器：选择具体的id属性值的元素(唯一)
                * 语法：`#id属性值{}`
            2. 元素选择器：选择具有相同标签的元素
            3.  * 语法：`标签名{}`
                * 注意：id选择器的优先级高于元素选择器
            3. class选择器：选择具有相同的class属性值的元素
                * 语法：`.class属性值{}`
                * 注意：class选择器优先级高于元素选择器
        2. 扩展选择器
            1. 选择所有元素：
                * 语法：`*{}`
            2. 并集选择器：
                * 选择器1,选择器2{}
            3. 子选择器：
                * 语法：选择器1,选择器2{}
            4. 父选择器：选择器1的子元素为选择器2的元素
                * 语法：选择器1>选择器2{}
            5. 属性选择器：选择元素名称，属性名=属性值的元素
                * 语法：
                    1. 元素名称[属性名=属性值]{}
                    2. [属性名=属性值]{}
                    3. [属性名~=属性值]{} ：包含指定属性值的元素
            6. 伪类选择器：选择一些元素具有的状态
                * 语法：元素:状态{}
                * 如`<a>`标签：
                    * link：初始状态
                    * visitesd：被访问过的状态
                    * active：正在访问的状态
                    * hover：触碰状态
6. 属性：
    1. 字体、文本
    		* font-size：字体大小
    		* color：文本颜色
    		* text-align：对其方式
    		* line-height：行高 
	2. 背景
		* background：
	3. 边框
		* border：设置边框，符合属性
	4. 尺寸
		* width：宽度
		* height：高度
	5. 盒子模型：控制布局
		* margin：外边距
		* padding：内边距
			* 默认情况下内边距会影响整个盒子的大小
			* box-sizing: border-box;  设置盒子的属性，让width和height就是最终盒子的大小
		* float：浮动
			* left
			* right

<html>
<!--在这里插入内容-->
<a style="color:red;display:inline;text-decoration:none;font-size:3em;margin:auto;" href="https://www.w3school.com.cn/cssref/index.asp">CSS 参考手册</a>
<br><br><br><br><br>
</html>

```
== 注册页面表单案例
```
```html
<!DOCTYPE html>
<html lang="ch">
<head>
    <meta charset="UTF-8">
    <title>注册页面</title>
    <link rel="stylesheet" href="css/register.css">
</head>
<body>
<div class="regedit">
    <div class="left">
        <b>新用户注册</b>
        <b>USER REGISTER</b>
    </div>
    <div class="main">
        <table>
            <tr>
                <td class="td_left"><label for="username">用户名</label></td>
                <td class="td_right"><input type="text" name="username" id="username" placeholder="请输入用户名"></td>
            </tr>

            <tr>
                <td class="td_left"><label for="password">密码</label></td>
                <td class="td_right"><input type="password" name="password" id="password" placeholder="请输入密码"></td>
            </tr>

            <tr>
                <td class="td_left"><label for="email">Email</label></td>
                <td class="td_right"><input type="email" name="email" id="email" placeholder="请输入邮箱"></td>
            </tr>

            <tr>
                <td class="td_left"><label for="name">姓名</label></td>
                <td class="td_right"><input type="text" name="name" id="name" placeholder="请输入姓名"></td>
            </tr>

            <tr>
                <td class="td_left"><label for="tel">手机号</label></td>
                <td class="td_right"><input type="text" name="tel" id="tel" placeholder="请输入手机号"></td>
            </tr>

            <tr>
                <td class="td_left"><label>性别</label></td>
                <td class="td_right">
                    <input type="radio" name="gender" value="male"> 男
                    <input type="radio" name="gender" value="female"> 女
                </td>
            </tr>

            <tr>
                <td class="td_left"><label for="birthday">出生日期</label></td>
                <td class="td_right"><input type="date" name="birthday" id="birthday" placeholder="请输入出生日期"></td>
            </tr>

            <tr>
                <td class="td_left"><label for="checkcode">验证码</label></td>
                <td class="td_right"><input type="text" name="checkcode" id="checkcode" placeholder="请输入验证码">
                    <img src="img/verify_code.jpg" align="center">
                </td>
            </tr>

            <tr>
                <td colspan="2" align="center"><input type="submit" value=""></td>
            </tr>
        </table>
    </div>
    <div class="right">
        已有账号？<a href="#">立即登录</a>
    </div>
</div>
</body>
</html>
```
```css
* {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
}

body {
    padding: 0;
    margin: 0;
    background: url("../img/register_bg.png") no-repeat fixed;
    background-size: cover;
}

a {
    text-decoration: none;
}

a:link {
    color: red;
}

a:hover {
    color: lightseagreen;
}

a:active {
    color: orange;
}

a:visited {
    color: red;
}

.left b:nth-child(1) {
    display: block;
    color: orange;
    font-size: 30px;
    font-weight: bold;
}

.left b:nth-child(2) {
    display: block;
    color: gainsboro;
    font-size: 24px;
    font-weight: bold;
}

.regedit {
    border-radius: 20px;
    margin: 100px auto 0 auto;
    width: 1200px;
    height: 700px;
    background-color: white;
}

.left {
    margin: 30px 0 0 30px;
    width: 270px;
    height: 670px;
    float: left;
}

.main {
    width: 600px;
    float: left;
    height: 700px;
}

.right {
    text-align: right;
    margin: 30px 30px 0 0;
    width: 270px;
    height: 670px;
    float: right;
}

table {
    margin-top: 70px;
}

table td {
    padding: 0;
    height: 60px;
}

.td_left {
    padding: 0 30px 0 0;
    text-align: right;
    width: 200px;
}

.td_right {
    height: 50px;
    width: 350px;
}

.td_right #name, #password, #username, #birthday, #email, #tel {
    padding: 10px;
    width: 300px;
    height: 40px;
    border-radius: 7px;
    border: 1px solid #cccccc;
    outline:none;
}

.td_right #checkcode {
    margin-right: 10px;
    padding: 10px;
    width: 150px;
    height: 40px;
    border-radius: 5px;
    border: 1px solid #cccccc;
    outline:none;
}

[type=submit] {
    background: url("../img/regbtn.jpg") no-repeat;
    width: 101px;
    height: 32px;
    outline:none;
}
```