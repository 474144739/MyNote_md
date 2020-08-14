## 多表查询

* 查询语法：`select 列名列表 from 表名列表 where;`
* 笛卡尔积：
    * 有两个集合A,B，取这两个集合的所有组成情况。
    * 要完成多表查询，需要消除无用的数据。
* 多表查询的分类：
    1. 内连接查询：
        1. 隐式内连接：使用where条件消除无用数据
			* 例子：
			-- 查询所有员工信息和对应的部门信息
            ```SQL
			SELECT * FROM emp,dept WHERE emp.`dept_id` = dept.`id`;
			
			-- 查询员工表的名称，性别。部门表的名称
			SELECT 
			    emp.name,-- 员工表的姓名
			    emp.gender,-- 员工表的性别
			    dept.name -- 部门表的名称
	        FROM 
	            emp,
	            dept 
            WHERE 
                emp.`dept_id` = dept.`id`;
			
			SELECT 
				t1.name, -- 员工表的姓名
				t1.gender,-- 员工表的性别
				t2.name -- 部门表的名称
			FROM
				emp t1,
				dept t2
			WHERE 
				t1.`dept_id` = t2.`id`;
			```
        2. 显式内连接：
			* 语法： `select 字段列表 from 表名1 [inner] join 表名2 on 条件`
			* 例如：
				
                ```SQL
                SELECT 
                    * 
                FROM 
                    emp 
                INNER 
                JOIN 
                    dept 
                ON 
                    emp.`dept_id` = dept.`id`;
				
				
				SELECT 
				    * 
				FROM 
				    emp 
				JOIN 
				    dept 
				ON 
				    emp.`dept_id` = dept.`id`;
				```
		3. 内连接查询注意事项：
		    1. 从哪些表中查询数据
		    2. 条件是什么
		    3. 查询哪些字段
    2. 外连接查询：
        1. 左外连接：
            * 语法：`SELECT 字段列表 FROM 表1 LEFT [OUTER] JOIN 表2 ON 条件;`
            * 查询的是左表所有数据以及其交集部分。
        2. 右外连接：
            * 语法：`SELECT 字段列表 FROM 表1 RIGHT [OUTER] JOIN 表2 ON 条件;`
            *查询的是右表所有数据以及其交集部分。
    3. 子查询：
        * 概念：查询中嵌套查询，称嵌套查询为子查询。
        * 语法：
            ```SQL
            #查询工资最高的员工信息
            
            SELECT MAX(salary) FROM emp;
            -- 查询最高工资

            SELECT * FROM emp WHERE emp.salary = 9000;
            -- 查询工资等于9000的员工
            
            SELECT * FROM emp WHERE emp.salary = (SELECT MAX(salary) FROM emp);
            -- 子查询，查询工资等于9000的员工
            ```
        * 子查询的不同情况
            1. 子查询的结果是单行单列的：
                * 子查询可以作为条件，使用运算符取判断。运算符：> < >= <= =
                ```sql
                -- 查询员工工资小于平均工资的人
                SELECT * FROM emp WHERE emp.salary < (SELECT AVG(salary) FROM emp);
                ```
            2. 子查询的结果是多行单列的：
                * 子查询可以作为条件，使用运算符IN来判断
                ```sql
                # 查询`财务部`和`市场部`所有的员工信息
                SELECT id FROM dept WHERE `NAME` = '财务部' OR `NAME` = '市场部';
                -- 查询出市场部的ID号
                -- 对ID号为3和2的部门进行查询
                SELECT * FROM emp WHERE dept_id = 3 OR dept_id = 2;
                -- 或者
                SELECT * FROM emp WHERE dept_id IN(3, 2);
                -- 简化语句使用子查询
                SELECT * FROM emp WHERE dept_id IN(SELECT id FROM dept WHERE `NAME` = '财务部' OR `NAME` = '市场部');
                SELECT * FROM emp WHERE dept_id IN(SELECT id FROM dept WHERE `NAME` IN('财务部','市场部'));
                -- 使用运算符IN
                ```
            3. 子查询的结果是多行多列的：
                * 子查询可以作为一张虚拟表作为表的查询
                ```sql
                #查询员工日期是2011-11-11之后的员工信息和部门信息
                
                -- 子查询
                SELECT * FROM dept t1,(SELECT * FROM emp WHERE emp.join_date > '2011-11-11') t2 WHERE t1.id = t2.dept_id;
                
                -- 普通内连接
                SELECT * FROM dept t1, emp t2 WHERE t2.dept_id = t1.id AND t2.join_date > '2011-11-11';
                ```
    * 多表查询练习 
        ```sql
        -- 部门表
        CREATE TABLE dept ( id INT PRIMARY KEY PRIMARY KEY, -- 部门id
        	dname VARCHAR ( 50 ), -- 部门名称
        	loc VARCHAR ( 50 ) -- 部门所在地
        );-- 添加4个部门
        INSERT INTO dept ( id, dname, loc )
        VALUES
        	( 10, '教研部', '北京' ),
        	( 20, '学工部', '上海' ),
        	( 30, '销售部', '广州' ),
        	( 40, '财务部', '深圳' );
        	
        -- 职务表，职务名称，职务描述
        CREATE TABLE job ( id INT PRIMARY KEY, jname VARCHAR ( 20 ), description VARCHAR ( 50 ) );
        -- 添加4个职务
        INSERT INTO job ( id, jname, description )
        VALUES
        	( 1, '董事长', '管理整个公司，接单' ),
        	( 2, '经理', '管理部门员工' ),
        	( 3, '销售员', '向客人推销产品' ),
        	( 4, '文员', '使用办公软件' );
        	-- 员工表
		CREATE TABLE emp (
		  id INT PRIMARY KEY, -- 员工id
		  ename VARCHAR(50), -- 员工姓名
		  job_id INT, -- 职务id
		  mgr INT , -- 上级领导
		  joindate DATE, -- 入职日期
		  salary DECIMAL(7,2), -- 工资
		  bonus DECIMAL(7,2), -- 奖金
		  dept_id INT, -- 所在部门编号
		  CONSTRAINT emp_jobid_ref_job_id_fk FOREIGN KEY (job_id) REFERENCES job (id),
		  CONSTRAINT emp_deptid_ref_dept_id_fk FOREIGN KEY (dept_id) REFERENCES dept (id)
		);
		-- 添加员工
		INSERT INTO emp(id,ename,job_id,mgr,joindate,salary,bonus,dept_id) VALUES 
		(1001,'孙悟空',4,1004,'2000-12-17','8000.00',NULL,20),
		(1002,'卢俊义',3,1006,'2001-02-20','16000.00','3000.00',30),
		(1003,'林冲',3,1006,'2001-02-22','12500.00','5000.00',30),
		(1004,'唐僧',2,1009,'2001-04-02','29750.00',NULL,20),
		(1005,'李逵',4,1006,'2001-09-28','12500.00','14000.00',30),
		(1006,'宋江',2,1009,'2001-05-01','28500.00',NULL,30),
		(1007,'刘备',2,1009,'2001-09-01','24500.00',NULL,10),
		(1008,'猪八戒',4,1004,'2007-04-19','30000.00',NULL,20),
		(1009,'罗贯中',1,NULL,'2001-11-17','50000.00',NULL,10),
		(1010,'吴用',3,1006,'2001-09-08','15000.00','0.00',30),
		(1011,'沙僧',4,1004,'2007-05-23','11000.00',NULL,20),
		(1012,'李逵',4,1006,'2001-12-03','9500.00',NULL,30),
		(1013,'小白龙',4,1004,'2001-12-03','30000.00',NULL,20),
		(1014,'关羽',4,1007,'2002-01-23','13000.00',NULL,10);
		
		-- 工资等级表
		CREATE TABLE salarygrade (
		  grade INT PRIMARY KEY,   -- 级别
		  losalary INT,  -- 最低工资
		  hisalary INT -- 最高工资
		);
		
		-- 添加5个工资等级
		INSERT INTO salarygrade(grade,losalary,hisalary) VALUES 
		(1,7000,12000),
		(2,12010,14000),
		(3,14010,20000),
		(4,20010,30000),
		(5,30010,99990);
        ```
        ```sql
        -- 需求：
			
        -- 1.查询所有员工信息。查询员工编号，员工姓名，工资，职务名称，职务描述
        -- 2.查询员工编号，员工姓名，工资，职务名称，职务描述，部门名称，部门位置
        -- 3.查询员工姓名，工资，工资等级
        -- 4.查询员工姓名，工资，职务名称，职务描述，部门名称，部门位置，工资等级
        -- 5.查询出部门编号、部门名称、部门位置、部门人数
        -- 6.查询所有员工的姓名及其直接上级的姓名,没有领导的员工也需要查询
        
        
        
        -- 1.查询所有员工信息。查询员工编号，员工姓名，工资，职务名称，职务描述
        SELECT 
        	t1.id, -- 员工编号
        	t1.ename, -- 员工姓名
        	t1.salary, -- 工资
        	t2.jname, -- 职务名称
        	t2.description -- 职务描述
        FROM 
        	emp t1, 
        	job t2
        WHERE 
        	t1.job_id = t2.id;
        
        
        -- 2.查询员工编号，员工姓名，工资，职务名称，职务描述，部门名称，部门位置
        SELECT
        	t1.id, -- 员工编号
        	t1.ename, -- 员工姓名
        	t1.salary, -- 工资
        	t2.jname, -- 职务名称
        	t2.description, -- 职务描述
        	t3.dname, -- 部门名称
        	t3.loc -- 部门位置
        FROM
        	emp t1,
        	job t2,
        	dept t3
        WHERE
        	t1.job_id = t2.id
        AND 
        	t1.dept_id = t3.id
        
        
        -- 3.查询员工姓名，工资，工资等级
        SELECT 
        	t1.ename, -- 员工姓名
        	t1.salary, -- 工资
        	t4.grade -- 工资等级
        FROM
        	emp t1, 
        	salarygrade t4
        WHERE
        	t1.salary BETWEEN t4.losalary AND t4.hisalary
        	
        	
        -- 4.查询员工姓名，工资，职务名称，职务描述，部门名称，部门位置，工资等级
        SELECT
        	t1.ename, -- 员工姓名
        	t1.salary, -- 工资
        	t2.jname, -- 职务名称
        	t2.description, -- 职务描述
        	t3.dname, -- 部门名称
        	t3.loc, -- 部门位置
        	t4.grade -- 工资等级
        FROM
        	emp t1,
        	job t2,
        	dept t3,
        	salarygrade t4
        WHERE
        	t1.job_id = t2.id 
        AND
        	t1.dept_id = t3.id
        AND
        	t1.salary BETWEEN t4.losalary AND t4.hisalary;
        
        
        -- 5.查询出部门编号、部门名称、部门位置、部门人数
        
        
        SELECT
        	dept.id 部门编号,
        	dept.dname 部门名称,
        	dept.loc 部门位置,
        	COUNT( emp.id ) 部门人数 
        FROM
        	emp,
        	dept 
        WHERE
        	emp.dept_id = dept.id 
        GROUP BY
        	emp.dept_id;
        	
        SELECT
        	dept.id 部门编号,
        	dept.dname 部门名称,
        	dept.loc 部门位置,	
        	t1.num 部门人数 
        FROM
        	dept,
        ( SELECT
        	dept_id,
        	COUNT( id ) num
        	FROM
        		emp 
        	GROUP BY
        	dept_id) t1
        WHERE
        	t1.dept_id = dept.id;
        
        
        -- 6.查询所有员工的姓名及其直接上级的姓名,没有领导的员工也需要查询
        SELECT
        	t1.ename 员工姓名,
        	t1.mgr 上级ID,
        	t2.ename 上级
        FROM
        	emp t1,
        	emp t2 
        WHERE
        	t1.mgr = t2.id;
        	
        SELECT
        	t1.ename 员工姓名,
        	t1.mgr 上级ID,
        	t2.ename 上级 
        FROM
        	emp t1
        	LEFT JOIN emp t2 ON t1.mgr = t2.id;
        ```
## 事务
1. 事务的基本介绍
    1. 概念：如果一个包含多个步骤的业务操作被事务管理，那么这些操作要么同时成功要么同时失败
    2. 操作：
        1. 开启事务：`start transaction`;
        2. 回滚：`roollback`;
        3. 提交：`commit`
    3. 例子：
        ```sql
        CREATE TABLE account (
			id INT PRIMARY KEY AUTO_INCREMENT,
			NAME VARCHAR(10),
			balance DOUBLE
		);
		-- 添加数据
		INSERT INTO account (NAME, balance) VALUES ('zhangsan', 1000), ('lisi', 1000);
        ```
    4. MySQL数据库中事务默认自动提交
        * 事务提交的两种方式：
            * 自动提交：
                * MySQL默认自动提交
                * 一条DML(增删改)语句会自动提交一次事务。
            * 手动提交：
                * Oracle 数据库默认手动提交
                * 需要先开启事务，再提交
        * 修改事务的默认提交方式：
            * 查看事务的默认提交方式：
            ```sql
            SELECT @@autocommit; 
            -- 1 自动提交
            -- 0 手动提交
            ```
            * 修改默认提交方式：
            ```sql
            set @@autocommit = 0;
            ```
2. 事务的四大特征：
    1. 原子性：是不可分割的最小操作单位，要么同时成功，要么同时失败。
    2. 持久性：当事务提交或回滚后，数据库会持久化的包存数据。
    3. 隔离性：多个事务之间相互独立。
    4. 一致性：事务操作前后，数据总量不变。
3. 事务的隔离级别(了解)
    * 概念：多个事务之间是隔离的，相互独立的。如果多个事务操作同一批数据，则会引发一些问题，设置不同的隔离级别就可以解决这些问题。
    * 存在问题：
        1. 脏读：一个事务，读取到另一个事务中没有提交的数据
        2. 不可重复读(虚读)：在同一个事务中两次读取到的数据不一样
        3. 幻读：一个事务操作(DML)数据表中所有记录，另一个事务添加了一条数据，则第一个事务查询不到自己的修改
    * 隔离级别：
        1. read uncommitted:读未提交
            * 产生的问题：脏读，不可重复读，幻读
        2. read committed：读已提交(Oracle)
            * 产生的问题：不可重复读，幻读
        3. repeatable read：可重复读(MySQL默认)
            * 产生的问题：幻读
        4. serializable：串行化
            * 可以解决所有问题
        * 注意：隔离级别从小到大安全性越来越高，但是效率越来越低
        * 数据库查询隔离级别：
            ```sql
            select @@tx_isolation;
            ```
        * 数据库设置隔离级别：
            ```sql
            set global transaction isolation level 级别字符串;
            ```
## DCL
* SQL分类：
    1. DDL:操作数据库和表
    2. DML：增删改表中的数据
    3. DQL：查询表中的数据
    4. DCL：管理用户，授权
* DBA：数据库管理员
* DCL：管理用户，授权
    1. 管理用户
        1. 添加用户：
            ```sql
            -- 创建用户
            CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';
            CREATE USER 'zhangsan'@'localhost' IDENTIFIED BY '123';
            CREATE USER 'zhangsan'@'%' IDENTIFIED BY '123';
            ```
        2. 删除用户：
            ```sql
            -- 删除用户
            DROP USER '用户名'@'主机名';
            DROP USER 'zhangsan'@'localhost';
            ```
        3. 修改用户密码：
            ```sql
            -- 修改用户密码
            UPDATE USER SET PASSWORD = PASSWORD('密码') WHERE USER = '用户名';
            UPDATE USER SET PASSWORD = PASSWORD('abc') WHERE USER = 'lisi';
            SET PASSWORD FOR '用户名'@'主机名' = PASSWORD('新密码');
            ```
            * MySQL中忘记了root的密码？
                1. cmd --> `net stop mysql`停止MySQL服务(管理员运行)
                2. 使用无验证方式启动MySQL服务：`mysql --skip-grant-tables`
                3. 打开新的cmd，打开MySQL敲回车，即可登录成功
                4. use mysql;
                5. 修改root用户的密码
                6. 重启MySQL进程和服务
        4. 查询用户：
            ```sql
            -- 查询user表
            SELECT
            	* 
            FROM
            	`user`;
            ```
            * 通配符：%表示可以在任意主机使用用户登录数据库。
    2. 权限管理：
        1. 查询权限
            ```sql
            -- 查询权限
            SHOW GRANTS FOR '用户名'@'主机名';
            SHOW GRANTS FOR 'zhangsan'@'localhost';
            ```
        2. 授权权限
            ```sql
            -- 授予权限
            GRANT 权限列表 ON 数据库名.表名 TO '用户名'@'主机名';
            GRANT SELECT ON db3.account TO 'zhangsan'@'localhost';
            -- 给张三用户所有权限
            GRANT ALL ON *.* TO 'zhangsan'@'localhost';
            ```
        3. 撤销权限
            ```sql
            -- 撤销权限
            REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名';
            REVOKE UPDATE,SELECT,DELETE ON db3.account FROM 'zhangsan'@'localhost';
            ```