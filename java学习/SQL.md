## DDL:操作数据库、表

#### 1. 操作数据库CRUD
1. C(Create):创建
    * 创建数据库：
        * `create database 数据库名称;`
    * 创建数据库，判断不存在，再创建：
        * `create database if not exists 数据库名称;`
    * 创建数据库并指定字符集：
        * `create database 数据库名称 character set 字符集名;`
    * 练习：创建db4数据库，判断是否存在，并指定字符集为gbk：
        * `create database if not exists db4 character set gbk;`
2. R(Retrieve):查询
    * 查询所有数据库的名称
        * `show databases;`
    * 查询某个数据库的字符集(查询某个数据库的创建语句)
        * `show create database 数据库名称;`
3. U(Update):修改
    * 修改数据库字符集
        * `alter database 数据库名称 character set 字符集名; `
4. D(Delete):删除
    * 删除数据库
        * `drop database 数据库名称;`
    * 判断数据库存在，再删除
        * `drop database if exists 数据库名称;`
5. 使用数据库
    * 查询当前正在使用的数据库名称
        * `select database();`
    * 使用数据库
        * `use 数据库名称;`

#### 2. 操作表

1. C(Create):创建
    1. 语法：
        * create 表名(<br>
        列名1 数据类型1,<br>
        列名2 数据类型2,<br>
        ....<br>
        列名n 数据类型n);
        * 注意：最后一列，不需要加逗号(,)
        * 数据类型：
            1. int : 整数类型
            2. double : 小数类型
            3. date : 日期，只包含年月日，yyyy-MM-dd
            4. datetime : 日期，包含年月日时分秒，yyyy-MM-dd  HH : mm : ss
                * 如果将来不给这个字段赋值，或者赋值为null，则默认使用当前的系统时间，来自动赋值
            6. varchar : 字符串
                * name varchar(20) : 姓名最大20个字符
    2. 创建表
        ```SQL
        create table student(
        id int,
        name varchar(20),
        score double(4,1),
        birthday date,
        insert_time timestamp
        );
        ```
    3. 复制表
        ```
        create table 表名 like 被复制的表名;
        ```
2. R(Retrieve):查询
    * 查询某个数据库中所有表名称
        * `show tables;`
    * 查询表结构
        * `desc 表名;`

3. U(Update):修改
    1. 修改表名
        ```
        alter table 表名 rename to 新的表名;
        ```
    2. 修改表的字符集
        ```
        alter table 表名 character set 字符集名称;
        ```
    3. 添加一列
        ```
        alter table 表名 add 列名 数据类型;
        ```
    4. 修改列名称，类型
        ```
        alter table 表名 change 列名 新列名 新数据类型;
        
        alter table 表名 modify 列名 新数据类型;
        ```
    5. 删除列
        ```
        alter table 表名 dorp 列名;
        ```
4. D(Delete):删除
    * `drop table 表名;`
    * `drop table if exists 表名;`

## DML：增删改表中的数据

1. 添加数据：
    * 语法：
        `insert into 表名(列名1, 列名2, ... 列名n) values(值1, 值2, ...值n);` 
    * 注意：
        1. 列名和值要一一对应。
        2. 如果表名后不定义列名，默认给所有的字段添加值 `insert into values(值1, 值2, ...值n);`
        3.除了数字类型，其他类型需要使用引号(单双都可以)引起来
2. 删除数据：
    * 语法：
        `delete from 表名 [where 条件]`
    * 注意：
        1. 如果不加条件，则删除表中所有记录。
        2. 如果要删除所有记录
            ```
            1. delete from 表名;--不推荐使用。有多少条记录就会执行多少次删除操作，降低效率。
            2. truncate table 表名;--推荐使用。先删除表，再创建一张一模一样的表，效率高。
            ```

3. 修改数据：
    * 语法：`update 表名 set 列名1 = 值1， 列名2 = 值2 ...[where 条件]`
    * 注意：如果不加任何条件，则会将表中所有记录全部修改。

## DQL：查询表中的记录
* `select * from 表名;`
1. 语法：
    ```SQL
    select
        字段列表
    from
        表名列表
    where
        条件列表
    group by
        分组字段
    having
        分组之后的条件
    order by
        排序
    limit
        分页限定
    ```
2. 基础查询：
    1. 多个字段的查询
        ```SQL
        select 字段名1, 字段名2... from 表名;
        ```
        * 注意：如果查询所有字段，则可以使用 * 来代替字段列表。
    2. 去除重复
        ```SQL
        distinct -- 所查询的记录完全相同才会去重
        ```
    3. 计算列
        * 一般可以使用四则运算计算一些列的值。(一般只会进行数值型的计算)
        ```SQL
        ifnull(表达式1, 表达式2) -- null参与运算，计算结果都为null
        # 表达式1：哪个字段需要判断是否为null
        # 如果该字段为null后的替换值
        ```
    4. 起别名
        ```SQL
        as -- as可以省略
        ```
3. 条件查询
    1. where子句后跟条件
    2. 运算符
        * `>、<、<=、>=、=、<>`
        * `between..and`
        * `in(集合)`
        * `like`
            * _:单个任意字符
            * %:多个任意字符
        * `is null`
        * `and 或 &&`
        * `or 或 ||`
        * `not 或 !`
4. 排序查询
    * 语法：order by 子句
        * order by 排序字段1 排序方式1,  排序字段2 排序方式2, ...排序字段n 排序方式n;
    * 排序方式：
        * ASC：升序，默认的。
        * DESC：降序。
    * 注意：
        * 如果有多个排序条件，则当前面的条件的值相同时，会判断第二个条件。
5. 聚合函数：将一列数据作为一个整体，进行纵向计算。
    1. count：计算个数
        1. 一般选择非空( **主键** )的列
        2. count(*)
    2. max：计算最大值
    3. min：计算最小值
    4. sun：求和
    5. avg：求平均数
    <br>
    <br>
    * 注意：聚合函数的计算，排除null的值。
    * 解决方案：
        1. 选择不包含非空的列进行计算
        2. ifnull函数
6. 分组查询：
    1. 语法：`group by 分组字段;`
    2. 注意：
        1. 分组之后查询的字段：分组字段、聚合函数
        2. where 和 having 的区别？
            * where 在分组之前进行限定，如果不满足条件，则不会参与分组。having 在分组之后进行限定，如果不满足条件，则不会被查询出来。
            * where 后不可以跟聚合函数，having 可以进行聚合函数的判断。
            ```sql
            -- 按照性别分组。分别查询男、女同学的平均分
    		SELECT sex , AVG(math) FROM student GROUP BY sex;
    		
    		-- 按照性别分组。分别查询男、女同学的平均分,人数
    		SELECT sex , AVG(math),COUNT(id) FROM student GROUP BY sex;
    		
    		--  按照性别分组。分别查询男、女同学的平均分,人数 要求：分数低于70分的人，不参与分组
    		SELECT sex , AVG(math),COUNT(id) FROM student WHERE math > 70 GROUP BY sex;
    		
    		--  按照性别分组。分别查询男、女同学的平均分,人数 要求：分数低于70分的人，不参与分组,分组之后。人数要大于2个人
    		SELECT sex , AVG(math),COUNT(id) FROM student WHERE math > 70 GROUP BY sex HAVING COUNT(id) > 2;
    		
    		SELECT sex , AVG(math),COUNT(id) 人数 FROM student WHERE math > 70 GROUP BY sex HAVING 人数 > 2;
            ```
7. 分页查询
    1. 语法：limit开始的索引，每页查询的条数；
    2. 公式：**开始的索引 = (当前页码 - 1) * 每页显示的条数**
    ```SQL
    SELECT * FROM student LIMIT 0,3; -- 第1页

    SELECT * FROM student LIMIT 3,3; -- 第2页
    
    SELECT * FROM student LIMIT 6,3; -- 第3页
    ```
    3. `limit` 分页操作是一个MySQL的“方言”
## 约束
概念：对表中的数据进行限定，保证数据的正确性、有效性和完整性。<br>
分类：
1. 主键约束：primary key
2. 非空约束：not null
3. 唯一约束：unique
4. 外键约束：foreign key 
* 非空约束：not null,某一列的值不能为null
    1. 创建表时添加非空约束
        ```SQL
        -- 创建表时添加非空约束
        CREATE TABLE stu (
        id INT,
        NAME VARCHAR ( 20 ) NOT NULL -- name为非空
        );
        ```
    2. 删除非空约束
        ```SQL
        -- 删除非空约束
        ALTER TABLE stu MODIFY NAME VARCHAR ( 20 );
        ```
    3. 创建完成表后，添加非空约束
        ```SQL
        -- 创建完成表后，添加非空约束
        ALTER TABLE stu MODIFY name VARCHAR(20) NOT NULL;
        ```
* 唯一约束：unique,某一列的值不能重复
    1. 注意：
        * 唯一约束可以有null值，但是只能有一条记录为null
    2. 创建表格时，条件唯一约束
        ```SQL
        -- 在创建表格时，条件唯一约束
        CREATE TABLE stu ( id INT, phone_number VARCHAR ( 20 ) UNIQUE );
        ```
    3. 删除唯一约束
        ```SQL
        -- 删除唯一约束
        ALTER TABLE stu DROP INDEX phone_number;
        ```
    4. 在表创建之后，添加唯一约束
        ```SQL
        -- 在表创建之后，添加唯一约束
        ALTER TABLE stu MODIFY phone_number VARCHAR ( 20 ) UNIQUE;
        ```
* 主键约束：primary key
    1. 注意：
        1. 含义：非空且唯一
        2. 一张表只能有一个字段为主键
        3. 主键就是表中记录的唯一标识
    2. 在创建表时，添加主键约束
        ```SQL
        -- 在创建表时，添加主键约束
        CREATE TABLE stu(
            id int PRIMARY KEY,
            name VARCHAR(20)
        );
        ```
    3. 删除主键
        ```SQL
        -- 删除主键
        ALTER TABLE stu DROP PRIMARY KEY;
        ```
    4. 创建表之后，添加主键
        ```SQL
        -- 创建表之后，添加主键
        ALTER TABLE stu MODIFY id INT PRIMARY KEY;
        ```
    5. 自动增长<br>
        1. 概念：如果某一列是数值类型，使用`auto_increment` 可以来完成值的自动增长
        2. 在创建表的时候，添加主键约束，并且完成主键的自增长
            ```SQL
            -- 在创建表的时候，添加主键约束，并且完成主键的自增长
            CREATE TABLE stu(
                id INT PRIMARY KEY AUTO_INCREMENT,
                name VARCHAR(20)
            );
            ```
        3. 删除自动增长
            ```SQL
            -- 删除自动增长
            ALTER TABLE stu MODIFY id INT;
            ```
        4. 创建表之后，添加自动增长
            ```SQL
            -- 创建表之后，添加自动增长
            ALTER TABLE stu MODIFY id INT AUTO_INCREMENT;
            ```
* 外键约束：foreign key，让表与表之间产生关系，从而保证数据的正确性(完整性)。
    1. 在创建表时，可以添加外键
        * 语法：
        ```SQL
        CREATE TABLE 表名(
            ....
            外键列
            CONSTRAINT 外键名称 FOREIGN KEY (外键列名称) REFERENCE 主表名称(主列表名称)
        );
        ```
    2. 删除外键
        ```SQL
        -- 删除外键
        ALTER TABLE employee DROP FOREIGN KEY 外键名称;
        ```
    3. 创建表之后，添加外键
        ```SQL
        -- 添加外键
        ALTER TABLE employee ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名称) REFERENCES 主表名称 (主表列名称);
        ```
    4. 级联操作
        1. 添加级联操作
            ```SQL
            -- 添加外键，设置级联更新，级联删除
            ALTER TABLE employee ADD CONSTRAINT emp_dept_fk FOREIGN KEY (dep_id) REFERENCES department(id) ON UPDATE CASCADE ON DELETE CASCADE;
            ```
        2. 分类：
            * 级联更新：`ON UPDATE CASCADE`
            * 级联删除：`ON DELETE CASCADE`
## 数据库设计
1. 多表之间的关系
    1. 分类：
        1. 一对一(了解)：
            * 如：人和身份证
            * 分析：一个人只有一个身份证， 一个身份证职能对应一个人
        2. 一对多(多对一)：
            * 如：部门和员工
            * 分析：一个部门有多个员工，一个员工只能对应一个部门
        3. 多对多：
            * 如：学生和课程
            * 分析：一个学生可以选择很多门课程，一个课程也可以被很多学生选择
    2. 实现关系
        1. 一对多(多对一)：
            * 如：部门和员工
            * 实现方式：在多的一方建立外键，指向一的一方的主键。
        2. 多对多：
            * 如：学生和课程
            * 实现方式：多对多关系实现需要借助第三张中间表。中间表至少包含两个字段，这两个字段作为第三章表的外键，分别指向两张表的主键。
        3. 一对一(了解)：
            * 如：人和身份证
            * 实现方式：一对一的关系实现可以在任意一方添加唯一外键指向另一方的主键。
2. 数据库设计的范式
    * 概念：设计数据库时要遵循的一些规范。要遵循后边的范式要求，必须先遵循前面的所有范式要求。
    * 分类：
        1. 第一范式(1NF)：每一列都是不可分割的原子数据项<br><br>
        2. 第二范式(2NF)：在1NF基础上，非码属性必须完全依赖于码(在1NF基础上消除传递依赖)<br><br>
            1. 几个概念：A-->B，如果通过A属性(属性组)的值，可以确定唯一B属性的值。则称B依赖A。
                * 例如：学号-->姓名。 (学号，课程名称) --> 分数
            2.  完全函数依赖：A-->B，如果A是一个属性组，则B属性值得确定需要依赖于A属性组中所有的属性值。
                * 例如：(学号，课程名称)-->分数
            3. 部分函数依赖：A-->B，如果A是一个属性组，则B属性值的确定只需要依赖于A属性组中的一些值即可。
                * 例如：(学号，课程名称)-->姓名
            4. 传递函数依赖：A-->B, B-->C, 如果通过A属性(属性组)的值，可以确定唯一B属性的值，再通过B属性(属性组)的值可以确定唯一C属性的值，则称C传递函数依赖于A。
                * 例如：学号-->系名， 系名-->系主任
            5. 码：如果在一张表中，一个属性或属性组，被其他属性所完全依赖，则称这个属性组(属性值)为该表的码。
                * 例如：该表中的码为 (学号，课程名称)
                * 主属性:码属性组中的所有属性
                * 非主属性：除过码属性组的属性
                ![例表](https://github.com/474144739/image/blob/master/YnoteImg/%E4%BE%8B%E8%A1%A8.jpg?raw=true)
        <br><br>
        3. 第三范式(3NF)：在2NF基础上，任何非主属性不依赖于其他非主属性(在2NF基础上消除传递依赖)
## 数据库的备份和还原

1. 命令行：
    * 语法
        * 备份 mysqldump -u用户名 -p密码 > 保存的路径
        * 还原：
            1. 登陆数据库
            2. 创建数据库
            3. 使用数据库
            4. 执行文件。source 文件路径
2. 图形化工具