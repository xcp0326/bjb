---
title: 一个页面从输入 URL 到页面加载显示完成，这个过程中都发生了什么？
category:
  - 面试题
date: 2019-01-07 11:51:20
tags:
---

![](/images/url-view/tcp.png)

第一步：根据域名进行 DNS 解析到 IP 地址

第二步：根据 IP 地址向服务器发起 TCP 连接，与浏览器进行三次握手：

- 主机向服务器发送一个建立连接的请求
- 服务器接到请求后发送同意连接的信号
- 主机到同意连接的信号后，再次向服务器发送了确认信号

第三步：发送 HTTP 请求，请求报文包括三部分：请求行、请求头和请求正文

第四步：服务器处理请求并返回 HTTP 响应报文，响应报文包括三部分：响应状态码、响应报头和响应报文。

如果该资源浏览器访问过，而且该资源的 response header 的 cache-control 有效时间内，就不再访问服务器，而是直接从本地读取，如果没有 cache-control 而有 expires，而且本地时间在 expires 时间之前，也不再访问服务器，也是直接从本地读取。expires 和 cache-control，cache-control 优先级更高，所以先访问 cache-control，它俩是 HTTP 强缓存。

如果 HTTP 强缓存都已经失效，则访问协商缓存，浏览器会携带缓存标识 Etag 和 Last-Modified 发送请求，如果 If-None-Match 值与浏览器携带的 Etag 值一致则返回 304，不用再请求服务器资源，缓存有效。如果失效，则看 Last-Modified-Sine 与浏览器携带的 Last-Modified 是否一致，如果一致，则返回 304 不再请求服务器资源。

第五步：解析并渲染页面

- HTML 开始构建一个 DOM 树
- 如果遇到的 DOM 节点是 JavaScript 代码，就会调用 JavaScript 引擎对 JavaScript 代码进行解释执行，此时由 JavaScript 引擎和 GUI 渲染线程的互斥，GUI 渲染线程就会被挂起，渲染过程停止；如果 JavaScript 代码的运行中对 DOM 树进行了修改，那么 DOM 的构建需要从新开始
- 如果节点需要依赖其他资源，如（图片，CSS等），便会调用网络模块的资源加载器来加载它们，但它们是异步的，不会阻塞当前DOM树的构建
- 如果遇到的是 JavaScript 资源 URL（没有标记异步），则需要停止当前 DOM 的构建，直到 JavaScript 的资源加载并被 JavaScript 引擎执行后才能继续构建 DOM
- 对于 CSS，CSS 解释器会将 CSS 文件解释成内部表示结构，生成 CSS 规则树；
- 然后合并 CSS 规则树和 DOM 树，生成渲染树
- 最后对渲染树树进行布局和绘制

第六步：通过四次挥手关闭 TCP：

- 主机向服务器发送一个断开的请求
- 服务器接到请求后发送确认收到请求的信号
- 服务器向主机发送断开通知
- 主机接到断开通知后断开连接并反馈一个确认信号，服务器收到取人信号后断开连接。

### 其他

[从输入 URL 到页面加载完成的过程中都发生了什么事情？](http://fex.baidu.com/blog/2014/05/what-happen/)
[经典面试题：从 URL 输入到页面展现到底发生什么？
](https://blog.fundebug.com/2019/02/28/what-happens-from-url-to-webpage/)