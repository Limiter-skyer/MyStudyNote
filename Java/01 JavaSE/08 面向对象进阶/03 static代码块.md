代码块是类的五大成分之一(成员变量，构造器，方法，代码块，内部类)
在Java类下，使用{}括起来的代码被称为代码块。


# 静态代码块
格式：static{}
特点：需要通过static关键字修饰，随着类的加载而加载，并且自动触发，只执行一次
使用场景：在类加载的时候做一些静态数据初始化的操作，以便以后使用。
```java
public class StaticDemo {  
  
    public static String name;  
  
    static {  
        //static代码块随着类的加载被执行  
        //用于初始化的静态资源  
        name="张三";  
        System.out.println("-----静态代码块被触发执行了-----");  
    }  
  
    public static void main(String[] args) {  
        System.out.println("后加载");  
        System.out.println(name);  
        StaticDemo2 s1=new StaticDemo2();  
    }  
  
}
```


# 构造代码块（所见甚少）
格式：()
特点：每次创建对象，调用构造器执行时，都会执行该代码块中的代码，并且在构造器执行前执行
使用场景：初始化实例资源

```java
public class StaticDemo2 {  
    /**  
     * 实例代码块  
     */  
    {  
        System.out.println("========实例代码块被触发执行==========");  
    }  
  
    public StaticDemo2() {  
        System.out.println("==========构造器被执行========");  
    }  
}
```

# 静态代码块案例

斗地主游戏：在启动游戏房间的时候，应该提前准备54张牌，后续才可以使用这些数据。

1. 该房间只需要一副牌
2. 定义一个静态ArrayList集合储存54张牌对象，静态的集合只会加载一份
3. 在启动游戏房间前，应该将54张牌初始化好
4. 当系统启动的同时需要准备54张牌数据，此时可以用静态代码块进行完成。

好处：静态代码块用于初始化资源，方法会调用很多次，随着类只调用一次的代码，用静态代码块来写
```java
public class StaticTest3 {  
    /**  
     * 模拟游戏启动前，初始化54张牌数据  
     */  
    public static ArrayList<String> cards=new ArrayList<>();  
  
    /**  
     *程序真正运行main方法前，把54张牌放放进去，后续可以直接使用  
     */  
    static {  
        //正式做牌  
        String[] sizes ={"A","2","3","4","5","6","7","8","9","10","J","Q","K"};  
        String[] colors={"♠","♥","♣","♦"};  
        //遍历点数  
        for (int i = 0; i < sizes.length; i++) {  
              
            //遍历花色  
            for (int j = 0; j < colors.length; j++) {  
                String card=colors[j]+ sizes[i];  
                cards.add(card);  
            }  
        }  
        //加入大小王  
        cards.add("小🃏");  
        cards.add("大🃏");  
  
    }  
  
    public static void main(String[] args) {  
        System.out.println(cards);  
    }  
}
```

特点：只创建一个，获取前就已经创建好了