call、aplly、bind，前文都介绍过。这些方法的作用是改变this的指向，特点是作用于函数。
例子：

```js
var foo = {
    value: 1
};

function bar() {
    console.log(this.value);
}

bar.call(foo); // 1
```

注意两点：
1. call 改变了 this 的指向
2. bar 函数执行了

# 模拟实现第一步

那我们该如何模拟实现这两个效果？
试想调用 call 的时候，把 foo 对象改造成如下

```js
var foo = {
    value: 1,
    bar: function() {
        console.log(this.value)
    }
};

foo.bar(); // 1
```

这时候 this 就指向了 foo ，是不是很简单。
但是这样就给 foo 添加了一个属性，破坏了数据。
不过也不用担心，调用完之后 delete 就行了。
所以我们模拟的步骤可以分为：
1. 将函数设置为对象的属性
2. 执行该函数
3. 删除该函数

以这个例子为例就是：
```js
//第一步
foo.fn = bar
//第二步
foo.fn()
//第三步
delete foo.fn
```
fn 是对象的属性名，反正要删除，所以随意起什么名字。

根据这个思路，我们可以写出第一版的 call 函数：

```js
Function.prototype.call2 = function(context){
	//首先要获取调用call2的函数，用this可以获取
	context.fn = this;
	context.fn()
	delete context.fn
}

//测试一下
function bar(){
	console.log(this.value)
}

var foo = {
	value : 1
}

bar.call2(foo)//1
```

第一个功能算是完成了

# 模拟实现第二步

call 还能给定参数执行函数，例如

```js
var foo = {
    value: 1
};

function bar(name, age) {
    console.log(name)
    console.log(age)
    console.log(this.value);
}

bar.call(foo, 'kevin', 18);
// kevin
// 18
// 1

```

注意：传入的参数并不确定个数，如何处理？
我们可以从 Arguments 对象中取值，取出第二个到最后一个函数，然后放到一个数组里
比如

```js
// 以上个例子为例，此时的arguments为：
// arguments = {
//      0: foo,
//      1: 'kevin',
//      2: 18,
//      length: 3
// }
// 因为arguments是类数组对象，所以可以用for循环
var args = [];
for(var i = 1, len = arguments.length; i < len; i++) {
    args.push('arguments[' + i + ']');
}

// 执行后 args为 ["arguments[1]", "arguments[2]", "arguments[3]"]
```

不定长的参数问题解决了，我们接着要把这个参数数组放到要执行的函数的参数里面去。
这一步看似简单其实还有些难搞

```js
context.fn(args.join(','))
//(O_o)??
//这样肯定不行的

//也可以用es6的展开
context.fn(...args)
//但是call这个方法es3就有了，这父子关系就乱了

//eval方法可以
eval('context.fn('+args+')')
//args会自动调用Array.toString
```

所以我们第二版克服了两个大问题，代码如下：

```js
// 第二版
Function.prototype.call2 = function(context) {
    context.fn = this;
    var args = [];
    for(var i = 1, len = arguments.length; i < len; i++) {
        args.push('arguments[' + i + ']');
    }
    eval('context.fn(' + args +')');
    delete context.fn;
}

// 测试一下
var foo = {
    value: 1
};

function bar(name, age) {
    console.log(name)
    console.log(age)
    console.log(this.value);
}

bar.call2(foo, 'kevin', 18); 
// kevin
// 18
// 1
```

成功

# 模拟实现第三步

模拟代码已经完成80%，还有两个小点要注意：
**1. this 的参数可以传 null，当为 null 的时候，视为指向 windows**
举个例子：

```js
var value = 1;

function bar() {
    console.log(this.value);
}

bar.call(null); // 1
```

**2. 函数是有返回值的**

这两个问题就非常好解决了，直接看最后一版的代码

```js
// 第三版
Function.prototype.call2 = function (context) {
    var context = context || window;
    context.fn = this;

    var args = [];
    for(var i = 1, len = arguments.length; i < len; i++) {
        args.push('arguments[' + i + ']');
    }

    var result = eval('context.fn(' + args +')');

    delete context.fn
    return result;
}

// 测试一下
var value = 2;

var obj = {
    value: 1
}

function bar(name, age) {
    console.log(this.value);
    return {
        value: this.value,
        name: name,
        age: age
    }
}

bar.call2(null); // 2

console.log(bar.call2(obj, 'kevin', 18));
// 1
// Object {
//    value: 1,
//    name: 'kevin',
//    age: 18
// }
```

# apply的模拟实现

apply 的实现，那就是依葫芦画瓢了。

```js
Function.prototype.apply = function (context, arr) {
    var context = Object(context) || window;
    context.fn = this;

    var result;
    if (!arr) {
        result = context.fn();
    }
    else {
        var args = [];
        for (var i = 0, len = arr.length; i < len; i++) {
            args.push('arr[' + i + ']');
        }
        result = eval('context.fn(' + args + ')')
    }

    delete context.fn
    return result;
}
```
