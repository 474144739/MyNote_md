### MP实现

#### 4.5乐观锁

> **适用场景**：当更新一条记录的时候，希望这条记录没有被更新，即实现线程安全的数据更新
>
> **乐观锁实现方式**：
>
> - 取出记录时，获取当前version
> - 更新时，带上这个version
> - 执行更新时：set version = newVersion where version = oldVersion
> - 如果version不对，就更新失败

> 1. 数据库中添加version字段
>
>    `ALERT TABLE user ADD COLUME version INT`
>
> 2. 

