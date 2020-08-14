# 数据库

## 索引失效情况

* 自动类型无法使用使用索引

  ```mysql
  select * from t_person where id = '1';
  ```

* !=或者<>无法使用索引
  
  ```
  select * from t_person where name <> 'zhangsan'
  ```
  
 - is null is not null无法使用索引

   ```
   select * from t_person where name is null
   ```

 * like '%str' 无法使用索引

   ```
   select * from t_person where name like '%san'
   ```

 * 函数无法使用索引如 where substring(column,1,3) ='abc'

   ```
   select * from t_person where substring(name,1,5)='zhang'
   修改为
   select * from t_person where name like 'zhang%'
   ```

 * or连接导致索引失效

   ```
   select * from t_person where name ='zhangsan' or age =3;
   ```