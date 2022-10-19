- 解决原生方式中的硬编码
- 简化代码

步骤：
1. 定义与SQL映射文件同名的UserMapper接口，并且将UserMapper接口和SQL映射文件放置在同一目录下
2. 设置SQL映射文件的namespace属性为Mapper接口的全限名
3. 在Mapper接口中定义方法，方法名就是SQL映射文件中sql语句的id，并保持参数类型和返回值类型一致
4. 编码
	1. 通过SqlSession的getMapper方法获取Mapper接口的代理对象
	2. 调用对应方法完成sql的执行

# 保持Mapper接口和SQL映射文件放置在同一目录下的方法
SQL映射文件即为写了SQL语句的xml文件。
在编译之前，xml文件都是统一放在resources文件夹中，Mapper接口文件都是放在java文件夹中，不可能在统一目录下。
但是在编译之后，java文件夹和resources文件夹是会合并的。因此，只需要在resources文件夹下，新建和UserMapper文件，相同结构的文件夹结构，就可以在编译后保证UserMapper文件和xml文件在同一目录下。
resources文件夹下，新建文件夹不可以使用小数点来代表结构，只能用斜杠，或者干脆一层层建文件夹。

# 设置namespace
在xml文件中，把namespace修改为Mapper接口的全限名，比如：com. obsidian. mapper. UserMapper。
修改UserMapper.xml文件位置后，必须把Mybatis-config. xml里的对应信息修改为全限名，右击UserMapper. xml文件，copy path复制路径，选择Path From Source Root。复制到mapper resource = "全限名"。

# 在接口中定义方法
在Mapper接口中定义一个方法 `List<User> selectAll();`，这个selectAll要与UserMapper. xml中的id相同

# 编码
重写执行sql语句的代码
用
```java
UserMapper userMapper = sqlSession.getMapper(UserMapper.class);  
List<User> users = userMapper.selectAll();
```
来代替
```java
List<User> users=sqlSession.selectList("test.selectAll");
```

整个流程就是，
1. 新建UserMapper对象，用对象来调用UserMapper接口中的方法 `selectAll()` `
2. UserMapper接口中 `selectAll()` 指向的是与接口同名的xml文件中，命名空间中的id
3. 执行id对应的sql语句


完成以上配置之后，可以回头把mybatis-config. xml中的加载映射文件的mapper，
从
```java
<!--加载sql映射文件-->  
<mapper resource="com/obsidian/mapper/UserMapper.xml"/>
```
改为 
```java
<!--Mapper代理方式-->  
<package name="com.obsidian.mapper"/>
```
