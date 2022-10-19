数组的工具类

常用：
```java
int[] arr = {15,24,43,84,13};  
System.out.println(arr);//输出地址  
System.out.println(Arrays.toString(arr));//输出内容  
  
Arrays.sort(arr);  
System.out.println(Arrays.toString(arr));//输出排序内容  
  
//二分搜索，只能对排好序的数组使用，否则逻辑上会出错  
int index = Arrays.binarySearch(arr,13);//在arr中找13这个值  
System.out.println(index);
```

比较器：自定义比较规则
```java
Integer[] ages = {34,12,46,32,24};  
/**  
 * 参数一：被比较的数组  
 * 参数二：匿名内部类对象，代表了一个比较器对象。  
 */  
Arrays.sort(ages, new Comparator<Integer>() {  
    @Override  
    public int compare(Integer o1, Integer o2) {  
        //o1和o2进行比较，左边数据大于右边数据返回正整数，等于返回0  
        return o2-o1;  
    }  
});  
System.out.println(Arrays.toString(ages));
```
详见API文档