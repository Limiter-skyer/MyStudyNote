对于 bind 方法，前文也早已介绍过，它与 call 大致一样，都可以改变 this 的指向，不一样的地方是 bind 会返回一个具有该功能的函数，并且不会执行。
总结一下有两个特点：
1. 返回一个函数
2. 可以传入参数

# 返回函数的模拟实现
___
bind 的改变this指向功能，直接用 call 来实现

```js
Function.prototype.bind2 = function(context){
	var self = this;
	return function(){
		return self.apply(context)
	}
}
```

return 的函数内部还有 return，是考虑到调用 bind 的函数也是有返回值的

```js
var foo = {
    value: 1
};

function bar() {
	return this.value;
}

var bindFoo = bar.bind(foo);

console.log(bindFoo()); // 1
```

# 传参的模拟实现
___
bind 返回的函数是不执行的，只有执行的时候需要传参，按理来说传参也和 bind 没什么关系。但是 bind 是可以传参的，看案例：

```js
var foo = {
    value: 1
};

function bar(name, age) {
    console.log(this.value);
    console.log(name);
    console.log(age);

}

var bindFoo = bar.bind(foo, 'daisy');
bindFoo('18');
// 1
// daisy
// 18
```

可以在 bind 的时候传一个参数，返回后执行的时候传第二个参数。这就复杂了。
可以用 arguments 处理

```js
// 第二版
Function.prototype.bind2 = function (context) {

    var self = this;
    // 获取bind2函数从第二个参数到最后一个参数
    var args = Array.prototype.slice.call(arguments, 1);

    return function () {
        // 这个时候的arguments是指bind返回的函数传入的参数
        var bindArgs = Array.prototype.slice.call(arguments);
        return self.apply(context, args.concat(bindArgs));
    }

}
```

`Array.prototype.slice.call()` 为什么可以截取出第二个到最后一个参数。
首先数组 `Array` 原型链上的方法 `slice(begin,end)` 本身有截取数组的功能，从第 `begin` 位截取到 `end` 位。但是这个方法作用于数组，而不是对象，而 `arguments` 就是对象，尽管这个对象的数据结构和数组无异。

```js
var arguments = {
	"0" : 5,
	"1" : 5,
	"2" : 5,
	length : 3
}
//这个对象和下面的数组是非常像的
var args = [5,5,5]

argument[0]//5
args[0]//5

//对于数组的截取，可以用slice
args.slice[1,2]//[5,5]
//对于相似结构的对象，可以用call来完成
Array.prototype.slice.call(arguments,1,2)//[5,5]
```

那这过程到底发生了什么手写一下 `slice` 的源码就可以明白

```js
Array.prototype.Myslice = function (begin,end){
	var start = begin || 0;   //判断begin时候存在 不存在给0 这里判断可以加强
	var len = this;    //获取this.length  这里得到了call进来的对象
	
	start = (start >= 0) ? start : Math.max(0, len + start); //判断参数是不是是不是大于1,负数情况下的begin取值
	end = (typeof end == 'number') ? Math.min(end, len) : len;  //判断end是不是大于this.length的长度
	if(end<0){
	    end = end + len  //判断负值的情况
	}
	var result = new Array();
	for (let i = 0; i < end.length; i++) {
	    result.push(this[i])
	}
	return result;
}

function list() {
	return Array.prototype.Myslice.call(arguments);
}
console.log(list(1, 2, 3));

```

我们可以看到，在  `result.push(this[i])` 这个语句中，`this[i]` 中的 `this` 他应该是一个数组，但是如果他是一个如 `arguments` 一样的对象，也是可以运行的。

# 构造函数效果的模拟实现
___
完成了这两点，最难的部分到了！因为 bind 还有一个特点那就是

> 一个绑定函数也能使用new操作符创建对象：这种行为就像把原函数当成构造器。提供的 this 值被忽略，同时调用时的参数被提供给模拟函数。

也就是说当 bind 返回的函数作为构造函数的时候，bind 时指定的 this 值会失效，但传入的参数依然生效。举个例子：

```js
var value = 2;

var foo = {
    value: 1
};

function bar(name, age) {
    this.habit = 'shopping';
    console.log(this.value);
    console.log(name);
    console.log(age);
}

bar.prototype.friend = 'kevin';

var bindFoo = bar.bind(foo, 'daisy');

var obj = new bindFoo('18');
// undefined
// daisy
// 18
console.log(obj.habit);
console.log(obj.friend);
// shopping
// kevin
```

注意：尽管在全局和 foo 中都声明了 value 值，最后依然返回了 undefind，说明绑定的 this 失效了，如果大家了解 new 的模拟实现，就会知道这个时候的 this 已经指向了 obj。

所以我们可以通过修改返回的函数的原型来实现，让我们写一下：
```js
// 第三版
Function.prototype.bind2 = function (context) {
    var self = this;
    var args = Array.prototype.slice.call(arguments, 1);

    var fBound = function () {
        var bindArgs = Array.prototype.slice.call(arguments);
        // 当作为构造函数时，this 指向实例，此时结果为 true，将绑定函数的 this 指向该实例，可以让实例获得来自绑定函数的值
        // 以上面的是 demo 为例，如果改成 `this instanceof fBound ? null : context`，实例只是一个空对象，将 null 改成 this ，实例会具有 habit 属性
        // 当作为普通函数时，this 指向 window，此时结果为 false，将绑定函数的 this 指向 context
        return self.apply(this instanceof fBound ? this : context, args.concat(bindArgs));
    }
    // 修改返回函数的 prototype 为绑定函数的 prototype，实例就可以继承绑定函数的原型中的值
    fBound.prototype = this.prototype;
    return fBound;
}

```