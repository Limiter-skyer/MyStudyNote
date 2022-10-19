# 概述
String是字符串类型
String时不可变字符串，String每次修改其实都是产生并指向了新的字符串对象。原来的字符串时没有改变的，所以称不可变字符串。

# 创建对象
方式一：""
String s1="";
在字符串常量池中存储，而且相同的内容只会存储一份（节省内存）

方式二：构造器
char[] chars={'a','b','c'};
String s2=new String(chars);
![[Pasted image 20220712085506.png]]

byte[] bytes={92,94,94,91};
String s4 =new String(bytes);
全都转换为对应的ASCII码

# 常用API
## equals()
字符串不适合用`==`来对比，推荐使用String类提供的equals比较。不关心地址，只关心内容。
`equals()`：比较字符串内容而不是地址
`equalsIgnoreCase()`：忽略大小写比较内容

```java
//正确的用户名密码验证码  
String okName = "obsidian";  
String okCode = "123456";  
String okVerCode = "1h4j2i";  
  
//输入账号密码验证码  
Scanner sc = new Scanner(System.in);  
System.out.println("用户名：");  
String name = sc.next();  
System.out.println("密码：");  
String code = sc.next();  
System.out.println("验证码：");  
String vercode = sc.next();  
  
//equals的用法，比较字符串内容而不是地址  
if (okName.equals(name)&&okCode.equals(code)){  
    //equalsIgnoreCase的用法，忽略大小写比较字符串  
    if (okVerCode.equalsIgnoreCase(vercode)){  
        System.out.println("登录成功！");  
    }else {  
        System.out.println("验证码错误");  
    }  
}else {  
    System.out.println("账号密码错误");  
}
```

## length()
返回字符串的长度int
```java
String name ="醉后不知天在水，满船清梦压星河。";  
//public int length();  
// 获取字符串的长度  
System.out.println(name.length());
```

## charAt()
返回某个索引处的字符char
```java
System.out.println(name.charAt(5));
```

## toCharArray()
将当前字符串转换为字符数组返回char[]
```java
char[] chs=name.toCharArray();  
System.out.println("---------------------");  
for (int i = 0; i < chs.length; i++) {  
    System.out.print(chs[i]+"\t");  
}  
System.out.println();
```

## substring()
public String substring(int BeginIndex, int endIndex)
根据开始索引到结束索引之间进行截取，得到新的字符串，**包前不包后**
public String substring(int BeginIndex)
从开始索引到末尾，截取新的字符串
```java
String name1 = name.substring(0,8);  
System.out.println(name1);
String name2 = name.substring(8);  
System.out.println(name2);
```

## replace()
public String replace(CharSequence target, CharSequence replacement)
用新值，替换掉字符串中的旧值，得到新的字符串
```java
String str = "近朱者赤，近墨者黑，近月者弯";  
System.out.println(str);  
String rs1=str.replace("近月","**");  
System.out.println(rs1);
```

## contains()
public boolean contains(CharSequence s);判断是否有字符串  
```java
System.out.println(str.contains("近月"));
```

## starsWiths
public boolean starsWiths(String prefix);是否以某字符串开始  
```java
System.out.println(str.startsWith("近月"));
```

## split()
public String[] split(String regex)
根据传入的规则切割字符串，得到字符串数组返回。
```java
String str2="犬夜叉，杀生丸，桔梗，戈薇，玲";  
String[] strs2 = str2.split("，");  
for (int i = 0; i < strs2.length; i++) {  
    System.out.println(strs2[i]);  
}
```

# 案例
## 验证码
验证码
需求：随机产生一个5位数的验证码，每位可能是数字、大写字母、小写字母。
分析：
1. 定义一个String类型的变量储存大小写字母和数字全部字符。
2. 循环5次，随机一个范围的索引，获取对应字符连接起来即可。

```java
String datas="1234567890qwertyuiopasdfghjklzxcvbnmQWERTYUIOPASDFGHJKLZXCVBNM";  
Random r =new Random();  
String code="";  
for (int i = 0; i < 5; i++) {  
    //随机索引  
    int index=r.nextInt(datas.length());  
    char a  = datas.charAt(index);  
    code+=a;  
}  
System.out.println(code);
```

## 模拟用户登录系统
系统后台定义正确的登录名称，密码。
使用循环控制3次，让用户输入正确的登录名和密码，判断是否登录成功，登录成功则不再进行登录；登录失败给出提示，并让用户继续登录。
```java
String okLoginName = "admin";  
String okPassword = "123456";  
  
//定义一个循环，循环3次，让用户登录  
for (int i=1;i<=3;i++) {  
    Scanner sc = new Scanner(System.in);  
    System.out.println("请输入用户名");  
    String loginName = sc.next();  
    System.out.println("请输入密码");  
    String password = sc.next();  
  
    //判断  
    if (okLoginName.equals(loginName)) {  
        if (okPassword.equals(password)) {  
            System.out.println("登录成功");  
        } else {  
            System.out.println("密码错误，您好剩余"+(3-i)+"次机会");  
        }  
    } else {  
        System.out.println("用户名错误，您好剩余"+(3-i)+"次机会");  
    }  
}
```

## 手机号屏蔽
需求：以字符串形式从键盘接受一个手机号，将中间四位号码屏蔽，最终效果为：
`152****4339`
分析：
1. 键盘录入一个字符串，用Scanner实现
2. 截取字符串前三位，截取字符串后4位
3. 中间替换

```java
Scanner sc = new Scanner(System.in);  
System.out.println("请输入手机号");  
String tel =sc.next();  
  
String before = tel.substring(0,3);  
String after = tel.substring(7);  
  
String s=before+"****"+after;  
System.out.println(s);
```



