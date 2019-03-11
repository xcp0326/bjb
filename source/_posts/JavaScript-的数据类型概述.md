---
title: JavaScript 的数据类型概述
tags:
  - JavaScript
  - 数据类型
date: 2018-11-16 13:58:19
---

> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

## JavaScript 的数据类型

JavaScript 语言的每一个值，都属于某一种数据类型。JavaScript 的数据类型有 7 种：

- 数值 number
- 字符串 string
- 布尔值 boolean true / false
- undefined：声明后但是未定义，或者未赋值
- 
- null：表示空值
- 对象 object
- Symbol：ES 6 新增的数据类型，表示唯一的值。

数值、字符串、布尔值是原始类型的值

对象是合成类型，引用类型的值，对象往往是由多个原始类型的值合成，可以看作是一个存放各种值的容器

undfined 和 null 被看成两个特殊值

对象又可以分成三个子类型：

- 狭义的对象 {}
- 数组 []
- 函数 function(){}

函数式一种数据类型，因为它能作为值赋值给变量

## 如何确定一个值的数据类型

有三种方法可以用来确定 JavaScript 值的数据类型：

- typeof 运算符
- instanceof 运算符
- Object.prototype.toString 方法

基本类型，undefined 和函数都可以通过 typeof 运算符来确定

```js
typeof 123 // 'number'
typeof '123' // 'string'
typeof false // 'boolean'
typeof a // 'undefined' 没有声明过的变量没有值，所以返回 undefind
function f(){}
typeof f // 'function'
```

而 狭义的对象和数组无法通过 typeof 运算符获知数据类型，这是因为 JavaScript 内部，它们本质上都是一种特殊的对象。

```js
typeof {} // 'object'
typeof [] // 'object'
```

可以用 instanceof 运算符来区分 {} 和 []

```js
var a = {}, b = []
a instanceof Object // true
b instanceof Array // true
```

null 的数据类型是 object

```js
typeof null // 'object'
```

null 的数据类型是 object，是最初 JavaScript 设计遗留的 bug。95 年 JavaScript 语言的第一版，只设计了 5 种数据类型：对象、整数、浮点数、字符串、布尔值，没考虑到 null，只把它当作 object 的一种特殊值。后来 null 独立出来，作为一种单独的数据类型，为了兼容一起的代码，typeof null 返回 object 就没法改变了... 

很简单确认值是不是 null 类型 [underscore 实现 isNull](https://underscorejs.org/docs/underscore.html#section-155)

```js
function isNull(obj) {
  return obj === null;
}
```