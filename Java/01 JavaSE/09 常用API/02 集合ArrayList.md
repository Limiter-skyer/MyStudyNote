# 概述
集合与数组类似，也是一种容器。
数组的特点：数组定义完成并启动后，类型确定、长度固定。
问题：当个数不确定，且需要增删操作时，数组不合适

集合的特点：集合的大小不固定，启动后可以动态变化，类型也可以选择不固定。
集合非常时候做元素个数不确定，且要增删改查的业务场景。
集合提供了很多丰富、好用的功能，而数组的功能很单一。

# ArrayList
ArrayList是集合的一种

创建对象：public ArrayList()

ArrayList集合添加元素的方法
public boolean add(E e)：将指定的元素追加到此集合的末尾
public void add(int index,E element)：在此集合中的指定位置插入指定元素

```java
//新建对象  
ArrayList list = new ArrayList();  
//添加各种类型的元素  
list.add("java");  
list.add("Java");  
list.add("MySQL");  
list.add(23);  
list.add(23.5);  
list.add(false);  
list.add('中');  
  
//输出，与数组直接输出会输出地址不同，集合会输出元素  
System.out.println(list);  
  
//指定索引位置插入元素，该位置原本的元素会向后挤压一个位置  
list.add(1,"贾宝玉");  
System.out.println(list);
```

# ArrayList对于泛型的支持
`ArrayList<E>`：泛型类，可以在编译阶段约束集合对象只能操作某种数据类型
`比如ArrayList<String>`，`ArrayList<Integer>。
集合中只能存储引用类型，不能储存基本类型。
```java
public static void main(String[] args) {  
    ArrayList<String> list =new ArrayList<String>();  
    list.add("张三");  
    list.add("李四");  
    list.add("王五");  
    list.add("21");  
  
    ArrayList<Integer> list2 =new ArrayList<>();  
    list2.add(21);  
    list2.add(21);  
  
}
```
最好定义集合都要定义泛型

# 常用API
## E get()
获取索引出的元素
```java
//定义集合  
ArrayList<String> list = new ArrayList<>();  
list.add("南京");  
list.add("镇江");  
list.add("常州");  
list.add("无锡");  
list.add("苏州");  
list.add("扬州");  
list.add("泰州");  
list.add("南通");  
list.add("淮安");  
list.add("宿迁");  
list.add("盐城");  
list.add("徐州");  
list.add("连云港");
//public E get(int index);获取某个索引位置处的索引值  
String e = list.get(3);  
System.out.println(e);
```
## size()
获取集合的大小（元素个数）
```java
//public int size();获取集合的大小（元素个数）  
System.out.println(list.size());  
//集合的遍历  
for (int i = 0; i < list.size(); i++) {  
    System.out.print(list.get(i));  
    System.out.print("\t");  
}
```
## remove()
删除元素
```java
//public E remove(int index);删除某个索引位置处的元素，并返回被删除的元素值  
System.out.println(list);  
String d=list.remove(0);  
System.out.println(d);  
System.out.println(list);  
//public boolean remove(Object o);直接删除元素值，删除成功并返回ture，删除失败返回fales  
list.remove("镇江");//如果有两个重复元素只会删第一个  
System.out.println(list);
```
## set()
删除索引处的元素值
```java
//public E set(int index, E element);修改某个索引位置处的元素值  
list.set(0,"上海");
```

# 案例
## 遍历并删除元素
需求：某个班级的考试在系统上进行，成绩大致为：98,77,66,89,79,50,100
现在要把成绩低于80分的数据都去掉

分析：定义ArrayList集合储存多名学员的成绩。
遍历集合每个元素，如果元素值低于80分，去掉它。
```java
//创建一个集合储存一个班级的成绩  
ArrayList<Integer> scores =new ArrayList<>();  
scores.add(98);  
scores.add(77);  
scores.add(66);  
scores.add(89);  
scores.add(79);  
scores.add(50);  
scores.add(100);  
System.out.println(scores);  
  
for (int i = scores.size()-1; i >= 0; i--) {  
    int score = scores.get(i);  
    if(score<80){  
        scores.remove(i);  
    }  
}  
System.out.println(scores);
```

遍历删除要倒着删，否则会错位

## 影片信息在程序中的表示
需求：某影院系统需要在后台储存上述三部电影，然后依次展示出来
分析
1. 定义一个电影类，定义一个集合储存电影对象。
2. 创建3个电影对象，封装相关数据，把3个对象存入到集合中去。
3. 遍历集合3个对象，输出相关信息。

定义类
```java
public class Movie {  
    private String name;  
    private double score;  
    private String actor;  
  
    public Movie() {  
    }  
  
    public Movie(String name, double score, String actor) {  
        this.name = name;  
        this.score = score;  
        this.actor = actor;  
    }  
  
    public String getName() {  
        return name;  
    }  
  
    public void setName(String name) {  
        this.name = name;  
    }  
  
    public double getScore() {  
        return score;  
    }  
  
    public void setScore(double score) {  
        this.score = score;  
    }  
  
    public String getActor() {  
        return actor;  
    }  
  
    public void setActor(String actor) {  
        this.actor = actor;  
    }  
}
```

调用
```java
//创建三个电影对象  
Movie m1=new Movie("《肖申克的救赎》",9.7,"罗宾斯");  
Movie m2=new Movie("《霸王别姬》",9.7,"张国荣");  
Movie m3=new Movie("《阿甘正传》",9.5,"汤姆·汉克斯");  
//创建一个集合，储存三部电影对象  
ArrayList<Movie> list = new ArrayList<>();  
list.add(m1);  
list.add(m2);  
list.add(m3);  
System.out.println(list);  
//遍历  
for (int i = 0; i < list.size(); i++) {  
    Movie m =list.get(i);  
    System.out.println("电影名称："+m.getName());  
    System.out.println("电影评分："+m.getScore());  
    System.out.println("电影演员："+m.getActor());  
    System.out.println("----------------------");  
  
}
```

![[Pasted image 20220714195327.png]]
## 学生信息系统的数据搜索
需求：后台程序需要存储如上学生信息并展示，然后要提供按照学号搜索学生信息的功能。
分析：
1. 定义Student类，定义ArrayList集合存储加上学生对象信息，并遍历展示出来。
2. 提供一个方法，可以接收ArrayList集合，和要搜索的学号，返回搜索的学生对象信息，并展示。
3. 使用死循环，让用户可以不停得搜索。

