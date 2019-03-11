---
title: GlobalEventHandlers 接口
date: 2019-03-07 17:19:20
tags:
---


> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

指定事件的回调函数，推荐使用的方法是元素的 addEventListener 方法

```js
div.addEventListener('click', clickHandler, false);
```

除此之外，还有一种方法可以直接指定事件的回调函数

```js
div.onclick = clickHandler
```

这个接口是有 GlobalEventHandlers 提供的。它的优点是使用比较方便，缺点是只能为每个事件指定一个回调函数，而且无法指定事件触发的阶段（捕获阶段还是冒泡阶段）

HTMLElement、Document 和 Window 都继承了这个接口，也就是说所有的 HTML 元素、document 对象、window 对象上都可以使用 GlobalEventHandlers 接口提供的属性。

### onabort

### onerror

error 事件发生时，就会调用 onerror 属性指定的回调函数。

error 事件分成两种：

- 一种是 JavaScript 的运行时错误，这会传到 window 对象，触发 window.onerror

  ```js
  window.onerror = function(
  	message, // 错误信息字符串
    source, // 报错脚本的 URL
    lineno, // 报错的行号，整数
    colno, // 报错的列号，整数
    error // 错误对象
  ) {
    // ...
  }
  ```

- 资源加载错误，比如 `<img>` 或者 `<script>` 加载的资源出现加载错误。这时 Error 对象会传到对象的元素，导致该元素的 onerror 属性开始执行，一般来说，资源加载错误并不会触发 window.onerror

### onload、onloadstart

元素完成加载时 触发 load 事件，执行 onload。加载开始时触发 loadstart 事件，执行 onloadstart。

### onfocus、onblur

onfocus 在非输入类元素下，须有 tabindex 属性才能触发

### oncontextmenu、onshow

oncontextmenu 返回 false 等与禁止了右键菜单，而元素的右键菜单显示时会触发 onshow 监听函数

### 其他

拖动的事件属性分成两类：一类与被拖动元素相关，另一类与接收被拖动元素的容器元素相关。

被拖动元素的事件属性。

- ondragstart：拖动开始
- ondrag：拖动过程中，每隔几百毫秒触发一次
- ondragend：拖动结束

接收被拖动元素的容器元素的事件属性。

- ondragenter：被拖动元素进入容器元素。
- ondragleave：被拖动元素离开容器元素。
- ondragover：被拖动元素在容器元素上方，每隔几百毫秒触发一次。
- ondrop：松开鼠标后，被拖动元素放入容器元素。

`<dialog>`对话框元素的事件属性。

- oncancel
- onclose