---
title: DOM 知识点总结篇
tags:
  - DOM
date: 2018-12-28 19:23:45
---


## 什么是 DOM

一个 HTML 文档，初始代码：

```html
<!DOCTYPE html> <!-- 告诉浏览器请用 HTML5 来解析以下代码 -->
<html lang="zh-Hans"> <!-- zh-Hans, s 是 simple 表示中文简体 -->
  <head>
  	<title>Document</title>
  </head>
  <body></body>
</html>
```

> 🤔 如果 DOCTYPE 是 XML 了？
>
> 我记得 RSS 就是 XML 格式的，[What Is RSS](https://www.xml.com/pub/a/2002/12/18/dive-into-xml.html)
>
> [A Guide to XML](https://www.xml.com/pub/a/w3j/s3.walsh.html) / What Do XML Documents Look Like? 
> 所以 XML 文档大概长这样，它的 DOCTYPE 是 `<?XML version=xxx..?>`
>
> ```xml
> <?XML version="1.0"?>
> <oldjoke>
>   <burns>Say <quote>goodnight
>     </quote>, Gracie.</burns>
>   <allen><quote>Goodnight, Gracie.
>     </quote></allen>
>   <applause/>
> </oldjoke>
> ```

HTML5 规范里，html、head、body 三个标签都是可以省略的，属性值无所谓单双引号。

HTML 文档，在 DOM 中对应字母 D，即 Document。
然后它在内存中存储的 HTML 文档的数据，对应 DOM 中的 O，即  Object，这个 Object 存在堆中，按照一个树型结构，通过构造函数构造出对象，将 DOM 存储在内存中。
Document 和 Object 之间的对应关系是用一个树型结构的模型来表示，这个模型叫做 Model。
它们三者组成了 Document Object Model 简称 DOM，读作 [dʌm]。

以下 DOM 树

![DOM tree](/images/dom/tree.png)

DOM 树中的每个节点都是 Node 类型。
节点类型主要有：document节点、标签节点、文本节点、注释节点

- 标签节点是由 Element 构造函数构造
- 文本节点是由 Text 构造函数构造
- document 节点是由 Document 构造函数构造，它代表了整个文档。`document` 可以获取整个文档，`document.documentElement` 则获取是 html 节点，即根节点。
- 所有节点构造函数（Element、Text、Comment、Document构造函数）都继承自 Node 构造函数，而 Node 构造函数又继承自 JavaScript 里的 Object.prototype。节点们都有 Node 的实例属性和方法，比如 Node.prototype.nodeName。节点们都有 Object 的实例属性和方法，比如 Object.prototype.toString 方法。

![Node constructor tree](/images/dom/node-tree.jpg)

## DOM 操作

DOM 将 HTML 页面与 JavaScript 连接起来，它提供了一系列 API 来操作 HTML 代码，操作 DOM 节点。DOM 最开始设计不是给 HTML 用的，而是给 XML 用的，所以有很多 API 用起来就是很奇怪。

- 获取节点：node.querySelector、node.querySelectorAll。通过节点关系获取节点：parentNode、children、nextSibling、nextElemntSibling 等
- 修改节点的属性：节点类型 标签/文本，节点名称 div/#text，节点内部文本 textContent，标签节点的样式 className、classList 等
- 删除节点：removeChild、删除标签节点 remove
- 创建节点：创建标签节点 document.createElement('div')、创建文本节点 document.createTextNode('xx')。

## 常用 Node 类型的属性及方法

所有节点都继承自 Node 类型，也就是所有节点都有以下这些属性和方法：

- 确定一个节点的类型

  ```js
  const p = document.querySelector('p')
  p.nodeName // "P"
  p.nodeType // 1
  document.nodeName // "#document"
  ```

  nodeName 返回节点的名字。如果是文本节点返回的是 `#text`、如果是文档节点返回的是 `#document`、而如果是标签节点则返回这个标签的大写，比如 p 标签则返回 `P`，有一个元素标签例外，那就是 SVG 标签，它返回的是小写的 svg。


  nodeType 返回的节点类型对应的数值。1 表示的是标签节点，即 Element 类型的节点；3 则表示是文本节点； 9 表示文档节点；11 表示 DocumentFragment。当时设计 nodeType 用数值表示，是为了省内存。

- 获取节点内的文本 textContext

  ```js
  const p = document.createElement('p')
  const tn = document.createTextNode('panda')
  p.appendChild(tn)
  document.body.append(p)
  tn.innerText // undefined
  tn.innerHTML // undefined
  tn.textContent // "panda"
  p.innerText // "panda"
  p.innerHTML // "panda"
  p.textContent // "panda"
  ```

  标签节点有个 innerHTML 和 innerText，如果文本节点访问 innerHTML、innerText 会返回 undefined。innerText 返回节点下所有的文本包括其后代节点。IE 发布 innerText ~ ，FF、opera 发布的 textContent，会获取 script、style 的文本。innerText 会触发重排。

- 获取兄弟节点、子节点、父节点

  ```js
  const main = document.querySelector('main')
  main.nextSibling
  main.nextElementSibling
  main.previousSibling
  main.childNodes // 返回一个动态的 NodeList 伪数组
  main.children // 返回一个动态的 HTMLCollection 标签集合伪数组
  main.firstChild
  main.lastChild
  main.parentNode
  main.parentElement
  ```

  注意 NodeList 伪数组只能使用数字索引节点，而 HTMLCollection 对象则可以用 id 属性或者 name 属性来索引标签节点。

- node.hasChildNodes() 返回是否有子节点，也可以用 node.firstChild 为空来判断，也可以用 node.childNodes && node.childNodes.length 为否来判断。

- node.cloneNode(deep) 默认 deep 为 true，表示深度克隆，会克隆该 node 的所有后代节点，否则浅克隆只克隆当前 node。

- node.contains(otherNode) 返回 node 是否包含 otherNode。

- node.isEqualNode(otherNode) 如果 node 和 otherNode 的 nodeType、子节点、属性等一致返回 true，否则 false

- node.normalize() 规范化 node 和 node 后代节点。规范化后不存在空的文本节点，或者两个相邻的文本节点。

- 输入 id 的值可以快速获取 id 为这个值的节点。比如有一段 HTML：`<div id="myHoneyPanda">balabala...</div>` 然后可以在 JavaScript 里面输入 `myHoneyPanda` 获取这个节点。

## 常见的 document 属性和方法

document 继承自 Document 类型。表示一个 HTML 的文档。

- document.documentElement 返回 html 节点
- document.activeElment 指向获得焦点的那个节点，相当于 Chrome devtool 里面的 `$0-$9` ？ 
- 获取文档下的指定标签的集合，可以抓包下载美图~
  - document.images 
  - document.links
- 获取文档信息
  - document.location 返回 Location 对象，document.documentURI 返回当前文档的路径，document.URL 返回当前文档的 URL
  - document.readyState 返回当前文档加载状态
  - document.cookie cookie 设置/获取
  - document.domain 域名
  - document.defaultView 返回一个 window 对象
- document.querySelectorAll 获取到的标签伪数组不是动态的。

## 常见 Element 节点的属性和方法

- document.documentElement.scrollHeight 获取整个页面的高度
- document.documentElement.clientHeight 获取整个浏览器视口的高度
- Element.clientHeight/clientLeft 返回节点的不包括 border、margin 的高度/左边框的宽度
- Element.scrollHeight/scrollLeft 经过浏览器视口变化，返回节点的不包括 border、margin 的高度/滚动条离视口左边的距离
- Element.offsetHeight/offsetLeft 经过浏览器视口变化，返回节点所占的整个页面左上角 距离 右下角的宽度和高度，并且左上角是包括 padding 和 border 的 / 距离父节点的左侧的距离。
- Element.getBoundingClientRect().left、Element.getBoundingClientRect().top

