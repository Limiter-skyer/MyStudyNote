HTTP (hypertext transport protocol) 协议，超文本传输协议，纤细规定了浏览器和万维网服务器之间互相通信的规则。
他是一种约定，规则，在同一个秩序下，大家才可以愉快的运行。
浏览器向万维网的通信，叫**请求报文**，万维网向浏览器的通信，叫**响应报文**

# 请求
___
重点是格式与参数，即，你浏览器向服务器发送的数据，长什么样子。
由四个部分组成：行，头，空行，体

## 请求行
1. 请求类型，比如 Get/Post
2. Url，比如我在 B 站搜索“前端”，Url
`search.bilibili.com/all?keyword=前端%20http&from_source=webtop_search......`
省略了一些
3. HTTP协议的版本，比如HTTP/1.1

## 请求头
这部分最重要的是格式。请求头的格式都是 key : value 式
1. Host : bilibili.com
2. Cookie : name=liminter 
3. Content-type : application/x-www-form-urlencoded ，告诉服务器我请求体是什么类型的
4. User-Agent : chorme 110
还有很多其他的

## 请求体
空行必须存在
Get 请求的请求体是空的，Post 可以不为空

`username=admin&password=123456`

综合一下

```
POST /s?ie=utf-8 HTTP/1.1
Host : bilibili.com
Coolie : name=liminter
Content-type : application/x-www-form-urlencoded
User-Agent : chorme 110

username=admin&password=123456
```


# 响应
___
响应与请求一样，完全一样的四个部分

## 响应行
1. HTTP 协议版本
2. 状态码，如 `200`，`404`
3. 响应字符串，如 `"ok"`

## 响应头
格式与请求完全一致

## 响应体
看需求

```
HTTP/1.1 200 OK
Content-type : text/html;charset=utf-8
Content-length : 2048
Content-encoding : gzip

<html>
	<head></head>
	<body></body>
</html>
```


# 如何查看
在浏览器种，以谷歌 chorme 为例，F12 界面的 network 中，可以查看所有的请求和对应的响应。
Header 中，Request Headers 是请求头，Response Headers 是响应头.
可以看到远超过文章举例数量的内容
Response 中就是响应体的内容