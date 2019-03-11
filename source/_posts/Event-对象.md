---
title: Event 对象
category:
date: 2019-02-14 11:35:55
tags:
---

> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

事件发生后，会产生一个事件对象，作为参数传给监听函数。浏览器原生提供一个 Event 对象，所有的事件都是这个对象的实例。

```js
event = new Event(type, options)
```

options 是一个对象，表示事件对象的配置，有两个属性：

- bubbles：布尔值，可选，默认为 false，表示事件对象是否冒泡。
- cancelable：布尔值，可选，默认为 false，表示事件是否可以被 Event.preventDefault() 取消，一旦事件被取消，就不会触发浏览器对该事件的默认行为。

注意：如果没有显式指定 bubbles 属性为 true，生成的事件就只能在捕获阶段触发监听函数。

### Event.bubbles、Event.eventPhase

Event.bubbles 属性返回一个只读布尔值，表示当前事件是否会冒泡。

Event.eventPhase 属性返回一个只读整数常量，表示事件目前所处的阶段。

```js
var phase = event.eventPhase;
```

四个阶段：

- 0 表示事件目前没有发生
- 1 表示事件目前处于捕获阶段，从祖先节点向目标节点传播的过程中
- 2 表示事件达到目标节点，event.target 属性指向的那个节点
- 3 表示事件冒泡阶段，从目标节点向祖先节点的反向传播过程中

### Event.cancelable、Event.cancelBubble、Event.defaultPrevented

Event.cancelable 属性返回一个只读布尔值，表示事件是否可以取消。大多数浏览器的原生事件是可以取消的，比如 click 事件，点击链接将无效。如果事件不可以被取消，调用 Event.preventDefault() 无效。

Event.cancelBubble 属性是一个布尔值，true 表示执行 Event.stopPropagation() 阻止事件的传播

Event.defaultPrevented 属性返回一个只读布尔值，表示该事件是否可以调用过 Event.preventDefault 方法

### Event.currentTarget、Event.target

Event.currentTarget 属性返回事件当前所在的节点，即正在执行的监听函数所绑定的那个节点

Event.target 属性返回原始触发事件的那个节点，即事件最初发生的节点。事件的传播过程中，不同节点的监听函数内部的 Event.target 与 Event.currentTarget 属性的值是不一样的，前者总是变化的，后者则是指向监听函数所在的那个节点对象。

### Event.type

```js
var evt = new Event('foo')
evt.type // "foo"
```

### Event.timeStamp

返回事件发生的毫秒时间戳

```js
var evt = new Event('foo')
evt.timeStamp // 3658.333
```

它的返回值有可能是整数，也有可能是小数（高精度时间戳），取决于浏览器的设置

下面是一个计算鼠标移动速度的例子，显示每秒移动的像素数量。

```js
var previousX, previousY, previousT
window.addEventListener('mousemove', function(event) {
  if (previousX !== undefined && previousY !== undefined && previousT !== undefined) {
    var deltaX = event.screenX - previousX
    var deltaY = event.screenY - previousY
    var deltaD = Math.sqrt(deltaX**2 + deltaY**2)
    var deltaT = event.timeStamp - previousT
    console.log(deltaD / deltaT * 1000)
  }
  previousX = event.screenX
  previousY = event.screenY
  previousT = event.timeStamp
})
```

### Event.isTrusted

返回一个布尔值，表示事件是否由真实用户行为产生。比如用户点击链接就会产生一个 click 事件，该事件是用户产生的；Event 构造函数生产的事件则是由脚本产生。

```js
var evt = new Event('foo')
evt.isTrusted // false
```

### Event.detail

只有浏览器的 UI 事件才有 Event.detail。该属性返回一个数值，表示事件的某种信息。具体含义与事件类型相关。比如 对于 click 和 dblclick 事件，Event.detail 是鼠标按下的次数（1 表示单击，2 表示双击，3 表示三击）；对于鼠标滚轮事件，Event.detail 是滚轮正向滚动的距离，负值是负向滚动的距离，返回值总是 3 的倍数。

### Event.stopPropagation()

方法阻止事件在 DOM 中继续传播，防止再触发定义在别的节点上的监听函数，但是不包括在当前节点上其他的事件监听函数。如果要阻止当前节点上的事件监听函数用 Event.stopImmediatePropagation()

### Event.stopImmediatePropagation()

阻止同一个事件的其他监听函数被调用，不管监听函数定义在当前节点还是其他节点。也就是说，该方法阻止事件的传播，比 Event.stopPropagation() 更彻底。

如果同一个节点对于同一个事件指定了多个监听函数，这些函数会根据添加的顺序依次调用。只要其中有一个监听函数调用了 Event.stopImmediatePropagation 方法，其他的监听函数就不会再执行了。

### Event.composedPath()

返回一个成员是事件的最底层节点和依次冒泡经过的所有上层节点的数组。