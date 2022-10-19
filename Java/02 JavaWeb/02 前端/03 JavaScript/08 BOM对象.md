Browser Object Model：浏览器对象模型
1. Windows：浏览器窗口对象
2. Navigator：浏览器对象
3. Screen：屏幕对象
4. History：历史记录对象
5. Location：地址栏对象

# window
window对象可以获取其他四个BOM对象（？？？）
 方法：
 1. alert ()：弹窗
 2. confirm ()：弹出带有确定按钮的弹窗，这个方法有返回值，如果点确定就返回true，点取消就返回false，用以按照用户选择执行后续的分支。
 3. setInterval (方法, 毫秒值)：每隔一段时间循环执行某个函数
 4. setTimeout (方法, 毫秒值)：在某个时间节点执行某个函数

```js
let flag = confirm("确认删除？");
if (flag) {
    alert("执行删除功能");
}
```

```js
setInterval(function study() {
    alert("使劲学");
},5000);

setTimeout(function eat() {
    alert("使劲吃");
},12000);
```
