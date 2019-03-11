---
title: EventTarget 接口
tags:
  - DOM
  - 事件
  - 发布订阅
categories:
date: 2019-02-03 11:28:29
---

> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

事件本质是程序各个组成部分之间的一种通信方式，也是异步编程的一种实现。

DOM 的事件操作（监听和触发）都定义在 EventTarget 接口。所有节点对象都部署了这个接口 比如一个 div 节点元素：

HTMLDivElement 继承自 HTMLElement
  => HTMLElement 继承自 Element
    => Element 继承自 Node
      => Node 继承自 EventTarget
        => EventTarget 继承自 Object 原型链终点

还有其他一些需要事件通信的浏览器内置对象 比如 XMLHttpRequest 也部署了这个接口：

XMLHttpRequest 继承自 XMLHttpRequestEventTarget
  => XMLHttpRequest 继承自 EventTarget
    => EventTarget 继承自 Object 原型链终点

EventTarget 接口的三个实例方法：

- addEventListener 添加事件侦听器
- removeEventListener 移除事件侦听器
- dispatchEvent （IE 的 fireEvent，jQuery 的 trigger() 方法）触发指定事件

### addEventListener 添加事件侦听器

```js
target.addEventListener(eventType, listener[, useCapture])
target.addEventListener(eventType, listener[, options])
```

- useCapture：true 表示捕获，false 表示冒泡，默认为 false 冒泡
  ![Graphical representation of an event dispatched in a DOM tree using the DOM event flow](https://www.w3.org/TR/DOM-Level-3-Events/images/eventflow.svg)

- listener：是指实现了 EventListener 接口的对象或者说一个函数

  - 一个 EventListener 接口对象的例子：EventListener 下须有且只有 handleEvent() 函数

    ```js
    buttonElement.addEventListener('click', {
      handleEvent: function (event) {
        alert('Element clicked through handleEvent property!');
      }
    });
    ```

- options：是一个属性配置对象，`{capture: true, once: true, passive: true}`。capture 是否在捕获阶段触发事件；once 表示监听事件只会触发一次，然后自动移除；passive 表示 listener 不会调用事件的 preventDefault 方法，如果 listener 调用了这个函数，客户端将会忽略它并抛出一个控制台警告。

### removeEventListener 移除事件侦听器

移除 addEventListener 方法添加的事件侦听器，参数必须与 addEventListener 完全一致，节点也必须是同一个。

### dispathEvent 触发事件

在节点上触发指定事件（事件必须是 Event 对象实例），从而触发 listener 的执行。返回一个布尔值，如果 listener 调用了 event.preventDefault()，返回 false，否则 true。

可以根据返回值判断事件是否被取消了：

```js
var canceled = !cb.dispatchEvent(event);
if (canceled) {
  console.log('事件取消');
} else {
  console.log('事件未取消');
}
```

### 事件侦听器 vs 事件处理器

- 事件侦听器是通过 EventTarget.addEventListener 注册的函数，一个事件可以注册多个，在 eventloop 时候，会先后执行。
- 事件处理器是通过 on... 属性注册的函数，一个事件只能注册一个，后注册的会覆盖先注册的函数。

```js
div.addEventListener('click', function() {alert(1)}, false)
div.addEventListener('click', function() {alert(2)}, false)
div.onclick = function() {alert(3)}
div.onclick = function() {alert(4)}
const e = new Event('click')
div.dispatchEvent(e) // 依次 alert 1，2，4
```

