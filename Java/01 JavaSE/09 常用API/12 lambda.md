# 概述
简化匿名内部类
必须是接口的匿名类，接口中只能由一个抽象方法。

接口：
```java
@FunctionalInterface  
interface Swimming{  
    void swim();  
}
```

简化：
```java
    Swimming s1 = new Swimming() {  
        @Override  
        public void swim() {  
            System.out.println("1111");  
        }  
    };  
    go(s1);  
    //简化  
    Swimming s2 = ()->{  
        System.out.println("222");  
    };  
    go(s2);  
  
    //再简化  
    go(()->{  
        System.out.println("333");  
    });  
}  
  
public static void go(Swimming s){  
    System.out.println("开始");  
    s.swim();  
    System.out.println("结束");  
}
```

