---
layout: post
title: ArrayBuffer 对象与Blob 对象
date: 2019-05-06 14:11:59
tags:
---


> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

ArrayBuffer 表示一段二进制数据，用来模拟内存里面的数据。JavaScript 通过它来读写二进制数据。

```js
const buffer = new ArrayBuffer(8) // 8 表示 buffer 占用 8 个字节
buffer.byteLength // 可以通过 byteLength 获取占用的内存字节长
buffer.slice(0) // 相当于复制一份 buffer 内存
```

Blob （Binary Large Object）表示二进制文件的数据内容。与 ArrayBuffer 的区别是，Blob 操作二进制文件的数据内容，而 ArrayBuffer 用于操作内存。

```js
const b = new Blob(array[, options])
```

- array：成员是字符串或者二进制对象表示新生成的 Blob 实例对象的内容
- options：可选配置对象，有一个 type 属性表示数据的 MIME 类型

```js
const o = {a: 1}
const b = new Blob([JSON.stringify(o)], {type: 'applicaiton/json'})
b.size // 7 等价于 JSON.stringify(o).length
b.type // "application/json"
b.slice(start, end, contentType) // 切片 b 返回一个新的 Blob 实例
```

input file 返回 FileList 对象里的 File 实例对象是一个特殊的 Blob 实例，它除了有 size，type 属性，还有 name 和 lastModifiedDate 属性。拖放 API 的 dataTransfer.files 返回的也是 FileList 对象，它的 File 也是 Blob 实例。

AJAX 请求时，responseType 指定为 blob，那么 get 数据时，response 返回的数据类型就时 blob。

URL.createObjectURL() 针对 Blob 对象生成一个临时 URL，这个 URL 以 blob:// 协议开头，后跟一个识别符，用来唯一对应内存里面的 Blob 对象。与 data:// URL  ( URL 包含实际数据 ) 和 file:// URL ( 本地文件系统里面的文件 ) 都不一样。

```js
var droptarget = document.getElementById('droptarget');

droptarget.ondrop = function (e) {
  var files = e.dataTransfer.files;
  for (var i = 0; i < files.length; i++) {
    var type = files[i].type;
    if (type.substring(0,6) !== 'image/')
      continue;
    var img = document.createElement('img');
    img.src = URL.createObjectURL(files[i]);
    img.onload = function () {
      this.width = 100;
      document.body.appendChild(this);
      URL.revokeObjectURL(this.src);
    }
  }
}
```

浏览器处理 Blob URL 跟普通 URL 一样，如果 Blob 对象不存在，返回 404；如果跨域请求，返回 403。Blob URL 只对 GET 请求有效，如果请求成功，返回 200。由于 Blob URL 就是普通 URL，因此可以下载。

获取到 Blob 对象，可以通过 FileReader 对象读取 Blob 的内容。

- FileReader.readAsText()
- FileReader.readAsArrayBuffer()
- FileReader.readAsDataURL()
- FileReader.readAsBinaryString()

```js
// HTML 代码如下
// <input type="file" onchange="typefile(this.files[0])"></input>
function typefile(file) {
  // 文件开头的四个字节，生成一个 Blob 对象
  var slice = file.slice(0, 4);
  var reader = new FileReader();
  // 读取这四个字节
  reader.readAsArrayBuffer(slice);
  reader.onload = function (e) {
    var buffer = reader.result;
    // 将这四个字节的内容，视作一个32位整数
    var view = new DataView(buffer);
    var magic = view.getUint32(0, false);
    // 根据文件的前四个字节，判断它的类型
    switch(magic) {
      case 0x89504E47: file.verified_type = 'image/png'; break;
      case 0x47494638: file.verified_type = 'image/gif'; break;
      case 0x25504446: file.verified_type = 'application/pdf'; break;
      case 0x504b0304: file.verified_type = 'application/zip'; break;
    }
    console.log(file.name, file.verified_type);
  };
}
```

