# 工具类
企业一般会有一些工具类，方法以完成一个共有功能为目的，这个类用来给系统开发人员共同使用的。

案例：再企业的管理系统中，通常需要一个系统的很多业务处使用验证码进行防刷新等安全控制。

同一个功能多处开发会出现代码重复度过高。
使用工具类的好处：一是方便调用，而是提高了代码复用（一次编写，处处可用）

为什么用静态方法（类方法），而不使用实例方法（对象方法）？
答：这只是为了调用方法，并不牵扯到新建对象，与对象无关，用实例方法只会徒耗内存。

工具类里都是静态方法，直接用类访问，所以不希望用户创建对象。
将构造器私有化(private)，可以直接禁用对象的创建。不私有的话，会存在默认构造器。
```java
private obsidianUtil() {  
}
```

面向搜索引擎工程师，API调用工程师。

# 案例
需求：实际开发中，经常会遇到一些使用数组的工具类。请按照如下要求编写一个数组的工具类：ArraysUtils。
1. 数组对象直接输出的时候，输出的是地址，但是更多时候需要输出内容，请在ArrayUtils中提供一个方法toString，用于返回整组数组的内容，返回的字符串格式如：[10,20,50,34,100]
2. 经常需要统计平均值，平均值为去掉最低分和最高分后的分值，请提供一个工具方法getAvrage，用于返回平均分（只考虑浮点型数组，且只考虑一维数组）
3. 定义一个测试类TestDemo，调用该工具类的工具方法，并返回结果。


工具类
```java
public class ArrayUtils {  
    public static String toString(int[] arrs){  
        //校验  
        if (arrs==null){  
            return null;  
        }  
  
        String rs="";  
        rs+="[";  
        for (int i = 0; i < arrs.length; i++) {  
            rs+=arrs[i];  
            rs+= i==arrs.length-1?"":",";  
        }  
        rs+="]";  
        return rs;  
    }  
  
    public static double getAverage(double[] arrs){  
        //校验  
        if (arrs==null||arrs.length<=2){  
            return -1;  
        }  
        //此处的null和length<=2不可以写反，如果输入的数组是null，则读取arrs.length会报错  
  
        //冒泡排序  
        for (int j = 1; j < arrs.length; j++) {  
            for (int i = 0; i < arrs.length-j; i++) {  
                if (arrs[i]>arrs[i+1]){  
                    double temp=arrs[i+1];  
                    arrs[i+1]=arrs[i];  
                    arrs[i]=temp;  
                }  
            }  
        }  
  
        //去掉最低最高求平均值  
        double sum=0;  
        for (int i = 1; i < arrs.length-1; i++) {  
            sum+=arrs[i];  
        }  
        double ave=sum/(arrs.length-2);  
        return ave;  
    }  
}
```

调用
```java
public static void main(String[] args) {  
    int[] arrs={10,20,30,40,50};  
    System.out.println(ArrayUtils.toString(arrs));  
    double[] arrs2={10,20,20,20,30};  
    System.out.println(ArrayUtils.getAverage(arrs2));  
}
```