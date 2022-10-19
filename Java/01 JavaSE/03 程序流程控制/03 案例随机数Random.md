用于在程序中生成随机数。
1. 导包
2. 新建一个随机数对象
3. 调用nextInt方法，返回一个随机数

# 案例1
需求：生成10个0~9之间的随机数。
```java
Random r = new Random();  
for (int i = 0; i < 10; i++) {  
    int data = r.nextInt(10);  
    System.out.println(data);  
}
```

# 案例2
计算机只能生成0~max之间的随机数，如果需要区间，比如20~50，该如何操作。
（20~50）=(0~30)+20。
可以通过把左边界减至0来输出随机数，再把输出的数加上等量的数值。
需求：生成10个20~50之间的数
```java
Random r = new Random();  
for (int i = 0; i < 10; i++) {  
    int data=r.nextInt(30)+20;  
    System.out.println(data);  
}
```

# 案例3
猜数据游戏
需求：
1. 随机生成一个1~100之间的数据
2. 使用死循环让用户不断提示用户猜测，猜大提示过大，猜小提示过小，猜中结束。

```java
Random generate =new Random();  
Scanner sc = new Scanner(System.in);  
int rightNumber = generate.nextInt(100)+1;  
int number;  
int i=1;  
    System.out.println("请猜测并输入一个1~100的整数，看看你猜几次能猜对：");  
do{  
    number=sc.nextInt();  
    if(number>100||number<1){  
        System.out.println("这个数字超出了1~100，再猜一个在范围内的整数吧：");  
    } else if (number<rightNumber) {  
        System.out.println("这个数字猜小了，再猜一个大一点的整数吧");  
    } else if (number>rightNumber) {  
        System.out.println("这个数字猜大了，再猜一个小一点的整数吧");  
    }else {  
        System.out.println("恭喜你猜对了，这个数字就是"+rightNumber+"。这次猜了"+i+"次");  
        break;  
    }  
    i++;  
}while (true);
```
