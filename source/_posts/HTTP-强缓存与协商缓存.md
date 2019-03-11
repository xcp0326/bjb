---
title: HTTP 强缓存与协商缓存
tags:
  - HTTP
date: 2019-01-07 14:11:54
---

### HTTP 报文

HTTP 缓存机制是根据 HTTP 报文的缓存标识定义的。而HTTP 报文分为 HTTP 请求报文 和 HTTP 响应报文。

HTTP 请求报文格式为：

1 请求行
2 通用头信息
2 请求头
2 实体头
3 
4 请求报文体

HTTP 响应报文对应为：

1 状态行
2 通用信息头
2 响应头
2 实体头
3
4 响应报文体

通用信息头一般包含：Cache-Control、Connection、Date
实体头是实体信息的实体头域常见的有：Connection、Keep-Alive、Contect-Type、Etag、Expires、Last-Modified、extension-header、Content-Encoding、Content-Range

### 强缓存

强缓存：向浏览器缓存查找该请求结果，并根据该结果的缓存规则来决定是否使用该缓存结果的过程。

强缓存是根据客户的保留的服务器端的 response header 中的字段：expires、cache-control 来控制的。

- cache-control 是过期的一个时间长度，expires 是过期的时间点。cache-control 的优先级高于 expires。在无法确定客户端时间是否与服务器端时间同步的情况下，cache-control 相比 expires 是更好的选择，所以如果同时存在时，只有 cache-control 会生效。
- Expires 是 HTTP/1.0 控制网页缓存的字段，其值为服务器返回请求结果缓存的到期时间，即再次发起该请求时，如果客户端的时间小于 expires 值，则直接返回缓存结果。

Cache-Control 是在 HTTP/1.1 用于控制网页缓存的字段，常见值：

- public：所有内容都缓存 客户端和代理服务器都可缓存（一般都是设置为这个）
- private：所有内容都缓存 客户端可缓存，默认值
- no-cache：客户端缓存内容，但是是否使用缓存则需要经过协商缓存来验证决定
- no-store：所有内容都不缓存，既不强制缓存也不协商缓存
- max-age=xxx：缓存在 xxx 秒后失效

缓存失效时间计算公式：expirationTime = responseTime + freshnessLifetime - currentAge

### 强缓存下的内存缓存与硬盘缓存

内存缓存：内存缓存会将变异解析后的文件，直接存入该进程的内存中，占据该进程一定的内存资源，以方便下次运行使用时的快速读取，但一旦关闭该进程，该缓存就没有了。

硬盘缓存：是直接写入硬盘文件中的，读取缓存需要对该缓存存放的硬盘文件进行 I/O 操作，然后重新解析该缓存内容，读取复杂，速度显然会比内存缓存慢

在浏览器中，浏览器会在 JS 和图片等文件解析执行后直接存入内存缓存中，当刷新页面时只需直接从内存缓存中读取，而 CSS 文件则会存入硬盘文件中，所以每次渲染页面都需要从硬盘缓存中读取。

### 协商缓存

协商缓存：在强制缓存失效后，浏览器携带缓存标识向服务器发送请求，由服务器根据缓存标识决定是否使用缓存的过程。

协商缓存的标识 response header 中的字段：Last-Modified / If-Modified-Since、Etag / If-None-Match 来控制的。

Etag / If-None-Match 的优先级高于 Last-Modified / If-Modified-Since。

Last-Modifed 是服务器响应请求时，返回该资源文件在服务器最后被修改的时间

Etag 是服务器响应请求时，返回当前资源文件的一个唯一标识（有服务器生成），客服端再次发起请求时，If-None-Match 携带上次请求返回的唯一标识 Etag 值，If-None-Match 与该资源在服务器的 Etag 值做比较，一致则返回 304，代表资源无更新，继续使用缓存文件；不一致则重新返回资源文件，状态码 200

### 小总结

协商缓存需要配合强缓存使用，你看前面这个截图中，除了Last-Modified这个header，还有强缓存的相关header，因为如果不启用强缓存的话，协商缓存根本没有意义。

强制缓存不会发请求到服务器，而协商缓存会发请求到服务器，所以说它两都不会从服务器加载资源数据。

### 浏览器缓存过程

浏览器第一次加载资源，服务器返回 200，浏览器将资源文件从服务器上请求下载下来，并把 response header 及该请求的资源一并缓存；也就是说缓存命中的请求返回的 response header 并不是来自服务器，而是来自之前的缓存的 response header；

浏览器再次请求这个资源的时候，会先从缓存中旋转这个资源的 response header，比较当前时间和上一次返回 200 的时间差，如果没有超过 cache-control 设置的 max-age，则没有过期，命中强制缓存，不发请求直接从本地缓存读取该文件（如果浏览器不支持 HTTP 1.1，则用 expires 设置的时间点判断是否过期）；如果过期，则向服务器发送 header 带有 If-None-Match 和 If-Modified-Since 的请求；

服务器收到请求后，优先根据 Etag 的值判断被请求的文件有没有被修改，Etag 与 If-None-Match 的值一致则表示没有修改，命中协商缓存，返回 304。如果不一致则有改动，直接返回新的资源文件带上新的 Etag 的 response header 并返回 500。

如果服务器收到的请求没有 Etag 值，则将 If-Modified-Since 和被请求文件的最后修改时间进行比较，一致则命中协商缓存，返回 304；不一致则返回新的 last-Modified 的 response header 和文件并返回 200

### 用户行为对浏览器缓存的控制

- 地址栏访问，链接跳转是正常的用户行为，都会触发浏览器缓存机制
- F5 刷新，浏览器会设置 max-age=0，跳过强缓存判断，进行协商缓存判断
- ctrl + F5 刷新，跳过强缓存和协商缓存，直接从服务器下载资源