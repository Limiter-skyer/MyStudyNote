# Blob类
___

```js
async function demo(){
	const blob = new Blob(['123'])
	console.log(await blob.arrayBuffer())
}

demo()
// Blob{size:3,type:""}
// Prototype指向Blob
```

# File类
___

```js
async function demo(){
	const file = new File(['123'],'123.txt')
	console.log(await file.arrayBuffer())
}

demo()
/* {
	lastModified: 1677476838023
	lastModifiedDate: Mon Feb 27 2023 13:47:18 GMT+0800 (中国标准时间) {}
	name: "123.txt"
	size: 4
	type: ""
	webkitRelativePath: ""
}
Prototype指向File，File的Prototype指向Blob
/
```
File 比 Blob 多了一个文件名和一大堆时间信息。
由于 File 的原型指向 Blob，所以能实现 Blob 的所有功能，算是上位替代。
与 File 类相关的还有一个 FileList 类，即文件列表。

# arrayBuffer()方法
___
Blob 实例对象有一个方法 arrayBuffer，当然 File 实例对象也能用。

```js
async function demo(){
	//......
    console.log(await blob.arrayBuffer())
    console.log(await file.arrayBuffer())
}
demo()
```

可以读取查看文件中的内容。
这是一个异步方法，会返回一个 Promise 对象

# FileReader类
___
```js
async function demo(){
    const fr = new FileReader()
    fr.onload = function(e){
        console.log(fr.result);
        console.log(e.target.result)
    }
    fr.readAsArrayBuffer(file)
}
demo()
```

FileReader 类是专门用来读取 File 实例的，给 onload 赋值一个回调后，就可以在每次读取 File 实例的时候触发回调。

# DataView类
___

```js
async function demo(){
    const dv = new DataView(await blob.arrayBuffer())
    console.log(dv)
    console.log(dv.getUint8(1))
    dv.setUint8(1,65)
    console.log(dv.getUint8(1))
}
```

DataView 类可以查看或者修改 arrayBuffer() 返回的结果。