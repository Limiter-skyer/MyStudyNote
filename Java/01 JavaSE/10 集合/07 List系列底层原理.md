# ArrayList集合的底层原理
- 基于数组实现：根据索引定位元素快，增删需要做元素位移
- 第一次创建集合并添加第一个元素的时候，在底层创建一个默认长度为10的数组。
- 每次容量满了会进行1.5倍的扩容。

# LinkedList集合的底层原理
很多操作首位指针的方法
- addFirst(E e);
- addLast(E e);
- getFirst()
- getLast()
- removeFirst()
- removeLast()

```java
//linkList是可以完成队列和栈结构（双链表）  
//栈  
LinkedList<String> stack = new LinkedList<>();  
stack.addFirst("第一颗子弹");  
stack.addFirst("第二颗子弹");  
stack.addFirst("第三颗子弹");  
stack.addFirst("第四颗子弹");  
stack.push("第五颗子弹");//push完全等于addFirst  
System.out.println(stack);  
  
System.out.println(stack.removeFirst());  
System.out.println(stack.removeFirst());  
System.out.println(stack.removeFirst());  
System.out.println(stack.pop());//pop完全等于removeFirst  
  
//队列  
LinkedList<String> queue = new LinkedList<>();  
queue.addLast("1号");  
queue.addLast("2号");  
queue.addLast("3号");  
queue.addLast("4号");  
queue.addLast("5号");  
System.out.println(queue);  
System.out.println(queue.removeFirst());  
System.out.println(queue.removeFirst());  
System.out.println(queue.removeFirst());
```

压栈和弹栈分别是push和pop（汇编里也是）

# 异常
当我们从集合中找出某个元素删除的时候可能会出现一种并发修改异常的问题。

- 迭代器遍历集合且直接删除元素的时候可能出现
- 增强for循环遍历集合且直接用集合删除元素的时候可能出现。

```java
List<String> list = new ArrayList<>();  
list.add("java");  
list.add("java");  
list.add("html");  
list.add("spring");  
list.add("springboot");  
System.out.println(list);  
  
//需求：删除全部"java"  
//迭代器  
 Iterator<String> it = list.listIterator();  
while (it.hasNext()){  
    String ele = it.next();  
    if ("java".equals(ele)){  
        it.remove();//使用迭代器的方法删除，而不是list.remove()，否则会错位  
    }  
}  
  
//foreach，底层原理一样，还是会报错  
for (String s:list) {  
    if ("java".equals(s)){  
        list.remove("java");  
    }  
};  
  
  
//lambda表达式，底层原理一样，还是会报错  
list.forEach(s ->{  
    if ("java".equals(s)){  
        list.remove("java");  
    }  
} );  
  
  
//for循环  
for (int i = list.size() - 1; i >= 0; i--) {  
    String ele = list.get(i);  
    if ("java".equals(ele)){  
        list.remove("java");  
    }  
}  
  
  
System.out.println(list);
```

解决：
- 迭代器自带的删除方法可以解决，集合自身的删除方法会报错
- for循环倒着删可以解决