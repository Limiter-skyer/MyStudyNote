# static修饰成员变量
static可以修饰成员变量和成员方法
该成员变量再内存中只储存一份，可以被共享访问、修改。
指一个类的所有对象共享一个属性。
因为只有一份，所以一般认为是类持有，而不是对象持有，虽然对象.static也可以，但是还是推荐类.static。

定义：
```java
public class User {  
    /**  
     * 在线人数  
     */  
    public static int onLineNumber=161;  
}
```

调用
```java
System.out.println(User.onLineNumber);  
  
User u =new User();  
u.onLineNumber++;  
System.out.println("加了一个人");  
System.out.println(User.onLineNumber);
```

# 内存机制
类加载到方法区的同时，静态成员变量就加载到堆内存之中。
普通成员变量是需要对象新建的时候，才会加载到堆内存中去。而且每个对象都有各自的成员变量。
实际上对象的静态成员变量根本不是对象的属性，静态成员变量随类而生，所有对象共有。
![[Pasted image 20220710163651.png]]

# static修饰成员方法
成员方法有用static修饰，也有不需要static修饰的。
静态成员方法（static）用类名访问。
实例成员方法用对象名访问。

static修饰的静态成员方法用类和对象都能访问。
同一个类中访问方法，类名可以不写。
究竟用不用static，取决于方法涉不涉及对象的属性。如果方法是不涉及对象的公用方法，那么则可以使用static。

# 内存机制
![[Pasted image 20220710213025.png]]

# 注意事项
1. 静态方法只能访问静态的成员，不可以直接访问实例成员。（可以创建对象访问）
2. 实例方法可以访问静态的成员，也可以访问实例成员。
3. 静态方法中是不可以出现this关键字的

```java
public class Student {  
    //实例成员变量  
     private String name;  
    //静态成员方法:归属于类  
    public static int getMax(int age1,int age2){  
        return age1 > age2 ? age1 : age2;  
    }  
    //实例方法  
    public void study(){  
        System.out.println(name+"正在学习");  
        System.out.println(this);  
    }  
    public static void main(String[] args) {  
        //name="谢志青";报错  
        //study;报错  
        Student s = new Student();  
        s.name="谢志青";  
        s.study();  
        }  
}
```
