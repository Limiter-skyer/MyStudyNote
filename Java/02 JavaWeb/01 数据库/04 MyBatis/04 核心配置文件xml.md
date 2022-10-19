详见[官网](https://mybatis.org/mybatis-3/zh/configuration.html)

- environments：连接某个，或者多个数据库。
- typeAliases：别名，这是一种只用于xml配置文件，如UserMapper的缩写

任意全限名的别名
```java
<typeAliases>
  <typeAlias alias="Author" type="domain.blog.Author"/>
  <typeAlias alias="Blog" type="domain.blog.Blog"/>
  <typeAlias alias="Comment" type="domain.blog.Comment"/>
  <typeAlias alias="Post" type="domain.blog.Post"/>
  <typeAlias alias="Section" type="domain.blog.Section"/>
  <typeAlias alias="Tag" type="domain.blog.Tag"/>
</typeAliases>
```
包名的别名
```java
<typeAliases>
  <package name="domain.blog"/>
</typeAliases>
```

**书写时严格按照官网的顺序**
-   configuration（配置）
    -   [properties（属性）](https://mybatis.org/mybatis-3/zh/configuration.html#properties)
    -   [settings（设置）](https://mybatis.org/mybatis-3/zh/configuration.html#settings)
    -   [typeAliases（类型别名）](https://mybatis.org/mybatis-3/zh/configuration.html#typeAliases)
    -   [typeHandlers（类型处理器）](https://mybatis.org/mybatis-3/zh/configuration.html#typeHandlers)
    -   [objectFactory（对象工厂）](https://mybatis.org/mybatis-3/zh/configuration.html#objectFactory)
    -   [plugins（插件）](https://mybatis.org/mybatis-3/zh/configuration.html#plugins)
    -   [environments（环境配置）](https://mybatis.org/mybatis-3/zh/configuration.html#environments)
        -   environment（环境变量）
            -   transactionManager（事务管理器）
            -   dataSource（数据源）
    -   [databaseIdProvider（数据库厂商标识）](https://mybatis.org/mybatis-3/zh/configuration.html#databaseIdProvider)
    -   [mappers（映射器）](https://mybatis.org/mybatis-3/zh/configuration.html#mappers)