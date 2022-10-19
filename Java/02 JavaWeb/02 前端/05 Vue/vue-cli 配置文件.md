# 查看配置
在终端中输入命令`vue inspect > output.js` 可以输出一个含有配置信息的文件。但这个文件只供预览，并不生效。


在不修改默认配置的情况下，有以下文件不能改：
- public文件夹的name
- public文件夹下的index.html的name不能改
- public文件夹下的favicon.ico网站图标的name不能改？？？
- src文件夹的name
- src文件夹下，main.js的name

# 修改配置
在package.json的同级目录下，新建一个vue.config.js的文件(最新版建好了)。
在Vue-cli的[配置文档](https://cli.vuejs.org/zh/config/) 中，详细写明了哪些可以修改。
比如关闭eslint语法检查`lintOnSave:false`