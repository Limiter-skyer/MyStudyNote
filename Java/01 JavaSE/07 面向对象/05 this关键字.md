# 概念
this关键字可以出现在构造器、方法中。
代表当前对象的地址。

验证
```java
public class Car {  
    public Car(){  
        System.out.println("无参构造器中的"+this);  
    }  
  
    public void run(){  
        System.out.println("方法中的"+this);  
    }  
}
```

```java
public class Test {  
    public static void main(String[] args) {  
        Car c =new Car();  
        c.run();  
        System.out.println(c);  
    }  
}
```


# 作用
指代当前对象（新建对象之后）
```java
public class Car{
	String name;
	double price;

	public Car(String name,double price){
		this.name=name;
		this.price=price;
	}
}
```

如果不使用this
比如
```java
public class Car{
	String name;
	double price;

	public Car(String name,double price){
		name=name;
		price=price;
	}
}
```
![[Pasted image 20220710093808.png]]
会造成赋值失败
![[Pasted image 20220710100118.png]]
