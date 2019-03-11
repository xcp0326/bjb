---
title: ParentNode 接口，ChildNode 接口
category:
date: 2018-12-25 00:24:44
tags:
---

> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

节点对象除了继承 Node 对象外，还会继承其他接口。 ParentNode 接口表示当前节点是一个父节点，它会有一些处理子节点的方法。ChildNode 接口表示当前节点是一个子节点，提供一些相关方法。

标签节点、文档节点、文档片段节点才有父节点。

ParentNode.children 属性，当前节点作为父节点，返回当前父节点的所有标签子节点，一个 HTMLCollection 实例，HTMLCollection 是动态集合。

ParentNode.firstElementChild、ParentNode.lastElementChild、parentNode.childElementCount

ParentNode.append() 当前节点下的最后一个节点后加一个或多个子节点，子节点可以是文本节点。

ParentNode.prepend() 当前节点下的第一个元素节点前添加一个或多个子节点，子节点可以是文本节点。

如果一个节点有父节点，那么该节点就继承了 ChildNode 接口。

ChildNode.remove()

ChildNode.before()、ChildNode.after()：在当前节点前面、后面插入一个或多个同级节点。节点可以是文本节点。

ChildNode.replaceWith()：使用参数节点替换当前节点。参数节点可以是文本节点。



