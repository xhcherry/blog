# 连接查询，自关联，子查询

连接查询基本上都是内，左，右连接

自关联：select * from 表名 as 别名1 inner join 表名 as 别名2 on 别名1.列=别名2.列

子查询：select * from 表名 where 列 in (select 列 from 表名 where 条件)