---
title: Math 对象
category:
date: 2018-12-04 15:24:01
---

> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

Math 是 JavaScript 的原生对象，提供各种数学功能。该对象不是构造函数，不能生成实例，所有的属性和方法都必须在 Math 对象上调用。

## 静态属性

Math 对象的静态属性，提供一些只读的数学常量。非严格模式下修改无效，严格模式下报错。

- Math.E：常数 e，（e 作为数学常数，是自然对数函数的底数。它是一个无限不循环小数，数值约为 2.71828182845904523536…，小数点后 20 位）
- Math.LN2：2 的自然对数（自然对数，是指以 e 为底数的对数函数，标记作 ln(x) 或者 log<sub>e</sub>(x)）
- Math.LN10：10 的自然对数
- Math.LOG2E：2 为底 e 的对数
- Math.LOG10E：10 为底 e 的对数
- Math.PI：常数 π
- Math.SQRT1_2：0.5 的平方根
- Math.SQRT2：2 的平方根

## 静态方法

**Math.ceil()**：ceiling 天花板数，向上取整

**Math.floor**()：地板数，向下取整

**Math.max**()：如果参数为空返回 -Infinity

**Math.min**()：如果参数为空返回 Infinity

**Math.pow**()：power 次方，缩写 pow

**Math.sqrt**()：开平方根，参数是负数返回 NaN

**Math.log**()：返回以 e 为底的自然对数。

```js
Math.log(Math.E) // 1
Math.log(10) // 等同于 Math.LN10
```

如果要计算以 10 为底的对数，可以用 Math.log 求出自然对数，然后除以 Math.LN10；求以 2 为底的对数，可以除以 Math.LN2

```js
Math.log(100) / Math.LN10 // 2
Math.log(8) / Math.LN2 // 3
```

> 引申题：求整数的位数 比如 1234 是 4 位
>
> => Math.floor(Math.log(1234)/Math.LN10) + 1

**Math.exp**()：返回常数 e 的参数次方

```js
Math.exp(3) === Math.E ** 3 // true
```

**Math.round()**：返回一个数字四舍五入后最接近的整数

```js
function round(number, percision) {
  return Math.round(+number + 'e' + percision) / Math.pow(10, percision);
  // 或者
  // return Number(Math.round(+number + 'e' + percision) + 'e-' + percision)
}
round(1.25, 1) // 1.3
round(1.25, 0) // 1
```

**Math.random()**：返回 0 到 1 之间的伪随机数。

生成任意范围的随机数

```js
function getRandomArbitrary(min, max) {
  return Math.random() * (max - min) + min;
}
```

生成任意访问的随机整数

```js
function getRandomArbitrary(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}
```

返回随机字符串

```js
function random_str(length) {
  var ALPHABET = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
  ALPHABET += 'abcdefghijklmnopqrstuvwxyz';
  ALPHABET += '0123456789-_';
	var str = '';
  for (var i = 0; i < length; ++i) {
    var rand = Math.floor(Math.random() * ALPHABET.length)
    str += ALPHABET.substr(rand, 1)
  }
  return str
}
```

## ⚠️ 三角函数方法

Math.sin 返回参数的正弦

Math.cos 返回参数的余弦

Math.tan 返回参数的正切

Math.asin 返回参数的反正弦

Math.acos 返回参数的反余弦

Math.atan 返回参数的反正切