## JDBC
1. 概念：**Java DataBase Connectivity,Java数据库连接，Java语言操作数据库**
    * JDBC本质：(SUN公司)官方定义的一套操作所有关系型数据库的规则，即接口。各个数据库厂商实现这套接口提供了数据库驱动jar包。我们可以使用这套接口(JDBC)编程，真正执行的代码是驱动jar包中的实现类。
2. 快速入门：
    * 步骤：
        1. 导入驱动jar包
            1. 复制jar包到项目的libs目录下
            2. 右键add as library
        2. 注册驱动
        3. 获取数据库连接对象
        4. 定义SQL语句
        5. 获取执行的SQL对象Statement
        6. 执行SQL
        7. 处理结果
        8. 释放资源
    * 代码实现：
    ```java
        public static void main(String[] args) throws Exception {
        // 1. 导入驱动jar包
        // 2. 注册驱动
        Class.forName("com.mysql.jdbc.Driver");
        // 3. 获取数据库连接对象
        Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/db3", "root", "123456");
        // 4. 定义SQL语句
        String sql = "update account set balance = 1000 where id = 1";
        // 5. 获取执行SQL的对象Statement
        Statement stmt = conn.createStatement();
        // 6. 执行SQL
        int count = stmt.executeUpdate(sql);
        // 7. 处理结果
        System.out.println(count);
        // 8. 释放资源
        stmt.close();
        conn.close();
    }
    ```
3. 详解各个对象：
    1. DriverManager:驱动管理对象
        * 功能：
            1. 注册驱动:告诉程序该使用哪一个驱动jar包
                * 方法：
                ```
                static void registerDriver(Driver driver) 
                //注册与给定的驱动程序 DriverManager 。 
                ```
                * 写代码使用：
                ```java
                Class.forName("com.mysql.jdbc.Driver");
                ```
                * 源码里，在`com.mysql.jdbc.Driver`类中存在静态代码块
                ```java
                static {
                    try {
                        java.sql.DriverManager.registerDriver(new Driver());
                    } catch (SQLException E) {
                        throw new RuntimeException("Can't register driver!");
                    }
                }
                ```
                * 注意：MySQL5之后的jar包可以省略注册驱动的步骤。
            2. 数据库连接：
                * 方法：
                ```java
                static Connection getConnection(String url, String user, String password) 
                //尝试建立与给定数据库URL的连接。  
                ```
                * 参数：
                    * url:指定的连接路径
                        * 语法：`jdbc:mysql://ip地址(域名):端口号/数据库名称`
                        * 例子：
                        ```java
                        "jdbc:mysql://localhost:3306/db3", "root", "123456"
                        ```
                        * 细节：如果连接的是本机的MySQL服务器，并且MySQL服务器默认端口是3306，则url可以简写为：`jdbc:mysql:///数据库名称`
                    * user：用户名
                    * password：密码
    2. Connection:数据库连接对象
        * 功能：
            1. 获取执行SQL的对象
                * 方法1：
                ```java
                Statement createStatement() 
                //创建一个 Statement对象，用于将SQL语句发送到数据库。 
                ```
                * 方法2：
                ```java
                PreparedStatement prepareStatement(String sql) 
                //创建一个 PreparedStatement对象，用于将参数化的SQL语句发送到数据库。 
                ```
            2. 管理事务：
                * 开启事务：调用此方法设置参数为false，即开启事务
                ```java
                void setAutoCommit(boolean autoCommit) 
                //将此连接的自动提交模式设置为给定状态。 
                ```
                * 提交事务：
                ```java
                void commit() 
                //使自上次提交/回滚以来所做的所有更改都将永久性，并释放此 Connection对象当前持有的任何数据库锁。  
                ```
                * 回滚事务：
                ```java
                void rollback() 
                //撤消在当前事务中所做的所有更改，并释放此 Connection对象当前持有的任何数据库锁。 
                ```
    3. Statement:执行SQL的对象
        * 功能：
            1. 执行SQL
                * `boolean execute(String sql)`：可以执行任意SQL(了解)
                ```java
                boolean execute(String sql) 
                //执行给定的SQL语句，这可能会返回多个结果。 
                ```
                * `int executeUpdate(String sql)`：执行DML(insert, update, delete)语句、DDL(create, alter, drop)语句
                    * 返回值：影响的行数，可以通过这个影响的行数判断DML语句是否执行成功，返回值>0则执行成功，反之则失败
                ```java
                int executeUpdate(String sql) 
                //执行给定的SQL语句，这可能是 INSERT ， UPDATE ，或 DELETE语句，或者不返回任何内容，如SQL DDL语句的SQL语句。
                //返回值为影响的行数
                ```
                * `ResultSet executeQuery(String sql)`:执行DQL(select)语句
                ```java
                ResultSet executeQuery(String sql) 
                //执行给定的SQL语句，该语句返回单个 ResultSet对象。 
                ```
            2. 练习：
                1. account表 添加一个记录
                ```JAVA
                public static void main(String[] args) {
                    Statement stmt = null;
                    Connection conn = null;
                    try {
                        // 1. 注册驱动
                        Class.forName("com.mysql.jdbc.Driver");
                        // 2. 定义SQL
                        String sql = "insert into account values(null, '王五', 3000)";
                        // 3. 获取Connection对象
                        conn = DriverManager.getConnection("jdbc:mysql:///db3", "root", "123456");
                        // 4. 获取执行SQL的对象 Statement
                        stmt = conn.createStatement();
                        // 5. 执行SQL
                        int count = stmt.executeUpdate(sql);//影响的行数
                        // 6. 处理结果
                        System.out.println(count);
                        if (count > 0) {
                            System.out.println("添加成功！");
                        } else {
                            System.out.println("添加失败！");
                        }
                    } catch (ClassNotFoundException | SQLException e) {
                        e.printStackTrace();
                    }finally {
                        //stmt.close();
                        //conn.close();
                        //
                        if (stmt != null){
                            try {
                                stmt.close();
                            } catch (SQLException e) {
                                e.printStackTrace();
                            }
                        }
                        if (conn != null){
                            try {
                                conn.close();
                            } catch (SQLException e) {
                                e.printStackTrace();
                            }
                        }
                    }
                }
                ```
                2. account表 修改记录
                ```JAVA
                public static void main(String[] args) {
                    Connection conn = null;
                    Statement stmt = null;
            
                    try {
                        // 1. 注册驱动
                        Class.forName("com.mysql.jdbc.Driver");
                        // 2. 获取连接对象
                        conn = DriverManager.getConnection("jdbc:mysql:///db3", "root", "123456");
                        // 3. 获取SQL对象Statement
                        stmt = conn.createStatement();
                        // 4. 定义SQL
                        String sql = "update account set balance = 1500 where id = 3";
                        // 5.执行SQL
                        int count = stmt.executeUpdate(sql);
                        // 6. 处理结果
                        System.out.println(count);
                        if (count > 0) {
                            System.out.println("修改成功！");
                        } else {
                            System.out.println("修改失败！");
                        }
                    } catch (ClassNotFoundException | SQLException e) {
                        e.printStackTrace();
                    }finally {
                        // 7. 释放资源
                        if(stmt != null){
                            try {
                                stmt.close();
                            } catch (SQLException e) {
                                e.printStackTrace();
                            }
                        }
                        if (conn != null){
                            try {
                                conn.close();
                            } catch (SQLException e) {
                                e.printStackTrace();
                            }
                        }
                    }
                }
                ```
                3. account表 删除一条记录
                ```JAVA
                public static void main(String[] args) {
                    Connection conn = null;
                    Statement stmt = null;
            
                    try {
                        // 1. 注册驱动
                        Class.forName("com.mysql.jdbc.Driver");
                        // 2. 获取连接对象
                        conn = DriverManager.getConnection("jdbc:mysql:///db3", "root", "123456");
                        // 3. 获取SQL对象Statement
                        stmt = conn.createStatement();
                        // 4. 定义SQL
                        String sql = "delete from account where id = 3";
                        // 5.执行SQL
                        int count = stmt.executeUpdate(sql);
                        // 6. 处理结果
                        System.out.println(count);
                        if (count > 0) {
                            System.out.println("删除成功！");
                        } else {
                            System.out.println("删除失败！");
                        }
                    } catch (ClassNotFoundException | SQLException e) {
                        e.printStackTrace();
                    }finally {
                        // 7. 释放资源
                        if(stmt != null){
                            try {
                                stmt.close();
                            } catch (SQLException e) {
                                e.printStackTrace();
                            }
                        }
                        if (conn != null){
                            try {
                                conn.close();
                            } catch (SQLException e) {
                                e.printStackTrace();
                            }
                        }
                    }
                }
                ```
    4. ResultSet:结果集对象，封装查询结果
        * 功能：
            1. `boolean next()`:游标向下移动一行，判断当前行是否是最后一行末尾(是否有数据)，如果是返回false，如果不是返回true
            ```java
            boolean next() 
            //将光标从当前位置向前移动一行。 
            ```
            2. `getXxx(参数)`:获取数据
                * Xxx:代表数据类型 如：`int getInt()`, `String getString()`
                * 参数：
                    1. Int:代表列的编号，从1开始。如：`getInt(1)`
                    2. String:代表列的名称。 如：`getDouble("balance")`
            3. 注意：
                * 使用步骤：
                    1. 游标向下移动一行
                    2. 判断是否有数据
                    3. 获取数据
            4. 练习：
                * 定义一个方法，查询emp表的数据并将其封装为对象，然后装载集合，返回。
                    1. 定义一个Emp类
                    2. 定义方法 `public Lsit<Emp> findAll(){}`
                    3. 实现方法 `select * from emp;`
    5. PreparedStatement:执行SQL对象
        1. SQL注入问题：
            1. 输入任意用户名，输入密码：`a' or 'a' = 'a`，有一些SQL的特殊关键字参与字符串拼接，会造成安全性问题
            2. SQL：select * from user where username = 'sadfads' and password = 'a' or 'a' = 'a'
        2. 解决SQL注入问题：使用PreparedStatement对象解决这个问题
        3. 预编译的SQL：参数使用?为占位符
        4. 步骤：
            1. 导入驱动jar包
            2. 注册驱动
            3. 获取数据库连接对象
            4. 定义SQL语句
                * 注意：SQL的参数使用?占位符。如：`select * from user where username = ? and password = ?;`
            5. 获取执行的SQL对象 PreparedStatement  `Connection.preparedStatement(String sql)`
            6. 给?赋值：
                * 方法：setXxx(参数1,参数2)
                    * 参数1：?的位置，从1开始
                    * 参数2：?的值
            7. 执行SQL
            8. 处理结果
            9. 释放资源
        5. 注意：后期都会使用PreparedStatement完成增删改查的所有操作
            1. 可以防止SQL注入
            2. 效率更高
    
## 抽取JDBC工具类： JDBCUtils
* 目的：简化书写
* 分析：
    1. 注册驱动
    2. 抽取一个方法获取连接对象
        * 需求：不想传递参数，过程繁琐，但是要保证工具类通用
        * 解决方案：配置文件
            * jdbc.properties
                ```properties
                url=jdbc:mysql:///db3
                user=root
                password=123456
                driver=com.mysql.jdbc.Dirver
                ```
    3. 抽取一个方法释放资源
* 代码实现:
```java
public class JDBCUtils {
    private static String url;
    private static String user;
    private static String password;
    private static String driver;
    /*
      文件的读取，只需要读取一次即可拿到这些值。使用静态代码块
     */
    static {
        //读取资源文件，获取值。
        try {
            //1. 创建Properties集合类
            Properties pro = new Properties();

            //获取src路径下文件的方式-->ClassLoader 类加载器
            ClassLoader classLoader = JDBCUtils.class.getClassLoader();
            URL res = classLoader.getResource("jdbc.properties");
            String path = res.getPath();
            //System.out.println(path);
            //2. 加载文件
            //pro.load(new FileReader("E:\\YUER\\IDEA-workspace\\yuer\\day04_jdbc\\src\\jdbc.properties"));
            pro.load(new FileReader(path));
            //3. 获取值，赋值
            url = pro.getProperty("url");
            user = pro.getProperty("user");
            password = pro.getProperty("password");
            driver = pro.getProperty("driver");
            //4. 注册驱动
            Class.forName(driver);
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
    /**
     * 获取连接
     *
     * @return 连接对象
     */
    public static Connection getConnection() throws SQLException {
        return DriverManager.getConnection(url, user, password);
    }
    /**
     * 释放资源
     *
     * @param rs   ResultSet对象
     * @param stmt Statement对象
     * @param conn Connection对象
     */
    public static void close(ResultSet rs, Statement stmt, Connection conn) {
        if (rs != null) {
            try {
                rs.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (stmt != null) {
            try {
                stmt.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (conn != null) {
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }

    /**
     * 释放资源
     *
     * @param stmt Statement对象
     * @param conn Connection对象
     */
    public static void close(Statement stmt, Connection conn) {
        if (stmt != null) {
            try {
                stmt.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (conn != null) {
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
```
* 练习
    * 需求：
        1. 通过键盘键入用户名和密码
        2. 判断用户是否登录成功
            * `select * from user where username = "" and password = "";`
            * 如果这个SQL有查询结果，则成功，反之，则失败。
    * 步骤：
        1. 创建数据库表 user
        ```sql
        CREATE TABLE USER(
        	id INT PRIMARY KEY AUTO_INCREMENT,
        	username VARCHAR(32),
        	password VARCHAR(32)
        );
        
        INSERT INTO USER
        VALUES
        	( NULL, 'zhangsan', '123' );
        INSERT INTO USER
        VALUES
        	( NULL, 'lisi', '234' );
        ```
        2. 创建 login 方法
        ```java
        /**
         * 登录方法
         */
        private boolean login(String username, String password) {
            if (username == null || password == null) {
                return false;
            }
            //连接数据库判断是否登录成功
            Connection conn = null;
            PreparedStatement pstmt = null;
            ResultSet rs = null;
            try {
                //1. 获取连接
                conn = JDBCUtils.getConnection();
                //2. 定义SQL
                String sql = "select * from user where username = ? and password = ? ;";
                //3. 获取执行SQL的对象
                pstmt = conn.prepareStatement(sql);
                pstmt.setString(1,username);
                pstmt.setString(2, password);
                //4. 执行查询，不需要sql
                rs = pstmt.executeQuery();
                //5. 判断
                return rs.next();
            } catch (SQLException e) {
                e.printStackTrace();
            } finally {
                JDBCUtils.close(rs, pstmt, conn);
            }
            return false;
        }
        ```
## JDBC控制事务
1. 事务：一个包含多个步骤的业务操作，如果这个操作被事务管理，那么着多个步骤要么同时成功，要么同时失败。
2. 操作：
    1. 开启事务
    2. 提交事务
    3. 回滚事务
3. 使用Connection对象来管理事务
    * 开启事务：`void setAutoCommit(boolean autoCommit) `调用此方法设置参数为false，即开启事务
        * 在执行SQL之前开启事务
    * 提交事务：`void commit() `
        * 当所有SQL都执行完提交事务
    * 回滚事务：`void rollback() `
        * 在catch中回滚事务