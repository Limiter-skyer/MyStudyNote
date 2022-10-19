- DriverManager
- Connection
- Statement
- ResultSet
- PrepareStatement

# DriverManager
## 注册驱动

`Class. forName ("com. mysql. jdbc. Driver");`
MySQL5之后的驱动包，这一步可以注释掉
加载驱动的代码写在静态代码块里，随着类的加载自动运行。

## 获取数据库连接
url: 连接路径
```sql
String url="jdbc:mysql://127.0.0.1:3306/db1";
```
语法：jdbc:mysql://ip地址(域名):端口号/数据库名称?参数键值对1&参数键值对2…

示例：jdbc:mysql://127.0.0.1:3306/db1
如果连接的是本机mysql服务器，并且mysql服务默认端口是3306，则url可以简写为：jdbc:mysql:///数据库名称?参数键值对
配置 useSSL=false 参数，禁用安全连接方式，解决警告提示

# Connection
## 获取，执行SQL对象
```sql
Statement createStatement ()
PreparedStatement prepareStatement (sql)
CallableStatement prepareCall (sql)
``` 

## 管理事务
![[11 事务#^7a6fc4|MySQL事务]]

```java
对象.setAutoCommit(false);//开启事务
对象.commit();//提交事务
对象.rollback();//回滚
```

```java
try {  
    conn.setAutoCommit(false);  
    int count1=stmt.executeUpdate(sql1);  
    System.out.println(count1);  
  
    int count2=stmt.executeUpdate(sql2);  
    System.out.println(count2);  
    //提交事务  
    conn.commit();  
} catch (Exception e) {  
    //回滚  
    conn.rollback();  
    e.printStackTrace();  
}
```

# Statement
执行SQL语句
- executeUpdate ()：执行DML、DDL语句，返回受影响的行数
- executeQuery ()：执行DQL语句，返回结果集对象

```java
Statement stmt =conn.createStatement();
int count1=stmt.executeUpdate(sql1);
int count2=stmt.executeUpdate(sql2);
```

# ResultSet
ResultSet执行会返回结果集对象，封装了DQL查询语句的结果
对象中有一个游标（指针），指向某一行数据，next () 方法可以操控它。
getXxx ()：获取数据如getInt (1)，getInt ("id")，索引从1开始。
一般用while (对象. next ()){}来循环遍历，遍历完next () 会返回false结束循环
```java
//注册驱动  
Class.forName("com.mysql.jdbc.Driver");  
  
//获取连接  
String url="jdbc:mysql://127.0.0.1:3306/db1";  
String username="root";  
String password="1234";  
Connection conn = DriverManager.getConnection(url,username,password);  
  
//定义sql  
String sql = "select * from stu";  
  
//执行sql  
Statement stmt = conn.createStatement();  
ResultSet rs=stmt.executeQuery(sql);  
  
//处理结果  
while (rs.next()){  
    int id = rs.getInt(1);  
    String name = rs.getString(2);  
    int age = rs.getInt(3);  
    String sex = rs.getString(4);  
    String address = rs.getString(5);  
    String math = rs.getString(6);  
    String english = rs.getString(7);  
    String hireDate = rs.getString(8);  
  
    System.out.println(id+"\t"+name+"\t"+age+"\t"  
            +sex+"\t"+address+"\t"+math+"\t"+  
            english+"\t"+hireDate);  
}  
  
//释放内存  
rs.close();  
stmt.close();  
conn.close();
```

案例：创建一个用户类，存入ArrayList集合

创建类：
```java
public class Account {  
    private int id;  
    private String name;  
    private int age ;  
    private String sex ;  
    private String address ;  
    private String math  ;  
    private String english ;  
    private String hireDate ;  
  
    public Account() {  
    }  
  
    public Account(int id, String name, int age, String sex, String address, String math, String english, String hireDate) {  
        this.id = id;  
        this.name = name;  
        this.age = age;  
        this.sex = sex;  
        this.address = address;  
        this.math = math;  
        this.english = english;  
        this.hireDate = hireDate;  
    }  
//getter和setter和toString省略
}
```

执行（部分，相同代码省略）
```java
ArrayList<Account> accs = new ArrayList<>();  
while (rs.next()){  
    int id = rs.getInt(1);  
    String name = rs.getString(2);  
    int age = rs.getInt(3);  
    String sex = rs.getString(4);  
    String address = rs.getString(5);  
    String math = rs.getString(6);  
    String english = rs.getString(7);  
    String hireDate = rs.getString(8);  
  
    Account acc =new Account(id,name,age,sex,  
            address,math,english,hireDate);  
    accs.add(acc);  
}  
  
System.out.println(accs);
```

# PreparedStatement
1. 预编译SQL语句并执行：预防SQL注入问题
2. SQL注入是通过操作输入来修改事先定义好的SQL语句，用以达到执行代码对服务器进行攻击的方法。
3. 一般鉴于账户密码攻击的操作

在sql语句中用“？”来代替账号和密码，再分别对这些“？”赋值

```java
String uname = "asdf";  
String psw = "'or'1'='1";  
//定义sql  
String sql = "select * from tb_user where username = ? and password = ?";  
  
//定义对象  
PreparedStatement pstmt= conn.prepareStatement(sql);  
  
//设置?的值  
pstmt.setString(1,uname);  
pstmt.setString(2,psw);  
//执行sql  
  
ResultSet rs=pstmt.executeQuery(sql);
```
相同代码省略

原理：
- 将敏感字符进行转义
- 正常是直接把sql语句传到MySQL，进行检查，编译，运行。
- 预编译，是把带"?"的sql语句编译成一个可执行函数，等待传入形参就可以直接运行。

预编译功能开启：在url后面加 `useServerPrepStmts=ture`