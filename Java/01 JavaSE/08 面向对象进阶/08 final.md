final关键字是最终的意思，可以修饰（类，方法、变量）
1. 修饰类：表明该类是最终类，不能被继承。
2. 修饰方法：表面该方法是最终方法，不能被重写。
3. 修饰变量：表示该变量第一次赋值后，不能被再次赋值（有且仅能被赋值一次）

1. 修饰类的应用：工具类
2. 修饰方法的应用：
3. 修饰变量的应用：
	1. 局部变量：圆周率，折扣，重力加速度这些逻辑上不变的变量
	2. 静态成员变量，即常量
	3. 实例成员变量：会导致每个对象的某个属性都会一致且无法改变。没有意义，几乎不用

注意事项：
1. final修饰的基本变量，那么变量的数据值不能改变
2. final修饰的引用变量，那么变量的地址值不能改变，地址指向的内容可以改变。

