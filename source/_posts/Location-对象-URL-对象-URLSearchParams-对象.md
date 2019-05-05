---
layout: post
title: Location 对象 URL 对象 URLSearchParams 对象
date: 2019-05-05 16:02:54
tags:
---


> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

window.location 和 document.location 都可以拿到 Location 对象。

- location.href：`http://user:passwd@www.example.com:4097/path/a.html?x=111#part1`
- location.protocol
- location.host：主机，包括冒号和端口号。例如：`www.example.com:4097`
- location.hostname：主机名，不包括冒号和端口号。例如：`www.example.com`
- location.search
- location.hash
- location.username
- location.password
- location.origin 只读。URL 的协议、主机名和端口号。例如：`http://user:passwd@www.example.com:4097`

- location.assign() vs location.href：跳转到参数所在的 URL
- location.replace(`<URL>`) vs history.replaceState(state, title, url)
- location.reload()

### URL 的编码和解码

> URL vs URI：
> URL 是 URI 的一个实现。
> URI 是统一资源标识符，URL 是统一资源定位符，URN 统一资源名称。
> URI 包含 URN 和 URL。

- URL 元字符：分号，逗号，斜杆，问号，冒号，at，`&`，等号，加号，美元符号，井号
- 语义字符：a-z、A-Z、0-9、连词字、下划线、点、感叹号、波浪线、星号、单引号、圆括号

其他都必须转移，规则是根据操作系统的默认编码，将每个字节转为百分号加上对应的十六进制，如果十六进制包含字母，字母大写。

> 查看操作系统的编码的命令是 locale
>
> ![](/images/system-locale.png)

- encodeURI(`URL`)  / decodeURL(`URL`) ：转义 / 解码元字符和语义字符之外的字符
- encodeURLComponent(`URL|URL 的组成部分`) / decodeURLComponent(`URL|URL 的组成部分`)：转义 / 解码语义字符之外的字符

### URL 对象

URL 对象是浏览器的原生对象，可以用来构造、解析和编码 URL。通过 window.URL 可以拿到这个对象。

`<a>` 和 `<area>` 上有这个接口。它们的 DOM 节点对象可以使用 URL 的实例属性和方法。

> DOM 节点 vs Element：Element 只是节点中的一种类型，DOM 节点除了 Element（元素节点），还有文本节点，注释节点，文档节点等等。

```js
var a = document.createElement('a');
a.href = 'http://example.com/?foo=1';

a.hostname // "example.com"
a.search // "?foo=1"
```

### URL 对象是一个构造函数

```js
const url = new URL(urlString, [baseURLString])
const url = new URL(urlString, baseURLobject)
```

urlString 为 `..`，则表示上层路径：

```js
const url3 = new URL('..', 'http://example.com/a/b.html')
url3.href
// "http://example.com/"
```

### URL 的静态方法

- **URL.createObjectURL()** 用来为上传/下载的文件、流媒体文件生成一个 URL 字符串。这个字符串代表了 File 对象或 Blob 对象的 URL。

```js
// HTML 代码如下
// <div id="display"/>
// <input
//   type="file"
//   id="fileElem"
//   multiple
//   accept="image/*"
//   onchange="handleFiles(this.files)"
//  >
const div = document.getElementById('display')
const handleFiles = files => {
  for (let i = 0; i < files.length; i++) {
    const img = document.createElement('img')
    img.src = window.URL.createObjectURL(files[i])
    div.appendChild(img)
  }
}
```

该方法生成的 URL 就像下面这样：

```js
blob:http://localhost/c745ef73-ece9-46da-8f66-ebes574789b1
```

由于每次使用 URL.createObjectURL 都会在内存中生存一个 URL 实例。如果不再需要改方法生成的 URL 字符串，可以使用 URL.revokeObjectURL 方法释放掉所占的内存。

- **URL.revokeObjectURL** 

所以上面 handleFiles 可以优化为：

```js
const handleFiles = files => {
  for (let i = 0; i < files.length; i++) {
    const img = document.createElement('img')
    img.src = window.URL.createObjectURL(files[i])
    div.appendChild(img)
    img.onload = () => window.URL.revokeObjectURL(this.src)
  }
}
```

### URLSearchParam 对象

浏览器原生对象，用来构造、解析、处理 URL 的查询字符串。

它是一个构造函数，参数可以是参训字符串，也可以是查询字符串的数组或对象。

```js
// 方法一：传入字符串
var params = new URLSearchParams('?foo=1&bar=2');
// 等同于
var params = new URLSearchParams(document.location.search);

// 方法二：传入数组
var params = new URLSearchParams([['foo', 1], ['bar', 2]]);

// 方法三：传入对象
var params = new URLSearchParams({'foo' : 1 , 'bar' : 2});
```

浏览器向服务器发送表单数据时，可以直接使用`URLSearchParams`实例作为表单数据。

```js
const params = new URLSearchParams({foo: 1, bar: 2});
fetch('https://example.com/api', {
  method: 'POST',
  body: params
}).then(...)
```

URL 实例的`searchParams`属性就是一个`URLSearchParams`实例，所以可以使用`URLSearchParams`接口的`get`方法。

```js
var url = new URL(window.location);
var foo = url.searchParams.get('foo') || 'somedefault';
```

DOM 的`a`元素节点的`searchParams`属性，就是一个`URLSearchParams`实例。

```js
var a = document.createElement('a');
a.href = 'https://example.com?filter=api';
a.searchParams.get('filter') // "api"
```

`URLSearchParams`实例有遍历器接口，可以用`for...of`循环遍历

- URLSearchParams.toString()：返回实例的字符串形式
- URLSearchParams.append(key, value)：追加查询参数。不过不会识别是否有同名查询参数
- URLSearchParams.delete(key)：删除键名为 key 的查询参数
- URLSearchParams.has(key)
- URLSearchParams.set(key, value)：设置 key 的查询参数的值为 value
- URLSearchParams.get('key')
- URLSearchParams.getAll('key')
- URLSearchParams.sort()：对查询字符串的键按照 Unicode 码点进行排序
- URLSearchParams.keys()、URLSearchParams.values()、URLSearchParams.entries() 返回遍历器