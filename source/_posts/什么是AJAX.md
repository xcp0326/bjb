---
title: 什么是 AJAX
tags:
  - AJAX
date: 2019-01-15 16:29:55
---


### 如何发请求

- form 通过 action 发请求，不过会新开一个页面
- a 标签可以通过 href 发请求，不过会刷新页面或者新开一个页面
- img 标签可以通过 src 发请求，不过会返回一个图片
- link 标签可以通过 href 发请求，不过只能以 CSS 展示结果
- script 标签可以通过 src 发请求，通过 SRJ （Server Render JavaScript）的方式实现 JSONP，不过会返回一个 script。

### AJAX 是异步的 JavaScript 和 XML

使用 XMLHttpRequest 发请求，服务器返回 XML 格式的字符串，JavaScript 解析 XML 并更新局部页面。

XMLHttpRequest 的 onreadystatechange 方法，readyState、status、responseText 属性

### 从 XML 到 JSON

从 XML 到 道格拉斯·克罗克福特（出书《JavaScript 语言精粹》）发明的数据交换语言 JSON。JSON 是抄袭 JS，但是是一门新语言，而它的作者还出了一本书专门说 JS 的不好的地方，😂 他与 JS 之父从没一起出场过。

JSON vs JS

1. JSON 没有抄袭 function 和 undefined
2. JSON 字符首尾必须是双引号

| JS                      | JSON                                |
| ----------------------- | ----------------------------------- |
| undefined               | 没有                                |
| null                    | null                                |
| ['a', 'b']              | ["a","b"]                           |
| function fn() {}        | 没有                                |
| 'frank'                 | "frank"                             |
| var a = {}; a.self = a; | 搞不定，JSON 没有变量，无法实现引用 |
| `{__proto__}`           | 没有原型                            |

HTTP response 第 4 部分永远都是字符串，AJAX 一般返回符合 JSON 对象语法的字符串。

```js
var xhr = new XMLHttpRequest()
xhr.onreadystatechange = function() {
  if (xhr.readyState === 4) {
    if (xhr.status >= 200 && xhr.status < 300) {
			var data = xhr.responseText
      console.log(JSON.parse(data))
      console.log('成功')
    } else if (xhr.status >= 400) {
      console.log('失败')
    }
  }
}
// 一个异步的 GET 请求
xhr.open('get', '/xxx')
xhr.send()
```

### AJAX 的同源策略与 CORS 跨域

form 发请求可以跨域，因为是跳转到其他页面，它是不会修改页面里面的内容。但是 AJAX 不可以！因为 AJAX 不会跳转，它是可以修改页面里面的元素的。

同源策略：协议 + 域名 + 端口 一模一样才允许发 AJAX 请求

设置 Access-Cross-Allow-Origin 为其他域名，就可以让其他域名访问当前服务器的数据。

Cross Origin Resource Sharing 跨站资源共享 CORS

### JS 操作请求与响应

JS 可以设置任意请求的 header

- 设置请求 header 的第一部分 xhr.open('GET', '/xxx')
- 设置请求 header 的第二部分 xhr.setHeader('content-type', 'x-www-form-urlencoded')
- 设置请求 header 的第四部分 xhr.send('a=1&b=2')

JS 可以获取任意响应 header

- 获取响应 header 的第一部分 xhr.status / xhr.statusText
- 获取响应 header 的第二部分 xhr.getResponseHeader() / xhr.getAllResponseHeaders()
- 获取响应 header 的第四部分 xhr.responseText

