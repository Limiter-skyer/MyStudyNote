一般，我们使用Vue的情况下，就要避免去操作DOM，因为Vue就是为了避免操作DOM才发明出来的。但是如果需求就是要求我们输出一个DOM，该怎么办。
正常我们通过给html元素一个id，比如`<div id="title></div>"`，再用js的api`getElementById()`来获取DOM。
我们换一种写法，把`id`改为`ref`
```html
<h1 ref="title">贺新郎·读史</h1>
```
这样我们就可以在vue对象上访问到这个DOM元素
```js
export default {
    name:"TitleRef",
    methods:{
      showDom(){
        console.log(this.$refs.title)
      }
    }
}
```
 在vm对象上的`$refs`属性上。

ref和id处理传统的html元素，并没有什么太大差别，主要差别在在处理组件上，二者的返回值是不一样的。
```vue
<template>
  <div>
    <title-ref ref="title" id="title"></title-ref>
    <button @click="showDom">输出Dom</button>
  </div>
</template>

<script>
import TitleRef from "@/components/TitleRef.vue";
export default {
    name:"App",
    components:{TitleRef},
    methods:{
      showDom(){
        console.log(this.$refs.title);
        console.log(document.getElementById("title"));
      }
    }
}
</script>
```
给组件TitleRef同时加上了id和ref，分别用vue和js的语法去访问他们，返回结果如下
ref
```
VueComponent {_uid: 7, _isVue: true, __v_skip: true, _scope: EffectScope, $options: {…}, …}
```
id
```
  <div>
    <h1>贺新郎·读史</h1>
  </div>
```

本质上，vue作为一个框架，最终是要翻译成最底层的html+css+js的。ref相当于是翻译前，在vue的框架范围内的操作。id是翻译完，什么template，什么组件，统统变成了html，再执行的`getElementById()`。