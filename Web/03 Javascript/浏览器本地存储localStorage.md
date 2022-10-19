# localStorage
ocalStorage是浏览器本地缓存，最常用来做搜索历史记录。

Api有四个，写入，读取，删除，清空
```js
//写入
localStorage.setItem('key','value');

//读取
localStorage.getItem('key');

//删除
localStorgae.removeItem('key');

//清空
localStorage.clear();
```

本地缓存是以键值对的方式储存的。想保存一个数据，就要写成键值对的形式。
而且只接受字符串的数据类型。如果是其他类型，会进行数据装换，调用toString方法。
以对象举例，默认的toString方法会让存储的对象不可读。虽说本应该避免储存对象，但是如果真的要在缓存里储存对象类型的话，可以调用JSON。
```js
//写入对象
const p = {name:'张三',age:18,sex:'man'}
localStorage.setItem('person',JSON.stringify(p));

//在控制台输出被字符串化的对象
const result = localStorage.getItem('person');
console.log(JSON.parse(result));
```
如果读取了不存在的数据，那么结果是null

# sessionStorage
API与lcoalStorage完全一致，区别是，浏览器一关就清空。