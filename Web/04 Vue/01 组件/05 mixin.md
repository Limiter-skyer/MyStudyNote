混合，当你很多组件都需要用到相同或者类似的方法，在每个组件中都写一遍，显然是重复工作。于是使用模块化的知识，可以在组件外写好方法，导入组件中使用。

组件外，我们需要写一个js文件，把需要的methods写好，里面需要用export暴露。
```js
export const Comic = {
    methods:{
        sell(){
            alert("clamp卖出了漫画");
        }
    }
}
```
这个文件命名为clamp.js

组件内，我们需要导入这个js模块，然后再vue对象上配置一个mixin属性，来接受混合。
```vue
<template>
  <div>
    <button @click="sell">clamp在干什么</button>
  </div>
</template>

<script>
import { Comic } from "@/clamp";

export default {
    name:"App",
    mixins:[Comic]
}
</script>
```
任何与methods,data平级，写在此处的配置项，都可以写进混合里。

全局混合：不需要一个组件一个组件的导入了，直接在**main.js**里导入配置一次，全局都可以使用该混合
```js
//导入混合
import { Comic } from "@/clamp";
//配置混合
Vue.mixin(Comic);
```


其实混合用的不多，因为说到底，还是违背了Vue的html+css+js一体化的思想。但是有需要就要解决。写得顺手，效率高，是最重要的，没有什么比实际解决问题更重要。