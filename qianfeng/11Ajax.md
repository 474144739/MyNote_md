# Ajax

Asynchronous JavaScript And XML 实现浏览器和服务器之间的异步交互（浏览器发送了请求但是页面不刷新的技术），ajax技术支持的数据格式，XML和 Json。

* JS和Java处理 Json数据

  * Json： JavaScript Object Notation，一种JavaScript内置的数据格式，采用键值对的形式存储，JS中使用大括号包围。

  * JS处理Json

    ```javascript
    function fun() {
        var j = {
            'username': 'admin',
            'password': '123'
        };
        var s = JSON.stringify(j);
        alert(j.username);
    }
    function fun2() {
        var s = '({"username":"admin","password":"123"})';
        var j = eval(s);
        // var j = eval("(" + s + ")");
        alert(j.username);
    }
    ```

  * Java处理Json

    * Java处理Json要借助第三方工具实现，常见的第三方工具：fastjson，Jackson，json-lib

    1. fastjson处理Json

       * 解析Json字符串

         * `JSONObject jsonObject = JSONObject.parseObject(json字符串)`;：将字符串转换为 Json对象
         * `jsonObject.get("username")`：使用键名获取对应的值
         * `User user = JSON.parseObject(json, User.class);`：将Json对象转为Java对象

         ```java
         // 解析Json字符串
         String json = "{username:'admin', password:'123'}";
         JSONObject jsonObject = JSONObject.parseObject(json);
         System.out.println(jsonObject.get("username"));
         System.out.println(jsonObject.get("password"));
         System.out.println("============");
         User user = JSON.parseObject(json, User.class);
         System.out.println(user);
         ```

       * 解析json数组并将数组转换为java的对象集合

         * `JSONArray jsonArray = JSONArray.parseArray(jsons); `：将字符串转换为 Json数组
         * `JSONObject jsonObject = jsonArray.getJSONObject(下标);`：在 Json数组中获取单个Json对象
         * `User user = jsonArray.getObject(i, User.class)`;：使用 Json数组，通过下标将Json数组中的 Json对象转为Java对象

         ```java
         /*
         		 * 将json格式的数组转换为java的对象集合
         		 * 解析json格式数组
         		 * 
         		 */
         String jsons = "[{username:'admin', password:'123'}, {username:'yuer', password:'456'}]";
         List<User> users = new ArrayList<User>();
         JSONArray jsonArray = JSONArray.parseArray(jsons);
         for(int i = 0; i < jsonArray.size(); i++) {
             // 解析json格式数组
             JSONObject jsonObject = jsonArray.getJSONObject(i);
             System.out.println(jsonObject.get("username"));
             // 将json格式的数组转换为java的对象集合
             User user = jsonArray.getObject(i, User.class);
             users.add(user);
         }
         for(User u : users) {
             System.out.println(u.getUsername());
             System.out.println(u.getPassword());
         }
         ```

       * 将Java对象和Java对象集合转为Json字符串

         * `String jsonStr = JSON.toJSONString(user);`：将Java对象转为Json字符串
         * `String jsonString = JSON.toJSONString(users);`：将Java对象集合转为Json字符串

         ```java
         // 将Java对象转为Json字符串
         User user = new User("admin", "123");
         String jsonStr = JSON.toJSONString(user);
         System.out.println(jsonStr);
         System.out.println("=============");
         // 将Java对象集合转换为Json字符串
         List<User> users = new ArrayList<User>();
         users.add(user);
         users.add(new User("yuer", "456"));
         String jsonString = JSON.toJSONString(users);
         System.out.println(jsonString);
         ```

         

    

