---
layout: post
title: img 标签
date: 2019-06-03 23:29:34
tags:
---


img 元素用于插入图片，主要继承了 HTMLImageElement 接口。浏览器的元素构造函数 Image 用于生成 HTMLImageElement 实例。

```js
var img = new Image(width, height)
img instanceof Image; // true
img instanceof HTMLImageElement; // true
```

HTMLImageElement.isMap 返回图像是否为服务器端的图像映射的一部分。

HTMLImageElement.useMap 表示当前图像对应的 map 元素

HTMLImageElement.complete

HTMLImageElement.crossOrigin

HTMLImageElement.natureWidth

HTMLImageElement.referrerPolicy 表示请求图像资源时，如果处理 HTTP 请求的 referrer 字段。对应有 5 个可能的值：no-referrer、no-referrer-when-downgrade、origin、origin-when-cross-origin、unsafe-url。

