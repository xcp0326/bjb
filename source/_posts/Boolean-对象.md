---
title: Boolean 对象
category:
date: 2018-11-29 10:53:42
---

> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

Boolean 对象是 JavaScript 的三个包装对象之一。作为构造函数，它主要用于生成布尔值的包装对象实例。

```js
var b = new Boolean(true);

typeof b // "object"
b.valueOf() // true

if (new Boolean(false)) {
  console.log('true');
} // true

if (new Boolean(false).valueOf()) {
  console.log('true');
} // 无输出
```

## Boolean 函数的类型转换作用 

Boolean 对象除了可以作为构造函数，还能单独使用，将任意值转为布尔值。这是 Boolean 就是一个单纯的工具方法，将它的参数转为布尔值。

```js
Boolean(undefined) // false
Boolean(null) // false
Boolean(0) // false
Boolean('') // false
Boolean(false) // false
Boolean(NaN) // false

Boolean(1) // true
Boolean('false') // true
Boolean([]) // true
Boolean({}) // true
Boolean(function(){}) // true
Boolean(/foo/) // true
```

使用双重否运算也可以将任意值转为对应布尔值

```js
!!'' // false
!!null // false
!!undefined // false
!!0 // false
!!NaN // false
!!false // false

!!1 // true
!!'false' // true
!![] // true
!!{} // true
!!function(){} // true
!!/foo/ // true
```

注意是不使用构造函数调用 Boolean 的差异

```js
if (Boolean(false)) {console.log('true')} // 无输出
if (new Boolean(false)) {console.log('true')} // "true"

if (Boolean('')) {console.log('true')} // 无输出
if (new Boolean('')) {console.log('true')} // "true"

if (Boolean(null)) {console.log('true')} // 无输出
if (new Boolean(null)) {console.log('true')} // "true"

if (Boolean(0)) {console.log('true')} // 无输出
if (new Boolean(0)) {console.log('true')} // "true"

if (Boolean(NaN)) {console.log('true')} // 无输出
if (new Boolean(NaN)) {console.log('true')} // "true"

if (Boolean(undefined)) {console.log('true')} // 无输出
if (new Boolean(undefined)) {console.log('true')} // "true"
```

