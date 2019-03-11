---
title: 实现一个 jQuery API
tags:
  - jQuery
category:
  - 练习
date: 2018-12-24 12:44:32
---


我们的需求是这样的：

```js
window.jQuery = ???
window.$ = jQuery

var $div = $('div')
$div.addClass('red') // 可将所有 div 的 class 添加一个 red
$div.setText('hi') // 可将所有 div 的 textContent 变为 hi
```

实现一个 addClass 的函数，给一个节点添加 className：

```js
function addClass(node, className) {
  node.classList.add(className)
}
```

题目的要求是给所有节点添加 class，那就给所有节点循环执行一下添加 className，我们要找到一个 NodeList 的伪数组，然后给它下的每个节点 add 一下 className

```js
function addClass(selector, className) {
  const nodes = document.querySelectorAll(selector)
  nodes.forEach(node => node.classList.add(className))
}
```

如果 selector 传入时就已经是一个节点，或者节点列表了，那就做一下类型判断：

```js
function addClass(nodeOrSelector, className) {
  let nodes
  if (typeof nodeOrSelector === 'string') {
    nodes = document.querySelectorAll(nodeOrSelector)
  } else if (typeof nodeOrSelector.length === 'undefined') {
    // nodeOrSelector 没有 length 那就说明传入的是单个节点
    nodes = [nodeOrSelector]
  } else {
    nodes = nodeOrSelector
  }
  nodes.forEach(node => node.classList.add(className))
}
```

然后把 addClass 移入到 window.jQuery 内。

题目上写了，`window.$ = jQuery`，给 window.jQuery 的别名设置为美元符号，接下来还调用了这个美元符号的函数，可以看出来，jQuery 为一个函数，参数为 selector 也就是我们 addClass 里面的 nodeOrSelector。

而 addClass 是作为这个函数执行后返回值的一个方法。

所以接下来的代码为：

```js
window.jQuery = function(nodeOrSelector) {
  let nodes
  
  if (typeof nodeOrSelector === 'string') {
    nodes = document.querySelectorAll(nodeOrSelector)
  } else if (typeof nodeOrSelector.length === 'undefined') {
    // nodeOrSelector 没有 length 那就说明传入的是单个节点
    nodes = [nodeOrSelector]
  } else {
    nodes = nodeOrSelector
  }
  
  return {
    addClass: function(className) {
      nodes.forEach(node => node.classList.add(className))
    }
  }
}
```

然后我们实现 setText，题目要求是用节点的 API textContent：

```js
function addClass(nodeOrSelector, text) {
  let nodes
  if (typeof nodeOrSelector === 'string') {
    nodes = document.querySelectorAll(nodeOrSelector)
  } else if (typeof nodeOrSelector.length === 'undefined') {
    // nodeOrSelector 没有 length 那就说明传入的是单个节点
    nodes = [nodeOrSelector]
  } else {
    nodes = nodeOrSelector
  }
  nodes.forEach(node => node.textContent = text)
}
```

把它作为 window.jQuery 函数返回值的一个方法。再加上别名：`window.$ = jQuery`。

```js
window.jQuery = function(nodeOrSelector) {
  let nodes
  
  if (typeof nodeOrSelector === 'string') {
    nodes = document.querySelectorAll(nodeOrSelector)
  } else if (typeof nodeOrSelector.length === 'undefined') {
    // nodeOrSelector 没有 length 那就说明传入的是单个节点
    nodes = [nodeOrSelector]
  } else {
    nodes = nodeOrSelector
  }
  
  return {
    addClass: function(className) {
      nodes.forEach(node => node.classList.add(className))
      return this
    },
    setText: function(text) {
      nodes.forEach(node => node.textContent = text)
      return this
    }
  }
}

window.$ = jQuery
```

原本 jQuery 函数它还会返回几个其他的 key，比如对应的node 还有 length 等，我们也来加入一下：

```js
window.jQuery = function(nodeOrSelector) {
  let jQueryObj, nodes
  
  if (typeof nodeOrSelector === 'string') {
    nodes = document.querySelectorAll(nodeOrSelector)
  } else if (typeof nodeOrSelector.length === 'undefined') {
    nodes = [nodeOrSelector]
  } else {
    nodes = nodeOrSelector
  }
  
  nodes.forEach((n, i) => {
    jQueryObj[i] = n
  })
  
  jQueryObj.length = nodes.length
  
  jQueryObj.addClass = function(className) {
    nodes.forEach(node => node.classList.add(className))
    return this
  }
  
  jQueryObj.setText = function(text) {
    nodes.forEach(node => node.textContent = text)
    return this
  }
    
  return jQueryObj
}

window.$ = jQuery
```

