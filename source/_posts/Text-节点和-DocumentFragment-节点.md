---
title: Text 节点和 DocumentFragment 节点
category:
date: 2018-12-27 13:22:53
tags:
---

> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

Text 节点代表元素节点和属性节点的文本内容。如果一个节点只包含一段文本，那么它就有一个文本子节点，代表该节点的文本内容。

可以使用父节点的 firstChild、nextSibling 等属性获取文本节点
使用 Document.createTextNode 方法创造一个文本节点

浏览器原生提供了一个 Text 构造函数，它返回一个文本节点实例。它的参数就是该文本节点的文本内容。

```js
var text = new Text('I\'m a lovely Panda.')
```

文本节点继承了 Node 接口，还继承了 CharacterData 接口。

## Text 节点的属性

data 属性等同于 nodeValue 属性，用来设置或读取文本节点的内容。

wholeText 属性将当前文本节点于毗邻的文本节点，作为一个整体返回。大多数情况下，wholeText 属性的返回值，与 data 属性和 textContext 属性相同。

```html
<p id="para">
  A <em>B</em> C
</p>
```

这时候的 wholeText 属性和 data 属性返回值相同

```js
var el = document.getElementById('para');
el.firstChild.wholeText // "A "
el.firstChild.data // "A "
```

但是一旦移除 em 节点，wholeText 属性与 data 属性就会有差异，因为这时其实 p 节点下面包含了两个毗邻的文本节点

```js
el.removeChild(para.childNodes[1]);
el.firstChild.wholeText // "A C"
el.firstChild.data // "A "
```

length、nextElementSibling、previousElementSibling 

appendData() 在 Text 节点后追加字符串、deleteData() 删除 Text 节点内部的子字符串，第一个参数为子字符串，第二个参数为子字符串的长度、insertData() 在 Text 节点插入字符串，第一个参数为插入的位置，第二个参数为插入的子字符串、replaceData() 用于替换文本，第一个参数为替换开始位置，第二个参数为需要被替换掉的长度，第三个参数为新加入的字符串、subStringData() 用于获取子字符串，第一个参数为子字符串在 Text 节点的开始位置，第二个参数为子字符串长度：

```js
// HTML 代码为
// <p>Hello World</p>
var pElementText = document.querySelector('p').firstChild;

pElementText.appendData('!');
// 页面显示 Hello World!
pElementText.deleteData(7, 5);
// 页面显示 Hello W
pElementText.insertData(7, 'Hello ');
// 页面显示 Hello WHello
pElementText.replaceData(7, 5, 'World');
// 页面显示 Hello WWorld
pElementText.substringData(7, 10); 
// 页面显示不变，返回"World "
```

remove() 移除当前 Text 节点

splitText() 将 Text 节点一分为二，变成两个毗邻的 Text 节点。它的参数就是分割位置（从零开始），分割到该位置的字符前结束。。如果分割位置不存在，就报错。分割后，该方法返回分割位置后方的字符串，而原 Text 节点变成只包含分割位置前方的字符串。父元素的节点的 normalize 方法可以将两个毗邻的 Text 节点河北。

## DocumentFragment 节点

DocumentFragment 节点代表一个文档片段，本身就是一个完整的 DOM 树形结构。它没有父节点，parentNode 为 null，但是可以插入任意数量的子节点。它不属于当前文档。操作 DocumentFragment 节点要比直接操作 DOM 数快得多。

它一般用于构建一个 DOM 结构，然后插入到当前文档。document.createDocumentFragment 方法以及浏览器原生的 DocumentFragment 构造函数，可以创建一个空的 DocumentFragment 节点。然后再使用其他 DOM 方法，向其添加子节点。

DocumentFragment 节点本身不能被插入当前文档。当它作为 appendChild()、insertBefore()、replaceChild() 等方法的参数时，是它的所有子节点插入当前文档，而不是它自身。一旦 DocumentFragment 节点被添加进文档，它自身就变成了空节点（textContent 属性为空字符串），可以被再次使用。如果想要保存 DocumentFragment 节点的内容，可以使用 cloneNode 方法。

DocumentFragment 反转一个指定节点的所有子节点的顺序

```js
function reverse(n) {
  var f = document.createDocumentFragment();
  while(n.lastChild) f.appendChild(n.lastChild);
  n.appendChild(f);
}
```

DocumentFragment 节点对象么有自己的属性和方法，全部继承自 Node 节点和 ParentNode 接口。也就是说 DocumentFragement 节点比 Node 节点多了四个属性：children、firstElementChild、lastElementChild、childElementCount。