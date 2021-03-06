---
layout: post
title: Cookie
date: 2019-03-14 22:30:48
tags:
---


> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

Cookie 是服务器保存在浏览器的一小段文本信息，每个 Cookie 的大小一般不超过 4KB。浏览器每次向服务器发出请求，就会自动附上这段信息。

Cooke 主要用来分辨两关请求是否来自同一个浏览器，以及用来保存一些状态信息。它的常用场合有以下一些：

- 会话（session）管理：保存登录、购物车等需要记录的信息
- 个性化：保存用户的偏好，比如网页的字体大小、背景色等
- 追踪：记录和分析用户行为

有些开发者使用 Cookie 作为客户端存储。这样做虽然可行，但是不推荐，因为 Cookie 的设计目的并不是这个，它的容量太小了，只有 4KB，而且也缺乏数据操作接口，还会影响到性能。客户端存储应该使用 Web storage API 和 IndexDB。

Cookie 包含以下几方面的信息：

- Cookie 的名字
- Cookie 的值（真正的数据写在这里面）
- 到期时间
- 所属域名（默认是当前域名）
- 生效的路径（默认是当前网址）

举例来说，用户访问网址 `www.example.com`，服务器在浏览器写入一个 Cookie。这个 Cookie 就会包含 `www.example.com` 这个域名，以及根路径 `/`。这意味着，这个 Cookie 对该域名的根路径和它所有子路径都有效。如果路径设置为 `/forums`，那么这个 Cookie 就只有在访问 `www.example.com/forums` 及其子路径有效。以后，浏览器一旦访问这个路径，浏览器就会附上这段 Cookie 发送给服务器。

浏览器可以设置为不接受 Cookie。开发者可以通过 `window.navigator.cookieEnable` 确定用户是否开启 Cookie。

`document.cookie` 返回当前网页的 Cookie。

不同浏览器对 Cookie 数量和大小的限制不一样的，一般说来，单个域名设置的 cookie 不应超过 30 个，每个 Cookie 的大小不超过 4KB。超过限制以后，Cookie 将被忽略，不会被设置。

浏览器的同源策略规定，两个网址只要域名和端口相同，就可以共享 Cookie。不需要协议相同。

### Cookie 与 HTTP 协议

Cookie 由 HTTP 协议生成，也主要是供给 HTTP 协议使用

#### HTTP 响应：Cookie 的生成

服务器如果希望在浏览器保存 Cookie，就要在 HTTP 响应的头信息里面，放置一个 Set-Cookie 字段。

HTTP 响应可以包含多个 Set-Cookie 字段，即在浏览器生成多个 Cookie，除了 Cookie 的值，Set-Cookie 字段还可以附加 Cookie 的属性。

如果服务器想修改已经设置的 Cookie，必须同时满足 4 个条件：Cookie 的 key、domain、path 和 secure 都匹配，才能修改。如果有一个不一样，就会生成一个全新的 Cookie。两个同名的 Cookie，匹配越精确的 Cookie 排名越靠前。

#### HTTP 请求：Cookie 的发送

浏览器向服务器发送 HTTP 请求时，每个请求都会带上相应的 Cookie。也就是说，把服务器保存在浏览器的那段 Cookie 再发送回服务器。这时要使用 HTTP 头信息的 Cookie 字段。

如果 Cookie 字段包含多个 Cookie，它们会用 分号 分隔。

服务器收到浏览器发来的 Cookie 时，有两点时无法知道的：

- Cookie 的各种属性，比如何时过期
- 哪个域名设置的 Cookie，到底是一级域名还是某个二级域名

### Cookie 的属性

Expires 是指具体的到期 UTC 格式时间，到了指定时间后，浏览器就不再保存这个 Cookie。浏览器根据本地时间，决定 Cookie 是否过期，由于本地时间是不精确的，所以没有办法保证 Cookie 一定会在服务器指定的时间过期。所以就有了 Max-Age。

Max-Age 指定的是从现在开始 Cookie 存在的秒数，比如 `60*60*24*365`，过了这个时间以后，浏览器就不再保留这个 Cookie。Max-Age 的优先级更高。如果 Set-Cookie 没有指定 Expires 或者 Max-Age 属性，那么这个 Cookie 就是 Session Cookie，只在当前会话有效，一旦用户关闭浏览器，浏览器就不会再保存这个 Cookie。

Domain 属性指定浏览器发出 HTTP 请求时，哪些域名要附带这个 Cookie。没指定的话，就是当前域名，这时子域名将不会附带这个 Cookie。比如 example.com 不设置 Cookie 的 domain 属性，那么 sub.example.com 就不回附带这个 Cookie。如果指定了 domain 属性，那么子域名就会附带这个 Cookie。如果服务器指定的域名不属于当前域名，浏览器就会拒绝这个 Cookie。

Path 指定浏览器发出 HTTP 请求时，哪些路径要附带这个 Cookie。只要浏览器发现 Path 属性时 HTTP 请求路径的开头的一部分，就会在头信息里面带上这个 Cookie。比如 PATH 属性时 `/`，那么请求`/docs` 路径也会包含该 Cookie，当前前提是域名必须一致。

Secure 属性指定浏览器只有在加密协议 HTTPS 下，才能将这个 Cookie 发送到服务器。另一方面，如果当前协议是 HTTP，浏览器就会自动忽略服务器发送来的 Secure 属性。该属性只是一个开关，不需要指定值。如果通信协议是 HTTPS 协议，该开关自动打开。

HttpOnly 属性指定该 Cooke 无法通过 JavaScript 脚本拿到，主要是 document.cookie 属性、XMLHttpRequest 对象和 Request API 都拿不到这个属性。这样就防止了该 Cookie 被脚本读到，只有浏览器发出 HTTP 请求时才会带上该 Cookie。

### document.cookie

读写当前网页的 Cookie。读取的时候，返回当前网页所有的非 HttpOnly 属性的 Cookie。多个 Cookie，返回的时候用分号隔开。写入的时候，Cookie 的值必须是 `key=value` 的形式。等号两边没有空格。这个值的分号、空格和逗号必须进行转义。document.cookie 一次可以读出全部 Cookie，但是只能写入一个 Cookie。这与 HTTP 协议的 Cookie 通信格式有关。浏览器向服务器发送 Cookie 的时候， Cookie 字段是使用一行将所有 Cookie 全部发送；服务器向浏览器设置 Cookie 的时候，Set-Cookie 字段是一行设置一个 Cookie。

写入 Cookie 时：

- path 属性必须为绝对路径，默认为当前路径
- domain 属性必须是当前发送 Cookie 的域名的一部分。比如当前域名是 example.com，就不能将其设置为 foo.com。该属性默认为当前的一级域名（不含二级域名）。
- max-age 秒数值后过期
- expires UTC 格式的时间点过期

Cookie 的这些属性值不可读，只可写。设置完了没有办法读取到。

删除一个现存的 Cookie 的唯一方法是设置它的 expires 属性为一个过去的日期。