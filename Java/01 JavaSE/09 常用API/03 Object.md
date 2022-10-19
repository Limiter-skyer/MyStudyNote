所有的类都默认继承了Object类，所以Object类的方法，所有类都是可以直接用的。

# 常用方法
- public StringtoString()：默认返回当前对象在堆内存中的地址信息：类的全限名@地址内存
- public Boolean equals(Object o)：默认比较当前对象与括号内对象的地址是否相同，相同返回true，不同返回false。

toString()方法存在的意义就是为了被子类重写，以便返回对象的内容信息。IEDA右击即可快速重写，输入“toS”也可以快速输入。
equals()方法也同理，本类其方法是比较地址，但是`==`也可以比较地址，该方法主要也是为了给子类重写，让子类自己定义比较规则。同样右键可以生成

# Objects
Objects工具类下的equals()，更安全。
- public static boolean equals(Object a, Object b)：比较两个对象，
	1. 先判断是否为同一个对象
	2. 再判断是否为空
	3. 最后判断两个对象是否相等
- public static boolean isNull(Object o)：判断是否为null，是返回true