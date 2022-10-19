![[03 SQL#^0ff6dc|概述]]

# 操作数据库
## 查询
`show databases;`

## 创建
`create dadabases name`
name为数据库的名称
如果数据库存在，则会报错，删除前让代码判断一下：
`create dadabases if not exists name`

## 删除
`drop databases name`
`drop databases if exists name`

## 使用数据库
`use name`

# 操作表
## 创建Create
```SQL
create table name(
字段名 数值类型,
字段名 数值类型,
字段名 数值类型,
……
字段名 数值类型
);
```
MySQL支持多种类型，可以分为三类：
1. 数值
2. 日期
3. 字符串
 
| 分类           | 数据类型       | 大小                  | 描述                            |
| -------------- | -------------- | --------------------- | ------------------------------- |
| 数值类型        | TINYINT        | 1 byte                | 小整数值                        |
|                | SMALLINT       | 2 bytes               | 大整数值                        |
|                | MEDIUMINT      | 3 bytes               | 大整数值                        |
|                | INT 或 INTEGER | 4 bytes               | 大整数值                        |
|                | BIGINT         | 8 bytes               | 极大整数值                      |
|                | FLOAT          | 4 bytes               | 单精度浮点数值                  |
|                | DOUBLE         | 8 bytes               | 双精度浮点数值                  |
|                | DECIMAL        |                       | 小数值                          |
| 日期和时间类型   | DATE           | 3                     | 日期值                          |
|                | TIME           | 3                     | 时间值或持续时间                |
|                | YEAR           | 1                     | 年份值                          |
|                | DATETIME       | 8                     | 混合日期和时间值                |
|                | TIMESTAMP      | 4                     | 混合日期和时间值，时间戳        |
| 字符串类型       | CHAR           | 0-255 bytes           | 定长字符串                      |
|                | VARCHAR        | 0-65535 bytes         | 变长字符串                      |
|                | TINYBLOB       | 0-255 bytes           | 不超过 255 个字符的二进制字符串 |
|                | TINYTEXT       | 0-255 bytes           | 短文本字符串                    |
|                | BLOB           | 0-65535 bytes         | 二进制形式的长文本数据          |
|                | TEXT           | 0-65535 bytes         | 长文本数据                      |
|                | MEDIUMBLOB     | 0-16777215 bytes      | 二进制形式的中等长度文本数据    |
|                | MEDIUMTEXT     | 0-16777215 bytes      | 中等长度文本数据                |
|                | LONGBLOB       | 0-4294967295 bytes    | 二进制形式的极大文本数据        |
|                | LONGTEXT       | 0-4294967295 bytes    | 极大文本数据                    |

案例：
Score double (总长度, 小数点后保留的位数)
Name char (10)="张三"，占十个字符空间
Name varchar (10)="张三"，占两个字符空间

案例创建学生表格
```sql
create table student(
	id int,-- 编号
	name varchar(10),-- 姓名，最长不超过10个汉字
	gender char(1),-- 性别，最多一个汉字
	birthday date,-- 生日，取值为年月日
	score double(5,2),-- 入学成绩，小数点后保留两位
	email varchar(64),-- 邮件地址，最大长度不超过64
	tel varchar(15),-- 家庭联系电话，会出现-字符
	status tinyint-- 学生状态（用数字表示，正常、休学、毕业……）
);
```

## 查询Retrieve
- `show tables` ：查询当前数据库下所有表的名称
- `desc name` ：查询表的结构
## 修改Update
- `alter table 表名 rename to 新表名` ：修改表名
- `alter table 表名 add 列名 数据类型` ：添加一列
- `alter table 表名 modify 列名 新数据类型` ：修改某一列的数据类型
- `alter table 表名 change 列名 新列名 新数据类型` ：修改列名和数据类型
- `alter table 表名 drop 列名`
## 删除Delete
- `drop table name`
- `drop table if exists name`