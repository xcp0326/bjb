---
title: JavaScript 导论
tags:
  - JavaScript
  - 脚本语言
date: 2018-11-15 12:38:13
---

> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

## JavaScript 是一种轻量级的脚本语言

- [什么是“脚本语言”](http://www.yinwang.org/blog-cn/2013/03/29/scripting-language)
- [什么是脚本语言？](https://www.zhihu.com/question/22220383)
  - Python 也是脚本语言，相对于 Python，JavaScript 是一种轻量级脚本语言。
  - 脚本语言比普通语言性能差

[为什么说 Python 的性能差？](https://www.jianshu.com/p/3e810a619e9f)

- 是动态语言，编译器无法在编译的过程中获知变量的相关属性，导致无法优化
- 虽然是解释执行，但是不支持 JIT
- 一切都是对象，每个对象都需要维护引用计数，增加了额外的工作
- [Python GIL](http://cenalulu.github.io/python/gil-in-python/)，Global Interpreter Lock
- 垃圾回收，每次垃圾回收的时候都会中断正在执行的程序。[据说 Instagram 禁用 GC 机制后，性能提升了 10%](https://mp.weixin.qq.com/s?__biz=MjM5MDE0Mjc4MA==&mid=2650995396&idx=1&sn=3fc6f216fcf8f09232694b499da541c3)

## JavaScript 也是一种嵌入式语言

核心算法不多，只能用来做一些数学和逻辑运算

本身不提供任何于 I/O 相关的 API，都要靠宿主环境提供，所以 JavaScript 只合适嵌入更型的应用程序环境，去调用宿主环境提供的底层 API。

常见的嵌入环境，浏览器或者服务器 Node 环境

## JavaScript 是一种"对象模型"语言

各种宿主环境通过这个模型，描述自己的功能和操作接口，从而通过 JavaScript 控制这些功能。

但是 JavaScript 并不是纯粹的“面向对象语言”，还支持七天编程范式（比如函数式编程）。

JavaScript 的核心算法只包括两部分：基本语法构造 和标准库。除此之外，各种宿主环境提供额外的 API，比如浏览器提供的额外的 API 分为三大类：浏览器控制类用于操作浏览器， DOM 类用于操作网页的各种元素，Web 类用于实现互联网的各种功能。

