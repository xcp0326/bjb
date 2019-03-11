---
title: String 对象
category:
date: 2018-12-03 15:21:15
---

> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

String 对象是 JavaScript 原生提供的三个包装对象之一，用来生成字符串对象。给生成的字符串对象调用 valueOf 将返回它对应的原始字符串。

除了用作构造函数，String 还可以用作工具方法，用来将任意类型的值转换为字符串。

## 静态方法

String 对象提供的静态方法（即定义在对象本身，而不是定义在对象实例的方法）

### String.fromCharCode

该方法的参数是一个或多个数值，代表 Unicode 码点，返回值是这写码点组成的字符串。如果参数为空，就返回空字符串。

```js
String.fromCharCode(104,101,108,108,111) // "hello"
```

不支持 Unicode 大于 0xFFFF 的字符，即传入的参数不能大于 0xFFFF，即 十进制 65535。

```js
String.fromCharCode() // ""
String.fromCharCode(0x20BB7) // 'ஷ' 
String.fromCharCode(0x20BB7) === String.fromCharCode(0x0BB7) // true
```

> String.fromCharCode 发现参数值大于 0xFFFF，就会忽略多出的位，即忽略 0x20BB7 里面的 2。这是因为码点大于 0xFFFF 的字符占用四个字节，而 JavaScript 默认只支持两个字节。这种情况下把 0x20BB7 拆成两个字符表示。
>
> ```js
> String.fromCharCode(0xD842, 0xDFB7)
> // "吉"
> ```
>
> 码点大于 0xFFFF 的字符的四字节表示法，由 UTF-16编码方法决定。

## 实例属性

String.prototype.charAt 返回指定位置的字符，参数从 0 开始编号的位置，默认值也是 0，不能是负数或者大于等于字符长度否则返回空字符串。

String.prototype.charCodeAt 返回字符串指定位置的 Unicode 码点对应的十进制数，相当于 String.fromCharCode 的逆操作。参数默认值为 0，参数若为负数或者大于等于字符串长度则返回 NaN。charCodeAt 方法返回的 Unicode 码点不回大于 65535 （0xFFFF），也就是说，只返回两个字节的字符的码点。如果遇到码点大于 65535 的字符（四个字节的字符），必须连续使用两次 charCodeAt，不仅读入 charCodeAt(i)，还要读入 charCodeAt(i+1)，将两个值放在一起。

String.prototype.concat 连接多个字符串，返回一个新的字符串，不改变原来的字符串。如果参数不是字符串，会先转换为字符串再连接。

```js
var one = 1;
var two = 2;
var three = '3';

''.concat(one, two, three) // "123"
one + two + three // "33"
```

String.prototype.slice 从字符串取出指定的子字符串。
第一个参数是子字符串的开始位置，第二个参数是子字符串的结束位置（不含该位置）
如果参数是负数，表示从结尾开始倒数计算的位置
如果第二个参数小于第一个参数，返回空字符串

String.prototype.substring 同 slice 也是用于从字符串中取出指定的字符串。
第一个参数是子字符串的开始位置，第二个参数是子字符串的结束位置（不含该位置）
如果参数是负数，substring 会自动将负数砖为 0
如果第二个参数小于第一个参数，substring 会自动更换两个参数的位置

**所以 substring vs slice，优先选择 slice，substring 违反直觉。**

String.prototype.substr 同 slice、substring 也是用于从字符串取出指定的字符串。
第一个参数是子字符串开始的位置，第二个参数是子字符串的长度。
如果第一个参数是负数，表示倒数计算字符的位置
如果第二个参数是负数，将被自动转为 0

String.prototype.indexOf，String.prototype.lastIndexOf 索引另一个字符串在字符串中出现的位置。第二个参数表示开始索引的位置。

String.prototype.trim 用于去除字符串两端的空格（包括制表符`\t`,`\v`、换行符`\n`、回车符`\r`），返回一个新字符串，不改变原字符串。

String.prototype.toLowerCase，String.prototype.toUpperCase 将字符串该为小写，大写，返回一个新字符串，不改变原字符串。

String.prototype.match 用于确定原字符串是否匹配某个子字符串，返回一个数组，成员为匹配的第一个字符串，如果没有返回 null。返回的数组还有 index 属性和 input 属性，分别表示匹配字符串开始的位置和原字符串。

```js
var matches = 'cat, bat, sat, fat'.match('at');
matches.index // 1
matches.input // "cat, bat, sat, fat"
```

String.prototype.search 同 match，但是返回值为匹配的第一个位置。如果没有匹配到，则返回 -1
String.prototype.replace 用于替换匹配的子字符串，一般只替换第一个匹配，如果加了 g 修饰符则替换全部匹配

String.prototype.split 按照给定的字符串返回一个由分割出来的子字符串组成的数组。
如果分割字符串为空字符串，则返回数组的成员是原字符串的每一个字符串
如果省略参数，则返回的数组的唯一成员就是原字符串
split 的第二个参数，限定返回数组的最大成员数

String.prototype.localCompare 用于比较两个字符串。它返回一个整数，如果是小于 0，表示第一个字符串小于第二个字符串；如果等于 0，表示两者相等；如果大于 0，表示第一个字符串大于第二个字符串。
按照自然语言的顺序进行比较。
第二个参数指定使用的语言。

```js
'B' > 'a' // false
'B'.localeCompare('a') // 1
```