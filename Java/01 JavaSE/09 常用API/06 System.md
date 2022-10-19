System的功能是通用的，都是直接用类名调用，所以System不能被实例化。

# 常用方法
- public static void exit(int status)：终止当前的java虚拟机，这种情况下是全0中止，非零表示异常中止。
- public static long currentTimeMillis()：返回当前系统的时间毫秒值形式。
- public static void arraycopy()：数组拷贝