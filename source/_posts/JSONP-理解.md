---
title: JSONP 理解
date: 2019-01-08 11:35:35
tags:
---


## 什么是JSONP

- 请求方：前端程序员（浏览器）
- 响应方：后端程序员（服务器）

这就是 JSONP 的整个过程：

- 请求方创建 script，src 指向响应方，同时传一个查询参数 ?callbackName=xxx
- 响应方根据查询参数 callbackName，构造一个函数调用 xxx.call(undefined, '请求方需要的数据') 这样的响应
- 请求方浏览器收到响应，就会执行 xxx.call(undefined, '请求方需要的数据')

行业约定：

1. callbackName -> callback
2. xxx -> 名字+随机数

## JSONP 请求方代码

```js
// 请求方创建 script
var script = document.createElement('script')
// functionName 是名字加随机数
var functionName = jsonpCallback + parseInt(Math.random() * 100000, 10)
// script 的 src 指向响应方，同时传一个查询参数 ?callback=functionName
script.src = 'http://example.com?callback=' + functionName
// 浏览器收到响应，就会执行 xxx.call(undefined, '请求方需要的数据')
window[functionName] = function(result) {
  // result 是请求方需要的数据
  // 这里可以对数据进行处理
}
document.body.appendChild(script)
script.onload = function(e) {
  // 移除动态加入的 script
  e.currentTarget.remove()
  // 删除内存中的 functionName 函数
  delete window[functionName]
}
script.onerror = function(e) {
  e.currentTarget.remove()
  delete window[functionName]
}
```

## JSONP 为什么无法支持 POST

- JSONP 是通过动态创建的 script 实现的
- 动态创建的 script 只能用 GET 没法用 POST