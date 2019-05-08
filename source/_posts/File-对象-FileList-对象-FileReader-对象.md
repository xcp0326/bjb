---
layout: post
title: File 对象、FileList 对象、FileReader 对象
date: 2019-05-08 13:20:57
tags:
---


> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

File 继承了 Blob 对象。是一种特殊的 Blob 对象，所有可以使用 Blob 对象的场合都可以使用它。比如表单的上传文件 input。

```js
new File(array, name [, options])
```

- array：一个数组，成员可以是二进制对象或字符串，表示文件的内容
- name：字符串，表示文件名或文件路径
- options：可选，配置对象，设置实例的属性。
  - type：表示实例对象的 MIME 类型 字符串，默认为空
  - lastModified：时间戳，表示上次修改的事件，默认为 `Date.now()`

```js
const f = new File(
	['foo'],
  'foo.txt',
  {
    type: 'text/plain'
  }
)
```

FileList 对象是一个类似数组的对象，代表一组选中的文件，每个文件就是一个 File 实例。主要场景：input[type="file"]，拖拉一组文件时，目标区的 DataTransfer.files 属性，返回一个 FileList 实例。

FileReader 对象用于读取 File 对象或者 Blob 对象所包含的文件内容。

- FileReader.prototype.error
- FileReader.prototype.readyState：读取文件时的当前状态：0，未加载认为数据；1，表示正在加载数据；2，表示加载完成。与 XMLHttpRequeset.readyState 状态不一样，XMLHttpRequest.readyState：0，代理被创建，还没有调用 `open()` 方法；1，`open()` 方法已经被调用；2，`send()` 方法已经被调用，并且头部和状态已经可获得；3，下载中，responseText 属性已经包含部分数据；4，下载操作已经完成。
- FileReader.prototype.result：读取到的文件内容，返回的是字符串或者 ArrayBuffer 实例
- FileReader.prototype.onabort
- FileReader.prototype.onerror
- FileReader.prototype.onload：通常在这个函数里面使用 result 属性，拿到文件内容
- FileReader.prototype.onloadstate
- FileReader.prototype.onloadend
- FileReader.prototype.onprogress

```js
// HTML 代码如下
// <input type="file" onchange="onChange(event)">

const onchange = (e) => {
  const file = e.target.files[0]
  const reader = new FileReader()
  reader.onload = (e) => {
    console.log(e.target.result)
  }
  reader.readerAsText(file)
} 
```

- FileReader.prototype.abort()：终止读取操作，readyState 属性将变成 2
- FileReader.prototype.readAsArrayBuffer()：以 ArrayBuffer 的格式读取文件，读取完成后 result 属性将返回一个 ArrayBuffer 实例
- FileReader.prototype.readAsBinaryString()：读取完成后，result 属性将返回一个二进制字符串
- FileReader.prototype.readAsDataURL()：读取完成后，result 属性返回一个 Data URL 格式（Base64 编码）的字符串，代表文件内容。对于图片文件，这个自发可以用于 `<img>` 元素的 src 属性。注意这个字符串不能直接进行 Base64 解码，必须把前缀 `data:*/*;base64,` 从字符串删除后，再进行解码
- FileReader.readAsText()：读取完成后，result 属性返回一个文件内容的文本字符串。该方法的第一个参数是代表文件的 Blob 实例，第二个参数可选，表示文本编码，默认是 UTF-8.