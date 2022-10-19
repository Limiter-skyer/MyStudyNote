# List集合特点
1. 有序：存储和取出的元素顺序一致
2. 有索引：可以通过索引操作元素
3. 可重复：储存的元素可以重复

# List的方法
List集合因为支持索引，所以多了很多索引操作的独特API，其他Collection的功能，List也都继承。
```java
//创建一个ArrayList对象，多态  
List<String> list = new ArrayList<>();  
list.add("李白");  
list.add("李白");  
list.add("杜甫");  
list.add("白居易");  
System.out.println(list);  
  
//在某个位置插入元素  
list.add(2,"王勃");  
System.out.println(list.remove(2));  
  
//根据索引获取元素  
list.get(3);  
  
//修改索引出的元素值  
list.set(1,"李太白");  
System.out.printf("", list);  
  
//集合的方法也适用  
list.clear();  
System.out.println(list);
```

# 底层原理
- ArrayList底层是基于数组实现的，根据索引查询元素快，增删相对慢。
- LinkedList底层基于双链表实现的，查询元素慢，增删首位元素非常快。