# 执行上下文

每个执行上下文包含三个重要属性

-   变量对象(Variable object，VO)
-   作用域链(scope chain)
-   this

这章我们来看this

# this

关于this的指向，向来是个很玄学的东西，虽然大部分时间，this就是指向自己，但是出错就出错在那些小部分时间。
解释this有两种方式，一是前人总结的经验，这种方式是好理解，坏处是仅仅是经验之谈，并不能覆盖所有情况。
第二是从底层逻辑来解释，前文都是如此，本文也是如此，但是this的底层逻辑，实在过于难以理解，涉及了大量的，我不知道的知识。没有基础知识的支撑，直接去学搭建在这些基础知识上的高级知识，那可是相当痛苦的行为。

# 经验之谈
先走第一种方式来解释一下
理念上，this指向“自己的上级”
先举几个案例
## 案例
### 案例1
全局情况下，this直接指向windows
```js
console.log(this)//windows

var a = 1000;
console.log(this.a)//1000
```

### 案例2
全局环境下的函数中，this指向undefined，非严格模式会自动指向windows
```js
function fun(){
	console.log(this)//windows，严格模式undefined
}
```

### 案例3
全局环境下的对象中，this指向windows
```js
var obj = {
	a:this//windows
}

var arr = [1,2,this]//数组也是一种对象
```
对于案例1的拓展：声明在全局环境的变量比如obj，他们实际上都是在windows对象上的，即window.obj，那么在obj对象上的函数windows.obj.this，obj的this，指向的是obj的上级。

### 案例4
结合前面两个案例
在对象中的函数中的this，指向对象
```js
var obj = {
	a:1,
	fun:function{
		console.log(this)//obj
	}
}
```
即obj.fun().this，指向fun()的上级obj

### 案例5
把上一个案例再套娃套一层
```js
var obj = {
	a:1,
	b:function(){
		function fun(){
			console.log(this)//windows，严格模式下为undefined
		}
	}
}
```
变成了obj.b().fun().this，this指向fun()的上级即b()，但可惜b()是个函数，**那么this会指向undefined**，如果是在非严格模式下，undefined会自动指向windows。
这是一条新秩序。

### 案例6
那this是不是看着代码文字往上找层级就行了呢，不行，由于js的内存机制，**this指向哪，不取决于写在哪，而取决于在哪执行**
```js
function fun(){
	console.log(this)
}

var obj = {
	a:1,
	b:fun
}

var t = fun

obj.b()//obj
t()//windows，严格模式下为undefined
```
而且js里的赋值，也就是改个指针，甚至可以这样写
```js
var obj = {
	a:1,
	b:function(){
		console.log(this)
	}
}

var t = obj.b

obj.b()//obj
t()//windows，严格模式下为undefined
```
二者完全等效

### 构造函数
作为构造函数，即涉及到类class的时候，才是this大放光彩的时候。
**如果函数作为构造函数用，那么其中的this就代表它即将new出来的对象**

class的出现，只是对构造函数的一种语法糖，本质上并没有变
```js
function Person(){
	this.name = '张三',
	this.age = 23
}

let zhangSan = new Person()
```
等效于
```js
class Person{
    constructor(name,age){
        this.name = name;
        this.age = age
    }
}

let zhangSan = new Person('张三',23)
```
原理
new做了下面这些事:

-   创建一个临时对象
-   给临时对象绑定原型
-   给临时对象对应属性赋值
-   将临时对象return

如果我不使用new，直接调用这个函数会怎样呢，大概率还是指向undefined


### apply, call, bind
这三个函数本来就是为了改变this指向而出现的。
这些方法很奇特，都是跟在函数后面
```js
var obj = {
    name:'张三',
    age:23,
    smile:true

}

function fun() {
	console.log(this.name)
}

fun.apply(obj)//张三
fun.call(obj)//张三
fun.bind(obj)()//张三
```
只有一个参数的情况下，他们的功能都是把fun函数中的this全都稳定指向obj，当然bind明显返回了一个新的函数，除此之外一样。

不一样的地方在于从第二个参数开始，第二个参数开始的参数，就是fun原函数需要的传参
```js
var person = {
    name:'张三',
    age:23,
    like:'DNF'
}

function run(address,time) {
    console.log(`
    ${this.name}今年${this.age}岁了,
    他每天${time},都会在${address}跑步,
    当然,他最爱的还是${this.like}
    `)
}

run.call(person,'中山陵','早晨6点')
run.apply(person,['中山陵','早晨6点'])
run.bind(person,'中山陵','早晨6点')
```


### 箭头函数
箭头函数是一种不可以用apply和bind,call来改变this的函数。
箭头函数会捕获其所在上下文的this值，作为自己的this值。
拿这不是推翻了之前的，this的指向取决于在哪调用，而不是写在哪。并不是。
出现这种情况的原因，本质上箭头函数只能赋值给一个有内存的变量。
当这个变量在内存里建好后，this也就确定了，再调用也只能调用这个变量。


## 总结
我们从this指向的结果去推导原因，大概可以找到一条“this是指向自己的上级”这么一条因果线，当然，这不完全正确，甚至说完全不正确，只可以通过它初步了解一下。
大部分情况下，this是指向windows和undefined的。而在非严格模式下，undefined会自动转向windows。
而且无论如何，this都只能是“实在的对象”，比如Object，而不能是Function，如果遵从“自己的上级”这个规则指向了Function，那么就会转向undefined。
这个“实在的存在”在解析底层逻辑的时候会详细列出来，放下不提。
而且由于js的内存机制，this指向哪是调用的时候才决定。这个前几章特意说过也，this在执行上下文创建时才确定。

# 底层
现在我们从底层来解释一下

如何判断this指向，在ES5规范里有详细解释，于是，我们要来学习ES5了
先奉上 ECMAScript 5.1 规范地址：

英文版：[http://es5.github.io/#x15.1](http://es5.github.io/#x15.1)

中文版：[http://yanhaijing.com/es5/#115](http://yanhaijing.com/es5/#115)

让我们开始了解规范吧！

## Types

第8章Types，类型

> Types are further subclassified into ECMAScript language types and specification types.

> An ECMAScript language type corresponds to values that are directly manipulated by an ECMAScript programmer using the ECMAScript language. The ECMAScript language types are Undefined, Null, Boolean, String, Number, and Object.

> A specification type corresponds to meta-values that are used within algorithms to describe the semantics of ECMAScript language constructs and ECMAScript language types. The specification types are Reference, List, Completion, Property Descriptor, Property Identifier, Lexical Environment, and Environment Record.

简述一下就是，ECMAScript 的类型分为语言类型和规范类型。
语言类型，就是我们的老朋友，Undefined，Number，Boolean，Null，String之类的。

而规范类型相当于 meta-values，是用来用算法描述 ECMAScript 语言结构和 ECMAScript 语言类型的。规范类型包括：Reference, List, Completion, Property Descriptor, Property Identifier, Lexical Environment, 和 Environment Record。

我看到这里就直接懵了，远远超出了我的知识范围，好在现在需要打交道的只有一个Reference类型。它与 this 的指向有着密切的关联。

## Reference

Reference类型，中文**引用**。

让我们看 8.7 章 The Reference Specification Type：

> The Reference type is used to explain the behaviour of such operators as delete, typeof, and the assignment operators.

所以 Reference 类型就是用来解释诸如 delete、typeof 以及赋值等操作行为的。

抄袭尤雨溪大大的话，就是：

> 这里的 Reference 是一个 Specification Type，也就是 “只存在于规范里的抽象类型”。它们是为了更好地描述语言的底层行为逻辑才存在的，但并不存在于实际的 js 代码中。

再看接下来的这段具体介绍 Reference 的内容：

> A Reference is a resolved name binding.
> A Reference consists of three components, the base value, the referenced name and the Boolean valued strict reference flag.
> The base value is either undefined, an Object, a Boolean, a String, a Number, or an environment record (10.2.1).
> A base value of undefined indicates that the reference could not be resolved to a binding. The referenced name is a String.

由于我的英文水平不好，贴上一些英文的翻译：
```
resolve:解决
bind:绑定
consists of:包括
environment record:专有名词，和Reference一样，是规范类型的一种
indicate:表明
```

这段讲述了 Reference 的构成，由三个组成部分，分别是：

-   base value
-   referenced name
-   strict reference

base value的值只可能是两种可能，
一是undefined，Object，Boolean，String，Number之一。
二是Enviroment Record。这也是规范类型的一种

reference name 是属性的名称，类型是String。

strict reference 是一个flag，自然是Boolean类型。

举两个例子
```js
var foo = 1;

//对应的reference
var fooReference = {
	base : EnviromentRecord,
	name : 'foo',
	strict : false
}
```

```js
var foo = {
    bar: function () {
        return this;
    }
};
 
foo.bar(); // foo

//bar对应的reference
var barReference = {
	base : foo,
	name : 'bar',
	strict : false
}
```
通过以上两个例子，我们了解了reference是什么，那么组成reference的三个属性，我们如何知道谁是谁呢。
在规范中提供了获取 Reference 组成部分的方法，比如 GetBase 和 IsPropertyReference。

> GetBase(V). Returns the base value component of the reference V.
> GetReferencedName(V). Returns the referenced name component of the reference V.
> IsStrictReference(V). Returns the strict reference component of the reference V.
> HasPrimitiveBase(V). Returns **true** if the base value is a Boolean, String, or Number.
> IsPropertyReference(V). Returns **true** if either the base value is an object or [HasPrimitiveBase](http://es5.github.io/#HasPrimitiveBase) (V) is **true**; otherwise returns **false**.
> IsUnresolvableReference(V). Returns **true** if the base value is **undefined** and **false** otherwise.

GetBase(V): 获取V的reference的base value
GetRerencedName(V): 获取V的reference的reference name
IsPropertyReference(V): 如果base value是一个Object，Number，String，Boolean，那么返回true

好像说了什么，又好像什么都没说。
所以到底base value该怎么确定。
目前看起来就是父级。如果V就是在一个对象A里，那么V的reference的base value，就是对象A。如果V就是老大，没有上级，那么base value 就是Enviroment Record

## getValue

除此之外，紧接着在 8.7.1 章规范中就讲了一个用于从 Reference 类型获取对应值的方法： GetValue。

简单模拟 GetValue 的使用：
```js
var foo = 1;

var fooReference = {
    base: EnvironmentRecord,
    name: 'foo',
    strict: false
};

GetValue(fooReference) // 1;
```

GetValue 返回对象属性真正的值，但是要注意：

**调用 GetValue，返回的将是具体的值，而不再是一个 Reference**

这个很重要，这个很重要，这个很重要。

## 如何确定this的值

关于 Reference 讲了那么多，为什么要讲 Reference 呢？到底 Reference 跟本文的主题 this 有哪些关联呢？如果你能耐心看完之前的内容，以下开始进入高能阶段：

看规范 11.2.3 Function Calls：

这里讲了当函数调用的时候，如何确定 this 的取值。

只看第一步、第六步、第七步：

> 1.Let _ref_ be the result of evaluating MemberExpression.
> 6.If Type(_ref_) is Reference, then
> 
> `a.If IsPropertyReference(ref) is true, then`
> `      i.Let thisValue be GetBase(ref).`
> 
> `b.Else, the base of ref is an Environment Record`
> `     i.Let thisValue be the result of calling the ImplicitThisValue concrete method of GetBase(ref).`
> 
> 7.Else, Type(_ref_) is not Reference.

让我们描述一下：
1.计算 MemberExpression 的结果赋值给 ref
2.判断 ref 是不是一个 Reference 类型
```
2.1 如果 ref 是 Reference，并且 IsPropertyReference(ref) 是 true, 那么 this 的值为 GetBase(ref)

2.2 如果 ref 是 Reference，并且 base value 值是 Environment Record, 那么this的值为 ImplicitThisValue(ref)

2.3 如果 ref 不是 Reference，那么 this 的值为 undefined
```

## 具体分析

### 1.  计算 MemberExpression 的结果赋值给 ref

MenberExpression又是什么东西？？？？？
**其实就是表达式**
看规范 11.2 Left-Hand-Side Expressions：

MemberExpression :

-   `PrimaryExpression` // 原始表达式 可以参见《JavaScript权威指南第四章》
-   `FunctionExpression` // 函数定义表达式
-   `MemberExpression [ Expression ] `// 属性访问表达式
-   `MemberExpression . IdentifierName `// 属性访问表达式
-   `new MemberExpression Arguments `// 对象创建表达式

莫名其妙，但是举个例子就懂了
```js
function foo() {
    console.log(this)
}

foo(); // MemberExpression 是 foo

function foo() {
    return function() {
        console.log(this)
    }
}

foo()(); // MemberExpression 是 foo()

var foo = {
    bar: function () {
        return this;
    }
}

foo.bar(); // MemberExpression 是 foo.bar

```

根据现象猜本质，就是去掉最后一个括号。

### 2.判断 ref 是不是一个 Reference 类型。

如何判断ref是不是Reference类型，比如`foo`，`foo()`，`foo.bar`，true or false？

接下来用几个例子来解释
```js
var value = 1;

var foo = {
  value: 2,
  bar: function () {
    return this.value;
  }
}

//示例1
console.log(foo.bar());
//示例2
console.log((foo.bar)());
//示例3
console.log((foo.bar = foo.bar)());
//示例4
console.log((false || foo.bar)());
//示例5
console.log((foo.bar, foo.bar)());
```

#### foo.bar()

`foo.bar()`，的MemberExpression，是`foo.bar`，
那么 foo.bar 是不是一个 Reference 呢？

查看规范 11.2.1 Property Accessors，这里展示了一个计算的过程，什么都不管了，就看最后一步：

> Return a value of type Reference whose base value is baseValue and whose referenced name is propertyNameString, and whose strict mode flag is strict.

我们得知该表达式返回了一个 Reference 类型！