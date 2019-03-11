---
title: NodeList 接口，HTMLCollection 接口
category:
date: 2018-12-24 23:50:27
tags:
---

> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

NodeList 是 Node 对象的伪数组

HTMLCollection 是标签节点的伪数组

Node.childNodes 属性返回 NodeList，也只有这个属性的返回的 NodeList 是动态集合

document.querySelectorAll() 节点搜索方法返回 NodeList

NodeList 有 forEach 方法和 length 属性，但是么有数组的 push 等方法

NodeList 的 item 方法，参数为整数值，表示成员的位置，返回该位置的成员。这个方法等价于 `NodeList[<int>]`

keys、values、entries 三个方法分别返回一个键名遍历器、一个键值遍历器、一个包含键名和键值的遍历器。遍历器对象可以通过 for...of 循环遍历获取每一个成员的信息。

HTMLCollection 没有 forEach 方法，只能使用 for 循环。它主要是一些 Document 对象的集合属性：document.links、document.forms、document.images 等。如果节点有 id 或 name 属性，可以用 id 或者 name 来引用节点元素。

```js
// HTML 代码如下
// <img id="pic" src="http://example.com/foo.jpg">

var pic = document.getElementById('pic');
document.images.pic === pic // true
```

namedItem() 参数为 id 属性或 name 属性的值，返回对应的标签节点，如果么有返回 null。