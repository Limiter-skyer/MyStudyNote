ajax是与后端交互的技术，我们作为前端，前端的技术我们我们是会的，但是后端到哪里去找呢。
如今就委曲求全一下，自己搞一个简易的，只做演示用的后端。

# node.js
node.js 这个东西，本身就是为了写后端发明出来的，我作为一个前端，用到他的地方，只有他的npm包管理工具了，真的非常爽。
回到他本身，node.js 是 javascript 的一种运行环境，是服务器端的 javascript 的解释器。
npm则是包含在node.js里面的一个包管理工具，就如同linux中的yum仓库，rpm包管理；如同python中的pip包管理工具一样。
npm 和 node.js 是绑定的，安装上 node.js，npm 也就安装完成了。
所以任何前端工程师的电脑上，node.js 都是不可或缺的。
但不一定是用他来写后端，只是用一用 npm 而已。

现在，我们真的要用它来写后端了

# express
express是一个基于 node.js 的服务器框架，用它可以非常快捷地搭建一个服务器，而不需要去搞 MySQL。

首先找个空文件夹，初始化npm，然后按照官网安装 express。

```
npm init --yes //初始化
npm install express
```

然后我们新建一个 js 文件

```js
// 引入
const express = require('express');


// 创建应用对象

const app = express()


// 创建路由规则
/**
 *
 * @param {any} request 对请求报文的封装
 * @param {any} response 对响应报文的封装
 */
app.get('/',(request,response) => {
    response.send('hello express')
});


// 监听端口
app.listen(8000,() => {
    console.log('服务已经启动')
})
```

这样一个服务器就搭好了，在浏览器地址栏输入 `127.0.0.1:8000`，就可以访问。前面的 ip 地址是自己的 ip，后面的8000是端口，是我们指定的。