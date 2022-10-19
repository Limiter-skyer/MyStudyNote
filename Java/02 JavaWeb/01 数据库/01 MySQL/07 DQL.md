![[03 SQL#^cb1680|概述]]
查询是最为常用，最为复杂的

- select 字段列表
- from 表名列表：单表多表查询
- where 条件列表
- group by 分组字段
- having 分组后条件
- order by 排序字段
- limit 分页限定

# 基础查询
`select 列1,列2 from 表名;` ：查询某些列
`select * from 表名` ：查询全部
`select distinct 列名 from 表名` ：查询结果去除重复
`SELECT math AS 数学,english AS 英语 FROM stu;` ：该表名展示
As可以不写

# 条件查询
where+条件列表
条件
```sql
-- 查询年龄大于20岁
SELECT * FROM stu WHERE age>20;

-- 查询年龄大于等于20岁
SELECT * FROM stu WHERE age>=20;

-- 查询年龄大于等于20岁，并小于等于30岁
SELECT * FROM stu WHERE age>=20 && age<=30;
SELECT * FROM stu WHERE age>=20 AND age<=30;
SELECT * FROM stu WHERE age BETWEEN 20 and 30;

-- 查询入学日期在1998-09-01到1999-09-01之间的学员
SELECT * FROM stu WHERE hire_date BETWEEN '1998-09-01' and '1999-09-01';

-- 查询年龄等于18岁的学院
SELECT * FROM stu WHERE age=18;

-- 查询年龄不等于18岁的学员
SELECT * FROM stu WHERE age!=18;

-- 查询年龄等于18岁，或者年龄等于20，年龄等于22
SELECT * FROM stu WHERE age=18 or age=20 or age=22;

-- 查询英语成绩为null的学员
-- null值的比较不能用“=”，要用is,is not
SELECT * FROM stu WHERE english is null;
SELECT * FROM stu WHERE english is not null;
```

模糊查询
```sql
-- 模糊查询like
/*
通配符：
（1）_：代表单个任意字符
（2）%：代表任意个数字符
*/

-- 查询姓'马'的学员信息
SELECT * FROM stu WHERE name LIKE '马%';

-- 查询第二个字是'花'的学员信息
SELECT * FROM stu WHERE name LIKE '_花%';

-- 查询名字中包含'德'的学员信息
SELECT * FROM stu WHERE name LIKE '%德%';
```

# 排序查询
ASC：升序，默认可以不写
DESC：降序
案例：
```sql
-- 查询学生信息，按照年龄升序排列
SELECT * FROM stu ORDER BY age;
-- 查询学生信息，按照数学成绩降序排列
SELECT * FROM stu ORDER BY math DESC;
-- 查询学生信息，按照数学成绩降序排列，如果数学成绩一样，再按照英语成绩降序排列
SELECT * FROM stu ORDER BY math DESC, english DESC;
```

# 聚合函数（null不参与）
- count ()
- max ()
- min ()
- sum ()
- avg ()
select 聚合函数名 (列名) From 表;
```sql
-- 分组查询
-- 统计班级一共有多少个学生
SELECT COUNT(*) FROM stu;
-- 查询数学成绩的最高分
SELECT MAX(math) FROM stu;
-- 查询数学成绩的最低分
SELECT MIN(math) FROM stu;
-- 查询数学成绩的平均分；
SELECT AVG(math) FROM stu;
```

# 分组查询
将一列作为一个整体，进行纵向计算
```sql
-- 分组查询
-- 查询男同学和女同学各自的数学平均分
SELECT sex,avg(math) from stu GROUP BY sex ;
-- 查询男同学和女同学各自的数学平均分，以及各自人数
SELECT sex,avg(math),COUNT(sex) from stu GROUP BY sex ;
-- 查询男同学和女同学各自的数学平均分，以及各自人数,要求低于70分不参与分组
SELECT sex,avg(math),COUNT(sex) from stu WHERE math>=70 GROUP BY sex ;
-- 查询男同学和女同学各自的数学平均分，以及各自人数,要求低于70分不参与分组，参与统计的人数不能小于3
SELECT sex,avg(math),COUNT(sex) from stu WHERE math>=70 GROUP BY sex HAVING COUNT(*)>2;
```

# where和having的区别
- 执行时机不一样，where是分组前进行限定，不满足条件不参与分组，而having是分组后对结果进行过滤。
- 可执行条件不一样，where不能对聚合函数进行判断，而having可以。
- 执行顺序where>聚合函数>having

# limit
数据过多不可能一次性展示出来，对内存消耗极大，需要对展示进行分页
SELECT 字段列表 FROM 表名 LIMIT 起始索引,查询条目数;
起始索引从0开始。
```sql
-- 分页查询
-- 从0开始查询，查询3条数据
SELECT * FROM stu LIMIT 0,3;
-- 每页现实3条数据，查询第1页。
SELECT * FROM stu LIMIT 0,3;
-- 每页现实3条数据，查询第2页。
SELECT * FROM stu LIMIT 3,3;
-- 每页现实3条数据，查询第3页。
SELECT * FROM stu LIMIT 6,3;
```
注：
- limit是MySQL的方言
- Oracle使用rownumber
- SQL Server使用top