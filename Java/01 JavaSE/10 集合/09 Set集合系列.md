- 无序：存取顺序不一样，但不会改变
- 不重复
- 无索引：没有索引方法

实现类：
- HashSet：无需、不重复、无索引
- LinkedHashSet：**有序**、不重复、无索引
- TreeSet：**排序**、不重复、无索引

Set集合的API与Collection大致一致。

# HashSet底层原理
Hash表是一种对于增删改查都较高效的结构。
底层是数组+链表+红黑树。

- 哈希值：是JDK、根据对象的地址，按照某种规则算出来的int型数值
- public int hashCode();返回对象的哈希值
- 同一个对象多次调用hashCode方法返回的哈希值是相同的
- 默认情况下，不同对象的哈希值是不同的。

HashSet的排序：
1. 算出每个元素的哈希值
2. 新建一个长16的数组，数组名table
3. 把元素的哈希值对16取余，结果是多少，就放进对应数组的位置
4. 如果该位置已经有元素了，那么会先判断元素是否重复，不重复新旧元素会构成链表
5. JDK8之后，链表长度超过8之后会自动转换成红黑树。
6. 当数组存满到12时，会自动扩容至两倍。

去重复：重写hashCode()，和equals()。直接右击重写。
```java
@Override  
public boolean equals(Object o) {  
    if (this == o) return true;  
    if (o == null || getClass() != o.getClass()) return false;  
    Student student = (Student) o;  
    return age == student.age && Objects.equals(name, student.name) && Objects.equals(sex, student.sex);  
}  
  
@Override  
public int hashCode() {  
    return Objects.hash(name, age, sex);  
}  
  
@Override  
public String toString() {  
    return "Student{" +  
            "name='" + name + '\'' +  
            ", age=" + age +  
            ", sex='" + sex + '\'' +  
            '}';  
}
```

# LinkedHashSet
HashSet的子类，有序、不重复、无索引
原理：底层数据结构依然是哈希表，只是每个元素又额外多了一个双链表的机制记录储存的顺序。

# TreeSet
- 对于数值类型：Interger，Double，官方按照大小进行升序排列
- 对于字符串类型：默认按照首字符的编号进行升序排序
- 对于自定义类型，TreeSet无法进行直接排序，需要自定义排序规则

自定义排序规则
1. 让自定义的类，实现Comparable接口重写里面的CompareTo方法来定制比较规则。
2. TreeSet集合又参数构造器，可以设置Comparator接口对应的比较器对象，来定制比较规则。

返回值：
1. 如果认为第一个元素大于第二个元素返回正整数即可。
2. 如果认为第二个元素大于第一个元素返回负整数即可。
3. 如果第一个元素与第二个元素相等，返回0即可，此时TreeSet集合只会保留一个元素，认为两者重复。

```java
Set<Student> set = new TreeSet<>(new Comparator<Student>() {  
    @Override  
    public int compare(Student o1, Student o2) {  
        return o2.getAge()-o1.getAge();  
        //Double型建议直接使用return Double.compare(o1,o2);  
    }  
});
```