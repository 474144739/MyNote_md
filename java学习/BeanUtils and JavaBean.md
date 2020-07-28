## BeanUtils(Apache)

<a href="http://commons.apache.org/proper/commons-beanutils/">Commons BeanUtils网站</a>

1. 使用方法：用于封装JavaBean

```java
		//获取所有请求参数
        Map<String, String[]> map = req.getParameterMap();
        //创建User对象
        User loginUser = new User();
        //使用BeanUtils封装
        try {
            BeanUtils.populate(loginUser,map);
        } catch (IllegalAccessException | InvocationTargetException e) {
            e.printStackTrace();
        }
```

2. 什么是JavaBean？
   1. 要求：
      1. 类必须被public修饰
      2. 必须提供空参的构造器
      3. 成员变量必须使用private修饰
      4. 提供公共的setter和getter方法
   2. 功能：封装数据
3. 概念：
   1. 成员变量：
   2. 属性：根据setter和getter方法截取后的产物
      * 如：`get Username()`→`Username`→`username`
   3. 方法：
      1. `setProperty()`
      2. `getProperty()`
      3. `populate()`(掌握)

