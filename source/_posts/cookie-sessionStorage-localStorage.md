---
title: 比较 cookie，localStorage 和 sessionStorage
date: 2017-12-17
tags: 
  - cookie
  - sessionStorage
  - localStroage
  - html5
---

### 前言
cookie, sessionStorage, localStroage都是存储在客户端本地机器上的数据。设置完后，可整站调用，是真正意义上的“全局变量”。

### cookie
由于HTTP无法记录状态，服务器端用cookie来记录用户状态的，由服务器端生成。
通过HTTP把值存储在用户本地机器上，下次服务器端再访问的时候，通过HTTP的请求头带着cookie来确认是不是同一个用户。  

最初是服务器端动态开发web的时候，是服务器端操作。  

有了AJAX后，AJAX请求接口的时候，AJAX通过xhrFields.withCredentials: true，在request.header里带cookie值给服务器端已确认是同一个用户。  

cookie也可以在前端生成，直接document.cookie="key=value;key2=value2;....domain=..path=..expired=..."，是键值对组成的字符串。cookie的设置，读取都是操作字符串，一般大家操作都会自己再重新定义setcookie，getcookie，removecookie等基础方法来操作document.cookie字符串。或者找类似的小框架比如 [cookie操作简易库](https://developer.mozilla.org/en-US/docs/Web/API/Document/cookie/Simple_document.cookie_framework)。  

cookie的存储量最多只能4kb，最多存储的键值对各个浏览器不同，最多的也只有50个。

### Storage
sessionStorage和localStorage都是H5新出来的，用来替换document.cookie的，与HTTP头无关系。
能存储5mb的数据。
有对应的webapi操作数据：
- storge.setItem(key, value) // 设置storage
- storage.getItem(key) // 获取storage
- storage.removeItem(key) // 删除key的storage
- storage.clear() // 清除storage

#### localStorage
localStorage是永久保存的，除非覆盖/清除

#### sessionStorage
sessionStorage，session这个词，大概就能知道它的时效性了。它在关闭会话或者浏览器就会被清除

##### sessionStorage场景？
调查问卷

### 参考链接
- [HTTP cookies 详解](http://blog.csdn.net/lijing198997/article/details/9378047)
- [Document.cookie](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/cookie)
- [详说 cookie, LocalStorage 与 SessionStorage](http://jerryzou.com/posts/cookie-and-web-storage/)
