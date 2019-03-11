---
title: null、undefined 和布尔值
tags:
  - null
  - undefined
  - 布尔值
  - boolean
date: 2018-11-16 14:28:22
---

> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

## null 与 undefined

null 和 undefined 都是没有得意思。if 语句中判断都是 false；而且相等运算 == 返回 true。

```js
if (!null) console.log('null is false') // null is false
if (!undefined) console.log('undefined is false') // undefined is false
null == undefined; // true
```

95 年 JavaScript 诞生时，null 设计为和 Java 里面的一样，null 可以自动转为 0

```js
Number(null) // 0
5 + null // 5
```

但是 JavaScript 的设计者 Brendan Eich 觉得这么做还不够。

首先，第一版 JavaScript 里面，null 就像 Java 一样，被当成一个对象，Brendan Eich 觉得表示“无”的值最好不是对象

其次，那时的 JavaScript 不包括错误处理机制，Brendan Eich 觉得，如果 null 自动转为 0，很不容易发现错误

因此 Brendan Eich 又设计了一个 undefined。区别是这样的：null 表示一个空的对象，转为数值时为 0；undefined 表示没有定义的原始值，转为数值时为 NaN

```js
Number(undefined) // NaN
5 + undefined // NaN
```

**总结一下就是：**

null 表示空值。调用函数时，某个参数未设置任何值，这是就可以传入 null，表示该参数为空。比如，某个函数接受引擎抛出的错误作为参数，如果运行过程中未出错，那么这个参数就会传入 null，表示未发生错误。

undefined 表示未定义

```js
var i;
i // 此时 i 为undefined，因为只声明了没有赋值

function f(x) {
  return x;
}
f() // 此时函数返回 undefined，因为 x 没有赋值

var o = {}
o.p // 返回 undefined，因为 p 是 o 没有赋值的属性

function f(){}
f() // 函数如果没有返回值，默认返回 undefined
```

## 布尔值

下列运算符会返回布尔值：

- 逻辑非运算符：`!`
- 相等运算符：`==`、`===`、`!=`、`!==`
- 比较运算符：`>`、`<`、`>=`、`<=`

如果 JavaScript 预期某个位置应该是布尔值，就会将该位置上的现有值转换为布尔值。转换规则是除了下面 6 个值被转为 false，其他都视为 true。

- undefined
- null
- false
- 空字符串 '' 或者 ""
- NaN
- 0

空数组和空对象都是 true

```js
if ([]) console.log('ahahaha1') // 这里打印出来 ahahaha1
if ({}) console.log('ahahaha2') // 这里打印出来 ahahaha2
```

