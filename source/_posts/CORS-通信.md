---
layout: post
title: CORS 通信
date: 2019-04-12 00:00:55
tags:
---


> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

CORS 是一个 W3C 标准，全称是"跨域资源共享"
CORS 需要浏览器和服务器同时支持。整个 CORS 通信过程，都是浏览器自动完成。浏览器实现跨域请求，然后服务器实现 CORS 接口。

CORS 的请求分为两类：简单请求（simple request）和非简单请求（not-so-simple request）。

只要同时满足两大条件，就属于简单请求：

1）请求方法：GET、HEAD、POST

2）HTTP 的头信息不超出几种字段：Accept、Accept-Language、Content-Language、Last-Event-ID、Content-Type 是 application/x-www-form-urlencoded、multipart/form-data、text/plain。

简单请求就是表单请求，浏览器沿袭了传统的处理方式，不把行为复杂化，否则开发者可能转为使用表单，规避 CORS 的限制。

对于简单请求，浏览器会直接发出 CORS 请求，在头信息之中，增加一个 Origin 字段。如果 Origin 指定的源，不在许可范围内，服务器会返回一个正常的 HTTP 回应。
如果回应头信息没有包含 Access-Control-Allow-Origin 字段，会抛出一个错误，在 XMLHttpRequest 的 onerror 回调函数里面可捕获到。注意
这种错误无法通过状态码识别，因为 HTTP 回应的状态码有可能是 200。

如果 Origin 指定的域名在许可范围内，服务器返回的响应，会多几个头信息字段：Access-Control-Allow-Origin、Access-Control-Allow-Credentials、
Access-Control-Expose-Headers、Content-Type。

1）Access-Control-Allow-Origin：必选字段，它的值要么与请求时的 Origin 字段的值一致，要么是一个 `*`。

2）Access-Control-Allow-Credientials：可选字段，表示是否允许发送 Cookie。默认情况下，Cookie 不包括在 CORS 请求之中。设为 true，即
表示服务器明确许可，浏览器可以把 Cookie 包含在 CORS 请求中一起发给服务器。这个值也只能设为 true，如果服务器不要浏览器发送 Cookie，不发送该字段即可。然后 AJAX 请求的 withCredentials 设置为 true。如果服务器要求浏览器发送 Cookie，
Access-Control-Allow-Origin 就必须指定明确的、与请求网页一致的域名，为了降低 CSRF 攻击的风险。

3）Access-Control-Expose-Headers：可选字段，CORS 请求时，XMLHttpRequest 对象的 getResponseHeader() 方法只能拿到
 6 个服务器返回的基本字段：Cache-Control、Content-Language、Content-Type、Expires、Last-Modified、Pragma。如果
 想拿到其他字段，就必须在 Access-Control-Expose-Headers 里面指定。
 
### 非简单请求

非简单请求是那种对服务器提出特殊要求的请求，比如请求方法是 PUT 或者 DELETE，或者 Content-Type 字段的类型是 
application/json。

预检请求 preflight：浏览器先询问服务器，当前网页所在的域名是否在服务器的许可名单中、以及可以使用哪些 HTTP 动词和头信息
字段，只有得到肯定答复，浏览器才会发出正式的 XMLHttpRequest 请求，否则就就报错。这是喂了防止这些新增的请求，对传统的没有 CORS 支持的服务器形成压力，给服务器一个提前拒绝的机会，这样可以防止服务器收到大量的 DELETE 和 PUT 请求，这些传统的表单不
可能跨域发出的请求。

预检请求的头信息里面包含字段 Origin、Access-Control-Request-Method、Access-Control-Request-Headers。

预检请求的回应没有任何 CORS 相关头信息或者明确表示请求不符合条件，XMLHttpRequest 对象的 onerror 回调函数可以捕获到错误。

CORS 相关字段：Access-Control-Allow-Methods、Access-Control-Allow-Headers、Access-Control-Max-Age。

一旦服务器通过了 预检请求，以后每次浏览器正常的 CORS 请求，就跟简单请求一样，会有一个 Origin 头信息字段。服务器的响应
也都会有一个 Access-Control-Allow-Origin 字段。