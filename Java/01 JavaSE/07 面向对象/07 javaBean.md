实体类，其对象可以在程序中封装数据：
学生类、汽车类、用户类（在现实世界中存在）

标准javaBean须满足以下书写要求：
1. 成员变量必须使用private修饰
2. 提供成员变量的setXxx()和getXxx()方法。
3. 必须提供一个无参构造器；有参构造器是可写可不写的。

定义
```java
public class User {  
    private String name;  
    private double height;  
    private double salary;  
  
    public User() {  
    }  
  
    public User(String name, double height, double salary) {  
        this.name = name;  
        this.height = height;  
        this.salary = salary;  
    }  
  
    /**  
     * 必须为成员变量生成成套的setter和getter方法  
     */  
    public void setName(String name) {  
        this.name = name;  
    }  
  
    public void setHeight(double height) {  
        this.height = height;  
    }  
  
    public void setSalary(double salary) {  
        this.salary = salary;  
    }  
  
    public String getName() {  
        return name;  
    }  
  
    public double getHeight() {  
        return height;  
    }  
  
    public double getSalary() {  
        return salary;  
    }  
}
```

调用
```java
public static void main(String[] args) {  
    //通过无参构造器封装一个用户信息  
    User u1=new User();  
    u1.setName("张三");  
    u1.setHeight(174);  
    u1.setSalary(8000);  
    System.out.println(u1.getName());  
    System.out.println(u1.getHeight());  
    System.out.println(u1.getSalary());  
  
    //通过有参构造器封装一个用户信息  
    User u2 =new User("李四",175,9000);  
    System.out.println(u2.getName());  
    System.out.println(u2.getHeight());  
    System.out.println(u2.getSalary());  
}
```
