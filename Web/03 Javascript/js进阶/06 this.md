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
但是求知就是这样，一开始就是要在黑暗中拓展的。

如何判断this指向，在ES5规范里有详细解释，于是，我们要来学习ES5了
先奉上 ECMAScript 5.1 规范地址：

英文版：[http://es5.github.io/#x15.1](http://es5.github.io/#x15.1)

中文版：[http://yanhaijing.com/es5/#115](http://yanhaijing.com/es5/#115)

让我们开始了解规范吧！

# Types

第8章Types，类型

> Types are further subclassified into ECMAScript language types and specification types.

> An ECMAScript language type corresponds to values that are directly manipulated by an ECMAScript programmer using the ECMAScript language. The ECMAScript language types are Undefined, Null, Boolean, String, Number, and Object.

> A specification type corresponds to meta-values that are used within algorithms to describe the semantics of ECMAScript language constructs and ECMAScript language types. The specification types are Reference, List, Completion, Property Descriptor, Property Identifier, Lexical Environment, and Environment Record.

简述一下就是，ECMAScript 的类型分为语言类型和规范类型。
语言类型，就是我们的老朋友，Undefined，Number，Boolean，Null，String之类的。

而规范类型相当于 meta-values，是用来用算法描述 ECMAScript 语言结构和 ECMAScript 语言类型的。规范类型包括：Reference, List, Completion, Property Descriptor, Property Identifier, Lexical Environment, 和 Environment Record。

我看到这里就直接懵了，远远超出了我的知识范围，好在现在需要打交道的只有一个Reference类型。它与 this 的指向有着密切的关联。

# Reference

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

# getValue

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

# 如何确定this的值

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

# 具体分析

## 1.  计算 MemberExpression 的结果赋值给 ref

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

## 2.判断 ref 是不是一个 Reference 类型。

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

### foo.bar()

`foo.bar()`，的MemberExpression，是`foo.bar`，
那么 foo.bar 是不是一个 Reference 呢？

查看规范 11.2.1 Property Accessors，这里展示了一个计算的过程，什么都不管了，就看最后一步：

> Return a value of type Reference whose base value is baseValue and whose referenced name is propertyNameString, and whose strict mode flag is strict.

我们得知该表达式返回了一个 Reference 类型！