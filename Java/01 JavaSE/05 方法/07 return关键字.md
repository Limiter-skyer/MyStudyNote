return可以单独使用
`return;`可以立刻跳出并结束当前方法的执行。可以放在任何方法中。

```java
public static void main(String[] args) {  
    int data1=33;  
    int data2=0;  
    divide(data1,data2);  
}  
  
public static void divide(int a,int b){  
    if(b==0){  
        System.out.println("除数不可为0");  
        return;  
    }  
    int c=a/b;  
    System.out.println(c);  
}
```

类似的有break；结束循环。