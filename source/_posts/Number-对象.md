---
title: Number 对象
category:
date: 2018-11-29 11:32:41
---

> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

Number 对象是数值对应的包装对象，可以作为构造函数也可以作为工具函数使用。

作为构造函数，它用于生成一个数值的包装对象。

作为工具函数，它用于把任何类型的值转为数值。

## 静态属性

Number.POSITIVE_INFINITY 正无限 指向 Infinity

Number.NAGETIVE_INFINITY 负无限 指向 -Infinity

Number.NaN 表示非数值，指向 NaN

Number.MIN_VALUE 表示最小的正数（即最接近 0 的正数，在 64 位符点数体系中为 5e-324），相应的最接近 0 的负数位 -Number.MIN_VALUE

Number.MAX_VALUE 表示最大的正数 在 64 位浮点数体系中为 1.7976313486231574e+308

Number.MAX_SAFE_INTEGER 表示能够精确表示的最大整数，即 9007199254730991

Number.MIN_SAFE_INTEGER 表示能够精确表示的最小整数，即 -9007199254740991

## 实例方法

Number 有 4 个实例方法，都跟将数值转换成指定格式有关。这 4 个实例方法是 ES5 的 Number 实例方法，ES6 新加了几个。

### Number.prototype.toString()

将数值转为字符串形式。

可以传入一个参数，表示输出的进制。默认为 十进制。进制数为 2 - 36

可以方括号预算法调用 toString 方法：

```js
10[toString](2) // "1010"
```

toString 方法是将十进制的数转换为其他进制的字符串。

parseInt 可以将其他进制的字符串转换为十进制的整数。

### Number.prototype.toFixed()

先将数值转换为指定位数的小数，然后返回这个小数的字符串形式。toFixed 方法的参数为小数位数，有效范围是 0 到 20，超出范围将抛出 RangeError 错误。

### Number.prototype.toExponential()

将数值转换为科学计数法表示的字符串，可以传入一个参数作为小数点的有效位数，范围为 0 到 20，超出范围将抛出 RangeError 错误。

### Number.prototype.toPrecision()

用于将数字转为指定位数的有效数字。toFixed 是指定小数的位数，toPrecision 是指定整个数字返回的位数。有效范围是 1 到 21，超出范围会抛出 RangeError 错误。

toPrecision 用于四舍五入时不太可靠，跟浮点数不是精确存储有关。

```js
(12.35).toPrecision(3) // "12.3"
(12.25).toPrecision(3) // "12.3"
(12.15).toPrecision(3) // "12.2"
(12.45).toPrecision(3) // "12.4"
```

