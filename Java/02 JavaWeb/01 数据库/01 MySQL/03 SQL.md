# 简介
SQL：Structured Query Language
可以操作所有的关系型数据库
对于同一种需求，不同的数据库操作的方式可能会存在些不一样的地方，称为“方言”。

# 通用语法
1. 可以单行多行书写，以分号结尾，见不到分号视为同一行
2. SQL不区分大小写
3. 单行注释 `-- ` （两横杠+一空格）或者 `#` (MySQL特有)
4. 多行注释 `/* */` 

# SQL分类
1. DDL（Data Definition Language）数据定义语言：定义数据库对象 ^0ff6dc
2. DML（Data Manipulation Language）数据操作语言：对数据库中表的数据进行增删改 ^0bc0f7
3. DQL（Data Query Language）数据查询语言：用来查询数据库中表的数据 ^cb1680
4. DCL（Data Control Language）数据控制语言：用来定义数据库的访问权限和安全级别，及创建用户
