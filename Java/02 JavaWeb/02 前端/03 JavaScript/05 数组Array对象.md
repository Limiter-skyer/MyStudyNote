# 定义
js的数组与java中的集合非常相似，语法不同，罕见得用中括号。（其实在Ae得表达式中就是中括号）
`var person = ["Bill", "Gates", 62];`
对象与数组非常相似
`var person = {firstName:"Bill", lastName:"Gates", age:19};`

访问和java差不多
数组：` var a = person[0];`
对象：` var a = person.firstName;`

js的数组是变长变类型的，而Java的数组是定长的，所以js的数组更像Java的集合。

# 属性
数组也是对象，也有length属性，用来遍历数组。

# 方法
push和splice，添加和删除。
```js
var arrs1 = [1,2,3];
arrs1.push(4);
document.writeln(arrs1);
arrs1.splice(2,2);
document.writeln(arrs1);
```

[参考书](https://www.w3school.com.cn/jsref/jsref_obj_array.asp)
