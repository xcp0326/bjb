---
title: Node 接口
category:
date: 2018-12-24 23:36:09
tags:
---

> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

所有的 DOM 节点都继承自 Node 接口，所有的 DOM 节点都共有 DOM.prototype 下的方法和属性。

nodeType

- 文档节点：9
- 元素节点：1
- 属性节点：2
- 文本节点：3
- 文档片段节点：11
- 文档类型节点：10
- 注释节点：8

nodeName

- 文档节点：#document
- 元素节点：大写标签名，除了 SVG 是小写标签名，因为是后来添加的。
- 属性节点：属性的名称
- 文本节点：#text
- 文档片段节点：#document-fragment
- 文档类型节点：文档的类型
- 注释节点：#comment

nodeValue：节点的文本值，所以只有文本节点、注释节点、属性节点才有 nodeValue 值。其他类型的节点都返回 null。

textContext：返回当前节点及其后代节点的文本内容。它会自动对 HTML 标签转义。文本节点、注释节点、属性节点的值和 nodeValue 一样。文档节点和文档类型节点的 textContent 属性为 null。读取整个文档的内容为 document.document.textContent.

baseURI：只读，一般由当前网页的 URL window.location 决定，可以使用`<base>`标签修改。用来计算网页上的相对路径的 URL。

nextSibling：当前节点的下一个节点，如果没有返回 null，用来遍历所有子节点：

```js
const parent = document.querySelector('.el')
const el = parent.firstChild
while(el !== null) {
  el = el.nextSibling
}
```

prevSibling：当前节点的上一个节点，如果没有就返回 null，同 nextSibling 原理，也可以用来遍历所有子节点。

parentNode：对于一个节点来说，它的父节点可能是三种类型：元素节点、文档节点、文档片段节点。

childNodes：返回节点的子节点 NodeList 伪数组动态集合

isConnected：返回节点是否在文档之中

appendChild()：插入一个节点，并作为当前节点最后一个节点，相当于生的最后一个儿子。返回这个节点。如果参数是 DocumentFragment 节点，那么插入的是 DocumentFragment 的所有子节点，而不是 DocumentFragment 节点本身。返回值是一个空的 DocumentFragment 节点。

hasChildNodes()：遍历当前节点的所有后代节点：

```js
function DOMComb(parent, callback) {
  if (parent.hasChildNodes()) {
    for (var node = parent.firstChild; node; node = node.nextSibling) {
      DOMComb(node, callback)
    }
  }
  callback(parent)
}

DOMComb(document.body, console.log)
```

cloneNode()：参数表示是否深度 clone，返回克隆的新节点。只克隆 DOM 里面带的属性，如果有 id 或者 name 属性，需要修改 clone 出来的节点的 id 和 name 属性。

insertBefore()：将第一个参数插入当前节点内的某个子节点即第二个参数前，返回新节点。如果第二个参数为 null，则是将节点插入到当前节点内部的最后的位置，也就是 appendChild 一样了。node 节点插入到 nnode 节点后：`parent.insertBefore(node, nnode.nextSibling)`。同 appendChild 方法，如果参数是 DocumentFragment 节点，那么插入的是 DocumentFragment 的所有子节点，而不是 DocumentFragment 节点本身。返回值是一个空的 DocumentFragment 节点。

replaceChild：第一个参数节点替换第二个参数节点，返回第二个参数节点

contains()：返回节点是否在当前文档中。确定参数节点为当前节点，参数节点是否为当前节点的子节点，参数节点是否为当前节点的后代节点。

compareDocumentPosition()：用法和 contains 一样，返回一个 6 bit 位的二进制值，表示参数节点与当前节点的关系

| 二进制值 | 十进制值 | 含义                                               |
| -------- | -------- | -------------------------------------------------- |
| 000000   | 0        | 两个节点相同                                       |
| 000001   | 1        | 两个节点不在同一个文档（即有一个节点不在当前文档） |
| 000010   | 2        | 参数节点在当前节点的前面                           |
| 000100   | 4        | 参数节点在当前节点的后面                           |
| 001000   | 8        | 参数节点包含当前节点                               |
| 010000   | 16       | 当前节点包含参数节点                               |
| 100000   | 32       | 浏览器内部使用                                     |

通过掩码进行运算，简称某个特定位置的标签嵌套是否合乎规范：

```js
var head = document.head;
var body = document.body;
if (head.compareDocumentPosition(body) & 4) {
  console.log('文档结构正确');
} else {
  console.log('<body> 不能在 <head> 前面');
}
```

isEqualNode() 节点与参数节点是否相等

isSameNode() 节点与参数节点是否为同一个节点

normalize() 去除节点文本节点之间的多余空格

getRootNode() 返回所在文档的根节点 `document`，与 `ownerDocument` 属性的作用相同。它可以在 document 节点自身，这一点与 document.ownerDocument 不同。