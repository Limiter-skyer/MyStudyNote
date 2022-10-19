props是父组件向子组件内部传送数据用的。比如著名element的组件
```vue
<template>
  <div>
  	<el-row>
  	  <el-col :span="12"></el-col>
  	  <el-col :span="12"></el-col>
  	</el-row>
  </div>
</template>
```
其中的24分列表组件，通过输入span的值，来决定列表的宽度，12就是一半宽。
此处的`:span="12"`，就是父组件传入子组件的props。此处的冒号是v-bind。

输入数据时，直接在html组件元素上输入即可。子组件想要接受这些数据，还要对每个输入的值一一配置。
调用：
```html
<props-in reader="张三" :age="18"></props-in>
```
之前说过加冒号是v-bind，也就代表这里传入的是js表达式。所以age传入的是Number类型的，而reader并没有加v-bind，传入的就是String数据。

配置：
```vue
<template>
  <div>
    <h1>清平乐·六盘山</h1>
    <p>天高云淡，望断南飞燕</p>
    <p>读者：{{reader}}，年龄：{{age}}</p>
  </div>
</template>

<script>
export default {
    name:"PropsIn",
    props:{
        reader:String,
        age:Number
    }
}
</script>
```
props可以写成对象或者数组，写成对象的话，可以规定输入数据的数据类型。
接受的props使用方法和data一致。