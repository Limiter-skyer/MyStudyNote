StringBuilder是可变字符串，可以节约内存。提高字符串的修改效率。
String是不可变字符串，每次修改字符串中的内容，都会新开一个内存，完全新建一个字符串。
![[Pasted image 20220716225324.png]]

# StringBuilder构造器
- StringBuilder()：创建一个空白可变字符串对象，不包含任何内容。
- StringBuilder(String str)：创建一个指定字符串内容的可变字符串对象

# 常用方法
![[Pasted image 20220716214437.png]]

append支持链式编程，因为每次执行完append方法后都会把原对象返回回来。
对象.append(1).append(2).append(3).append(4).append(5).append(6)。
reverse()也支持链式编程。

最终的结果还是要转换成String，用toString方法。

# 案例：打印数组
写一个方法，可以打印出数组的全部数据。[12,24,36,48]如此。
```java
public static String toString(int[] arr){  
    //确保输入的数组不是null  
    if (arr!=null){  
        //开始拼接  
        StringBuilder str =new StringBuilder("[");  
        for (int i = 0; i < arr.length; i++) {  
            str.append(arr[i]).append(i==arr.length-1?"":",");  
        }  
        str.append("]");  
        return str.toString();  
    }else {  
        return null;  
    }  
}
```