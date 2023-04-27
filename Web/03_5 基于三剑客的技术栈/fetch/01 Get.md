fetch 的操作，可谓大大精简，简直是官方封装的 `Axios`，代码如下

```js
fetch('http://ajax-base-api-t.itheima.net/api/getbooks').then(value => {
    return value.json()
}).then(value => {
    console.log(value)
}).catch(reason => {
    console.log(reason)
})
```