设计模式：问题有n种解法，但其中有最优解，这个最优解法，被人总结出来，称为设计模式
设计模式有20多种，对应20多种软件开发中会遇到的问题
学设计模式主要是学2点
第一这种设计模式是解决什么问题的
第二遇到这种问题，该模式是怎么写的问题是怎么解决的。

# 单例模式
可以保证系统中，应用该模式的这个类永远只有一个实例（对象）
例如任务管理器，只需要一个对象就可以解决问题，这样可以节省空间。

## 饿汉单例
在类获取对象之前，对象已经为你提前创建好了

设计步骤：
1. 定义一个类，把构造器私有
2. 定义一个静态变量储存一个对象
![[Pasted image 20220711103652.png]]

定义
```java
//单例类，对外永远只有一个对象  
public class SingleInstance {  
    /**  
     * 构造器私有化，把构造器藏起来，拒绝外部创建对象  
     */  
    private SingleInstance() {  
    }  
  
    /**  
     * 饿汉单例是指获取对象前，对象已经创建好了  
     */  
  
    public static SingleInstance instance =new SingleInstance();  
}
```


验证
```java
public static void main(String[] args) {  
    //验证单例类：多次获取的对象是否是同一个（唯一一个）  
    SingleInstance s1=SingleInstance.instance;  
    SingleInstance s2=SingleInstance.instance;  
    System.out.println(s2==s1);  
}
```

## 懒汉单例
真正需要该对象的时候，才去创建一个对象（延迟加载对象）

设计步骤：
定义一个类，把构造器私有化
定义一个静态变量储存一个对象
提供一个返回单例对象的方法

![[Pasted image 20220711105012.png]]

定义
```java
public class SingleInstance2 {  
    /**  
     * 构造器私有化，把构造器藏起来，拒绝外部创建对象  
     */  
    private SingleInstance2() {  
    }  
  
    /**  
     * 懒汉单例是指，等到需要对象的时候，再新建对象。  
     * 私有化，避免别人调用SingleInstance2 s1=SingleInstance2.instance来新建空对象  
     */  
  
    private static SingleInstance2 instance ;  
  
    /**  
     * 提供方法对外返回单例对象  
     */  
    public static SingleInstance2 getInstance(){  
        if (instance==null){  
            //第二次调用的时候，不创建新对象  
            instance= new SingleInstance2();  
        }  
        return instance;  
    }  
  
}
```
