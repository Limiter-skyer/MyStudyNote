Map集合是双列集合，每个元素包含两个数据。key=value(键值对元素)。
Map集合也被称为“键值对集合”。左为键，右为值。

# Map集合使用场景一：购物车系统
- 购物车提供的四个商品和购买数量在后台需要容器存储。
- 每个商品对象都一一对应一个购买数量。
- 把商品对象看成是Map集合的键，购买数量是Map集合的值
- {商品1=1，商品2=1，商品3=2，商品4=1}

# 特点：Map是接口
- Map
	- HashMap
		- LinkedHashMap
	- HashTable
		- Properties
	- ……
		- TreeMap

- Map集合的键，是无序、不重复、无索引的。值不做要求。
- Map集合后面重复的键对应的值会覆盖前面重复键的值。
- Map集合的键值对都可以为null

实现类：
- HashMap：无序、不重复
- LinkedMap：有序、不重复
- TreeMap：排序、不重复

# 常用API
![[Pasted image 20220719210203.png]]

Map集合与Set集合是类似的，去掉值就是Set集合
`public Set<K> keySet()`
`public Collection<K> values()`

# 遍历
如果直接去遍历Map集合的话，会发现Key=Value的形式不是任何数据类型，代码没办法写。
```java
//创建一个Map集合对象  
Map<String,Integer> maps=new HashMap<>();  
maps.put("螺旋丸",78);  
maps.put("千鸟",76);  
maps.put("螺旋手里剑",86);  
maps.put("天照",85);  
maps.put("超大玉螺旋手里剑",98);  
maps.put("须佐能乎加具土命",99);  
maps.put("光轮疾风漆黑矢零式",132);  
```
## 遍历方式一：键找值
1. 把键提取成一个集合，
2. 遍历键集合，通过键找值。

- `Set<K> keySet()`
- `V get(Object key)`

```java
//方式一  
Set<String> keys=maps.keySet();  
for (String key : keys) {  
    int value = maps.get(key);  
    System.out.println(key+"-----------"+value);  
}
```
## 遍历方式二：键值对
1. 把Map集合转换为Set集合
2. 遍历Set集合，找键找值

- `Set<Map.Entry<K,V>>entrySet()`
- `K getKey()`
- `V getValue()`

```java 
// 方式二，Set集合的类型为Map.Entry，Map.Entry包装了键值  
Set<Map.Entry<String,Integer>> entries = maps.entrySet();  
for (Map.Entry<String, Integer> entry : entries) {  
    String key=entry.getKey();  
    int value=entry.getValue();  
    System.out.println(key+"----"+value);  
}
```

## 遍历方式三：lambda
`default void forEach(BiConsumer<? suoer K, ? super V>action)`

```java
maps.forEach(new BiConsumer<String, Integer>() {  
    @Override  
    public void accept(String s, Integer integer) {  
        System.out.println(s+"-----------"+integer);  
    }  
});
```
简化：
`maps.forEach((k,v)->{System.out.println(k+"-----------"+v);});`

# 案例：统计投票人数
需求：某个班80个学生，一共四个景点，每个学生选一个。
分析：输入80个学生的数据，定义集合用于存储结果。
```java
//把80个数据输入，实际用随机生成  
String[] selects={"A","B","C","D"};  
StringBuilder sb = new StringBuilder();  
Random r = new Random();  
for (int i = 0; i < 80; i++) {  
    sb.append(selects[r.nextInt(selects.length)]);  
}  
System.out.println(sb);  
  
//定义一个Map集合存储结果  
Map<Character,Integer> infos = new HashMap<>();  
//遍历80个学生的数据  
for (int i = 0; i < sb.length(); i++) {  
    //提取当前字符  
    char ch = sb.charAt(i);  
    //判断集合中存不存在这个键  
    if (infos.containsKey(ch)){  
        //值加一  
        infos.put(ch,infos.get(ch)+1);  
    }else {  
        infos.put(ch,1);  
    }  
}  
System.out.println(infos);
```

# HashMap
- HashMap是直接体现Map特性的实现类，没有额外需要学习的方法，直接用Map的方法。
- HashMap和HashSet底层原理是Map实现的，都是哈希表结构，只是HashMap的每个元素包含两个值
- 实际上，Set集合的底层就是Map实现的，只是Set集合中的元素只要键入数据，不需要值数据。
- 通过HashCode和equals方法保证键唯一，自定义的类型要重写HashCode和equals
# LinkedHashMap
- HashMap的子类，多了有序的添加顺序这一优点。底层是哈希表，额外多了双链来记录位置信息。
# TreeMap
- 可排序，按照键的大小默认排序
- 自定义类型要重写比较规则
- TreeMap与TreeSet底层一样