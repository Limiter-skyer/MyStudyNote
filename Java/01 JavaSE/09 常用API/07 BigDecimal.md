大数据类型，用于解决浮点运算失真的情况。
0.1+0.2=0.300000000004

禁止使用BigDecimal(double)的方式把double值转换为BigDecimal对象，这个构造器依旧存在精度损失的问题。
如：BigDecimal g = new BigDecimal(0.1F)，实际的值为：0.10000000149

推荐使用入参为String的构造器；或者使用valueOf方法，该方法内部其实执行了Double的toString，而Double的toString按double的实际能表达的精度对尾数进行了截断。
`BigDecimal recommend1 = new BigDecimal("0.1")`
`BigDecimal recommend2 = BigDecimal.valueOf(0.1)`


# 常用方法
