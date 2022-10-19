![[03 SQL#^0bc0f7|概述]]

# 添加数据
- 指定列添加数据：`INSERT INTO 表名 (列名1, 列名2) VALUES (数据1, 数据2);`
- 全部列添加数据：`INSERT INTO 表名 (列名1, 列名2,……) VALUES (数据1, 数据2,……);`
- 简化：`INSERT INTO 表名 VALUES (数据1, 数据2,……);`
- 批量添加数据：Value后面加逗号

# 修改数据
`UPDATE 表名 SET 列名1=值1,列名2=值2,……where条件`
案例：`update stu set sex='女' where name = '张三';`
不加where条件会对所有的数据进行操作。

# 删除数据
`delete from 表名 where条件;`
案例：`DELETE FROM stu WHERE name='张三';`
不加where条件会对所有的数据进行操作。