---
layout: post
title: XMLHttpRequest 对象
date: 2019-03-20 13:28:40
tags:
---


> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

1999 年，微软发布 IE5，引入新功能，它允许 JavaScript 向服务器发起 HTTP 请求。2004 年 Google 发布 Gmail，2005 年 发布 Google Map，这个功能才发广泛重视。

后来这个功能被人命名为 AJAX，Asynchronous JavaScript And XML。通过 JavaScript 发起 HTTP 请求，服务器返回 XML 文档，XML 文档里面带了需要的数据，然后把数据更新到网页的对应部分，这样不用刷新整个网页。

后来 AJAX 就是指 JavaScript 发起 HTTP 请求。

W3C 于 2006 把 AJAX 纳入标准。

一个 AJAX 请求的整个过程是：

1. 创建一个 XMLHttpRequest 实例
2. 发出 HTTP 请求
3. 接收服务器传回的数据
4. 更新网页数据

XMLHttpRequest 对象是 AJAX 的主要接口，用于浏览器于服务器之间的通信。尽管名字有 XML 和 Http，其实它还可以使用多种协议 比如 file 或 ftp。（[但在标准中并没有说](https://www.w3.org/TR/XMLHttpRequest/#introduction)），可以发送任何格式的数据。

AJAX 只能向同源网址（协议、域名、端口都相同）发请求，跨域会报错。

```js
var xhr = new XMLHttpRequest()
xhr.onreadystatechange = function() {
  if (xhr.readyState === 4) {
    if (xhr.status === 200) {
      console.log(xhr.reponseText)
    } else {
      console.log(xhr.statusText)
    }
  }
}
xhr.onerror = function (e) {
  console.error(xhr.statusText)
}
xhr.open('GET', '/endpoint', true);
xhr.send();
```

**xhr.readyState** 返回实例的当前状态一个整数

- 0 表示 XMLHttpRequest 实例已经生成，实例的 open() 方法还没被调用
- 1 表示 open 方法已经调用，但 send 方法还没调用，这时可以调用实例的 setRequestHeader() 方法，设定 HTTP 请求的头信息
- 2 表示 send 方法已经调用，服务器返回的头信息和状态码已经收到
- 3 表示正在接收服务器的数据体 body 部分，这时，如果实例的 reponseType 属性是 text 类型或者空字符串，reponseText 属性就会包含已经收到的部分信息
- 4 表示服务器返回数据已经接收，或者本次接收失败

AJAX 通信过程中，每当实例对象发生状态变化，它的 readyState 属性的值就会改变。这个值每一次变化，都会触发 readyStateChange 事件

**xhr.onreadystatechange** 指 xhr.readyState 属性改变时执行 xhr.onreadystatechange 监听函数。对应的是 readystatechange 事件。

如果调用了实例的 abort 方法，终止请求，也会触发 readyState 属性变化而执行 xhr.onreadystatechange

**xhr.response** 表示服务器返回的数据体（即 HTTP 回应的 body 部分），可以是任何数据类型，具体类型由 xhr.reponseText （只读）决定。如果请求没成功或者数据不完整，返回 null。如果 reponseType 是 text 类型或空字符串，在请求没结束之前（readyState 等于 3 的阶段），reponse 属性包含服务器已经返回的部分数据。[text 类型是指包含在 DOMString 对象中的文本。DOMString 是一个 UTF-16 字符串。JavaScript 的字符串就是 UTF-16 字符串，所以 DOMString 直接映射的 String，DOMString 就是 String](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/responseType)

**xhr.reponseType** 表示服务器返回数据的类型。可以在调用 open 方法之后，send 方法之前设置它的值，告诉浏览器返回指定类型的数据。如果设置为空字符串，就等于默认值 text 类型，也就是普通的字符串。

- ''：等同于 text 类型，表示服务器返回文本数据
- arraybuffer：ArrayBuffer 对象，表示服务器返回二进制数组。ArrayBuffer 在 WebGL 中的应用及了解 WebGL，强烈推荐：[WebGL 技术储备指南](http://taobaofed.org/blog/2015/12/21/webgl-handbook/)
- blob：Blob 对象，binary large object，表示服务器返回的是二进制大对象。
- document：返回一个可以检查和解析的 XML 或者 HTML 的 Document 对象
- json：JSON 对象
- text：字符串

xhr.reponseType 更详细介绍：[理解DOMString、Document、FormData、Blob、File、ArrayBuffer数据类型](https://www.zhangxinxu.com/wordpress/2013/10/understand-domstring-document-formdata-blob-file-arraybuffer/)

**xhr.reponseText** 返回服务器接收到的字符串，只有在 HTTP 请求完成接收后 xhr.status 为 200，它才会包含完整的数据。

**xhr.reponseXML** xhr.reponseType 设置为 document，HTTP 响应的 Content-Type 头信息是 text/xml 或者 application/xml，就可以通过 xhr.reponseXML 接收到服务器端发送来的 XML 或者 HTML 文档对象。如果 HTTP 相应的 Content-Type 头信息不是 application/xml 或 text/xml，但是还是要获取 xhr.reponseXML 的话，可以手动调用下 xhr.overrideMiMeType 方法，强制执行 XML 解析。

```js
var xhr = new XMLHttpRequest();
xhr.open('GET', '/server', true);

xhr.responseType = 'document';
// 强制执行 XML 解析
xhr.overrideMimeType('text/xml');

xhr.onload = function () {
  if (xhr.readyState === 4 && xhr.status === 200) {
    console.log(xhr.responseXML);
  }
};

xhr.send(null);
```

**xhr.reponseURL** 返回发送数据的服务器地址。它不一定和 xhr open 方法里面的指定的 URL 相同。如果服务端发生跳转，返回的是最后实际返回数据的网址。并且不会包括 hash 部分。

**xhr.status** 返回服务器响应的 HTTP 状态码。

- 200 OK 访问正常
- 301 Moved Permanently 永久移动
- 302 Moved temporarily 临时移动
- 304 Not Modified 未修改
- 307 Temporary Redirect 临时重定向
- 401 Unauthorized 未授权
- 403 Forbidden 禁止访问
- 404 Not Found 未发现指定资源
- 500 Internal Server Error 服务器发生错误

基本上，只有 2xx 和 304 的状态码，表示服务器返回是正常状态。也就是 200 到 300 间以及 304 表示资源接收正常

**xhr.statusText** 返回一个字符串，表示服务器发生的状态提示。包含整个状态信息。在请求发送前 也就是 open 方法之前，它的返回值是空字符串；如果服务器没有返回状态，它的值是默认值 OK。

**xhr.timeout、xhr.ontimeout** 毫秒级别的超时时间、超时事件的监听函数

**xhr 的其他事件监听函数**：xhr.onloadstart、xhr.onprogress、xhr.onabort、xhr.onerror、xhr.ontimeout、xhr.onloadend。

发生网络错误时，onerror 并不能获取错误信息。onpressgress 函数的事件对象参数，有三个属性：loaded 已经传输的数据量、total 数据总量、lengthComputable 返回是否可以计算加载进度

**xhr.withCredentials** 在跨域时，是否把用户信息（Cookie 和认证的 HTTP 头信息）包含在请求中。

如果需要跨域 AJAX 请求发送 Cookie，服务器端必须显式返回 Access-Control-Allow-Credentials 头信息，然后设置 withCredentials 为 true。

就算 withCredentials 设置为了 true，但是脚本依然是无法读取跨域的 Cookie 的。它只是针对 AJAX 对 HTTP 头信息的设置。

**xhr.upload** 发送文件以后，通过 xhr.upload 属性可以得到一个对象，通过监听这个对象的事件得知上传的进展。

```js
function upload(blobOrFile) {
  var xhr = new XMLHttpRequest();
  xhr.open('POST', '/server', true);
  xhr.onload = function (e) {};

  var progressBar = document.querySelector('progress');
  xhr.upload.onprogress = function (e) {
    if (e.lengthComputable) {
      progressBar.value = (e.loaded / e.total) * 100;
      // 兼容不支持 <progress> 元素的老式浏览器
      progressBar.textContent = progressBar.value;
    }
  };

  xhr.send(blobOrFile);
}

upload(new Blob(['hello world'], {type: 'text/plain'}));
```

**xhr.open()** 5 个参数：method、url、async 是否异步默认为 true、user 用于认证的用户名、password 用于认证的密码。如果对使用过 xhr open 方法的 AJAX 请求，再次使用 xhr.open()，等同于调用 abort()，终止请求。

**xhr.send()** 实际发出 HTTP 请求，参数是可选的，对应请求的类型 ArrayBufferView、Blob、Document、String、FormData。

**xhr.setRequestHeader()** 设置浏览器发送的 HTTP 请求头信息。必须在 open 方法之后，send 方法之前调用。多次设置同一个头信息，会合并成一个单一的值发送。

**xhr.overrideMimeType()** 指定 MIME 类型，覆盖服务器返回的真正的 MIME 类型。必须在 send 之前调用

**xhr.getResponseHeader()** 如果没收到服务器回应或者指定字段不存在，返回 null，该方法参数不区分大小写。

**xhr.getAllResponseHeader()** 

**Navigator.sendBeacon()** 用户卸载网页的时候，异步向服务器发数据。

```js
// HTML 代码如下
// <body onload="analytics('start')" onunload="analytics('end')">

function analytics(state) {
  if (!navigator.sendBeacon) return;

  var URL = 'http://example.com/analytics';
  var data = 'state=' + state + '&location=' + window.location;
  navigator.sendBeacon(URL, data);
}
```

