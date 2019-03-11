---
title: DOM 概述
category:
date: 2018-12-24 22:43:37
tags:
---

> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

DOM 是 JavaScript 操作网页的接口，它将网页转成一个 JavaScript 对象，从而可以用脚本进行各种操作。

浏览器根据 DOM 模型，将 HTML 解析成一系列节点，再由这些节点组成一个树状结构 DOM tree。所有节点和最终的树状结构，都有规范的对外接口。

DOM 只是一个接口规范，可以用各种语言实现。所以严格地说，DOM 不是 JavaScript 语法的一部分，但是 DOM 操作是 JavaScript 最常见的任务，离开 DOM，JavaScript 就无法控制网页。另一方面，JavaScript 也是最常用于 DOM 操作的语言。

DOM 的最小组成单位是节点 node，DOM 树就是由各种不同类型的节点组成。每个节点可以看作是文档树的一片叶子。

节点的类型有七种，他们都继承自浏览器提供的节点对象 Node，都有 共有属性 Node.prototype

- Document：整个文档树的顶层节点
- DocumentType：doctype 标签
- Element：标签节点
- Attribute：标签节点的属性
- Text：标签之间（比如空格）或标签包含的文本
- Comment：注释
- DocumentFragment：文档片段

document 节点继承自 Document，代表整个文档树，它有一个子节点标签节点 html，也是 DOM 树的根节点 root node。