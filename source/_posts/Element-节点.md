---
title: Element 节点
category:
date: 2018-12-27 12:31:17
tags:
---

> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

不同的 HTML 对象对应的标签节点不一样，浏览器使用了不同的构造函数，生成不同的标签节点。比如标签 a 的节点对象是由 HTMLAnchorElement 构造生成，button 标签的节点对象由 HTMLButtonElement 构造生成。因此，元素节点不是一种对象，而是一组对象，这些对象除了继承 Element 的属性和方法，还有各自的构造函数的属性和方法。

特性属性

- Element.id
- Element.tagName 除了 svg 标签其他标签都是返回大写的标签名，与 nodeName 属性的值相等
- Element.dir 读写当前标签的文字方向：ltr 或者 rtl。并不是所有 Element 都有这个值，亲测 svg 是 undefined，div、p、span 标签的都是空
- Element.accessKey 标签设置快捷键
- Element.draggable
- Element.lang
- Element.tabIndex 按 Tab 键遍历 tabIndex 为正整数的标签，从大到小，如果值一样按出现顺序，然后遍历 0、属性为非法值或者没有 tabIndex 属性的标签，按照它们出现顺序。
- Element.title 鼠标悬浮时弹出的文字提示框的文案

状态属性

- Element.hidden 优先级低于 CSS 指定的 display
- Element.contentEditable、Element.isContentEditable：前者设置或者返回一个字符串 true/false/inherit，后者表示是否设置了 contentEditable，返回布尔值。

Element.attributes 返回标签属性值的伪数组

Element.className、Element.classList：className 返回标签的所有 class 的字符串。classList 返回一个包含所有 class 的伪数组，这个伪数组还有方法可以控制标签的 class 们：add(classStr1, classStr2, ...)、remove(classStr1, classStr2, ...)、contains(classStr1, classStr2, ...)、toggle(classStr) 返回 false 表示标签已经清空 classStr，否则表示添加上了 classStr、item(indexNum)、toString()

Element.dataset 返回标签所有 data-xxx 属性的对象。xxx 只能包含英文字母、数字、连词线、点、冒号和下划线。如果 xxx 内是用连词线后跟字母，则 dataset 获取的时候，需要删除连词线并大写连词线后的字母后的字符串作为 dataset 的属性来获取。

Element.innerHTML 设为空，清空标签下的所有节点。读取属性值的时候，文本节点包含 &、<、> 都会被转成实体形式。如果想得到原文，可以用 Element.textContent。而写入的时候，如果插入的文本包含 HTML 标签，会被解析成为节点对象。如果文本中有 script 标签，虽然可以生成 script 节点，但是插入的代码不会执行。但是以下 alert 还是会被执行。因此为了安全考虑，如果插入文本使用 textContent 代替 innerHTML

```js
var name = "<img src=x onerror=alert(1)>";
el.innerHTML = name;
```

Element.outerHTML 返回一个有父节点的当前标签及其当前标签下的所有子标签，没有父节点会报错

Element.clientHeight、Element.clientWidth 块级标签的 CSS 高度/宽度，如果没有则返回实际高度/宽度，包括 padding 部分，但不包括 border、margin 部分。小数会四舍五入为整数。

Element.clientLeft、Element.clientTop 块级标签左边框 left border 的宽度、上边框 top border 的高度

Element.scrollHeight、Element.scrollWidth 元素的总高度 包括溢出超出部分。document.documentElement.scrollHeight 获取整个网页的内容高度。

Element.offsetParent offset 的相对元素

Element.offsetHeight、Element.offsetWidth 包括 padding、border。额，Mac Chrome 亲测只是单纯的高度和宽度，并不包括 padding、border。

Element.offsetLeft、Element.offsetTop 包括 margin 值

Element.style

Element.children、Element.childElementCount

Element.firstElementChild、Element.lastElementChild

Element.nextElementSibling、Element.previousSibling

属性相关方法

- Element.getAttribute()、Element.getAttributeNames()、Element.setAttribute()、Element.hasAttribute()、Element.hasAttributes()、Element.removeAttribute() 
- Element.getAttributeName()
- Element.querySelector()
- Element.querySelectorAll()
- Element.getElementsByClassName()
- Element.getElementsByTagName()
- Element.closest() 参数是 CSS 选择器，返回匹配该选择器、最接近当前节点的一个祖先节点（包括当前节点本身），没有返回 null。
- Element.matches() 返回当前节点是否匹配给定的 CSS 选择器

事件相关方法

- Element.addEventListener() 添加监听事件、Element.removeEventListener() 移除监听事件、Event.dispatchEvent() 触发事件
- Element.scrolllntoView() 浏览器滚动到当前标签
- Element.getBoundingClientRect() 返回一个包含当前标签的 CSS 盒模型的所有信息的 rect 对象 x：元素左上角相对于视口的横坐标、y：元素左上角相对于视口的纵坐标、height、width、top、bottom。元素相对于视口的位置，是随着页面滚动变化的，因此表示位置的四个属性值，都不是固定不变的。如果想得到绝对的值，可以将 left 属性加上 window.scrollX，top 属性加上 window.scrollY。rect 对象里的值都是把 border 计算进去了的。也就是说，都是从边框外缘的各个点来计算。因此，width 和 height 包括了元素本身 + padding + border。rect 对象里的属性都是继承自原型的属性，所以 Object.keys(rect) 是一个空数组。
- Element.getClientRects()  返回标签节点在页面上形成的所有矩形的伪数组，伪数组内都有 bottom、height、left、right、top、width 6 个属性，表示矩形相对于视口的四个坐标以及本身的高度和宽度。块状标签只有一个矩形，而内嵌标签，如果是多行文本就会有多个矩形。
- Element.insertAdjacentElement() 在相对于当前标签节点的指定位置，插入一个新的节点，返回被插入的节点，如果插入失败，返回 null。接收两个参数，第一个参数表示插入元素的位置：beforebegin 表示当前元素之前、afterbegin 表示当前元素内部的第一个子节点前面、beforeend 表示元素内部的最后一个子节点后面、afterend 表示当前元素之后。beforebegin 和 afterend 这两个值，只有在当前节点有父节点时才会生效，如果没有父节点插入会失败，返回 null。
- Element.insertAdjacentHTML()、Element.insertAdjacentText() 前者是插入一段 HTML 字符串 到指定的位置，第一个参数是指定位置，位置参数与 insertAdjacentElement 一致。后者插入的是文本节点。
- Element.remove() 继承自 ChildNode 接口，用于将当前节点从它的父节点移除。
- Element.focus()、Element.blur() 接受一个对象作为参数，参数对象的 preventScroll 属性是一个布尔值，指定是否将当前元素停留在原始位置，而不是滚动到可见区域。意思就是可以实现获取焦点并滚动到焦点所在的位置。
- Element.click() 模拟鼠标点击，也就是触发 click 事件。