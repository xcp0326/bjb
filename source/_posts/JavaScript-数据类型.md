---
title: JavaScript 数据类型
date: 2018-12-05 15:12:37
tags:
---


JavaScript 除了 ES6 的 Symbol 类型，一共还有 6 种数据类型，分别是原始类型 string、number、boolean、undefined、null，引用类型 object。

## string 字符串

字符串类型是用单引号或者双引号包含的 0 个或者多个字符就是字符串。如果单引号内再出现单引号，需要转义，如果双引号内再出现双引号，也需要转义一下。转义即在需要转义的引号前加反斜杆。

多行字符串，在切换换行的结尾需要加入反斜杆，并且反斜杆后不能出现空格或者其他字符。一般多行字符的时候，会把字符分开多个单行字符然后用 + 连接。还有，因为 function.toString 方法不会过滤掉注释，可以把字符串写在注释内，再调用该方法，已经 split，slice，join 合作实现多行字符串。或者用 ES6 的模版字符串实现。

转义字符反斜杆后除了在转义引号内的同类引号，还可以加 b 后退键 \b，\f 换页符，\0 null，\t \v 制表符，以及 `\\`反斜杆。还可以后面 3 个八进制数值转义八进制 \777，十六进制 \x00a9。加正常字符的时候直接返回字符。

字符串是类数组，它有 length 方法，也可以通过 str[index] 获取第 index 位的字符

Unicode 码点 0000 到 FFFF 之间的字符是 2 个字节，而 10000 到 10FFFF 之间的字符是 4 个字节。但 JavaScript 内部都是以 UTF-16 的格式存储的，它只能识别 2 个字节。如果一个字符是 4 个字节的字符，JavaScript 会不识别，会认为它是 2 个字符。

ASCII 0 到 31 码点的符号是无法打印出来的，可以用 Base64 编码转成 0 ～ 9、a ～ z、A ～ Z、+ 和 `/` 这 64 个字符组成的可打印字符。JavaScript 的 btoa 方法将字符串转为 Base64 编码字符串，atob 方法将 Base64 编码字符串转为字符串原来的值。如果是非 ASCII 码字符串转 Base64 编码，需要先使用 encodeURIComponent 转码（这个转码会转义出来字母、数字、`(`、`)`、`.`、`~`、`!`、`*`、`'`、`-` 和 `_` 之外的所有字符。），然后再对 encodeURIComponent 转码后的字符串调用 btoa 进行 Base64 编码。

## number 数值

数值分为，整数，有限小数，无限小数。在 JavaScript 内部，所有数值都是以 64 位浮点数存储的，整数也是如此，所以 1.0 是全等于 1 的。除了进行位运算时，JavaScript 会把运算子转为 32 位整数进行运算，最后也返回 32 位整数。

64 位浮点数是国际标准组织 IEEE 754 提出来的，又称为双精度浮点数。它的规则是：

- 第 1 个二进制位表示符号，表示是否为负数，如果是负数值 为 1，如果是正数值为 0
- 第 2 位到第 12 位二进制位，共 11 位表示浮点数的指数部分
- 第 13 位到第 64 位二进制位，共 52 位表示小数部分，也称有效数字部分

符号位是数值的正负，指数部分决定了数值的大小，小数部分决定了数值的精度。

指数部分共有 11 个二进制位，它能表示的指数是 0 ～ 2<sup>11</sup> - 1。IEEE 754 规定，如果指数在 (0, 2470) 区间，那么有效数字的第一位默认总是 1，所以有效数字最长可以存储 53 位。64 位浮点数可以存储 -2<sup>53</sup> 到 2<sup>53</sup> 的十进制数值。2<sup>53</sup> 等于 9007199254740992，是一个 16 位的十进制数，所以可以简单理解为 JavaScript 可以精确处理的 15 位的十进制数值。

根据标准 64 位浮点数的指数部分的长度是 11 个二进制位，指数部分最大值为 2047，分一半给负数。所以 JavaScript 能表示的数值范围是 2<sup>1024</sup> 到 2<sup>-1023</sup> ，超出范围的数值将无法表示。如果大于等于 2<sup>1024</sup> 会正向溢出返回 Infinity，如果小于等于 2<sup>-1075</sup> （指数部分最小值 -1023 加上小数部分 52 位），会负向溢出返回 0。

数值小数点前的数字多余 21 位，或者小数点后的零多余 5 位，数值会自动转换为科学计数法。

进制的前缀：八进制 octal 0o 或者 0O，二进制 binary 0B 或者 0b，十六进制 hex 0X 或者 0x，以及没前缀的十进制 decimal。

+0 和 -0 除了符号位不一样，以及 +0 被当分母返回 +Infinity，-0 当分母返回 -Infinity 外，其他的运算都是一样，值也是全等于的。

NaN 的任何运算都是返回 NaN。数字运算中除了加法，其他运算时，如果运算子不是数值会返回 NaN；0 / 0 返回 NaN。

Infinity 加减乘除 0，-0，null、undefined，NaN、infinity，-Infinity

```js
Infinity + -0 // Infinity
Infinity - -0 // Infinity
-0 - Infinity // -Infinity
Infinity * -0 // NaN
Infinity / -0 // -Infinity
-0 / Infinity // 0

// null 参与运算会被转为数值 0，所以同 Infinity 与 null 的运算就是 Infinity 和 0 之间的运算
Infinity + 0 // Infinity
Infinity - 0 // Infinity
0 - Infinity // -Infinity
Infinity * 0 // NaN
Infinity / 0 // Infinity
0 / Infinity // 0

// undefined 参与运算会被转为数值 NaN，所以同 Infinity 与 undefined 的运算就是 Infinity 和  NaN 的运算
Infinity + NaN // NaN
Infinity - NaN // NaN
NaN - Infinity // NaN
Infinity * NaN // NaN
Infinity / NaN // NaN
NaN / Infinity // NaN

Infinity + Infinity // Infinity
Infinity - Infinity // NaN
Infinity * Infinity // Infinity
Infinity / Infinity // NaN

Infinity + -Infinity // NaN
Infinity - -Infinity // Infinity
-Infinity - Infinity // -Infinity
Infinity * -Infinity // -Infinity
Infinity / -Infinity // NaN
-Infinity / Infinity // NaN
```

parseInt 字符串转换为整数，先把参数转换为字符串，然后再转换为整数。
如果参数是 0x 开始的十六进制数值，会先被转换成十六进制的数值再转换为整数返回，其他进制的是返回 0
如果参数是 + 好开始后面接了数值的字符串，会返回这个数值，如果没接，返回 NaN
如果参数是可以自动转换为科学计数法的数值，会转换为科学计数法的数值转换字符串再转换为整数。
如果参数里面有空格，会自动忽略。

parseInt 的第二个参数是转换为整数时，规定的整数的进制，进制只能是在 2 到 36 之间的。超出返回 NaN，如果是 0，null，undefined 直接忽略。

isNaN 是先将参数转换为数值，再判断数值是不是 NaN，所以如果是参数是字符串，是对象，是非空且不是只有一个数值成员项的数组时，会返回 NaN。所以判断 isNaN 应该先判断 typeof args 是否是 number，再执行 isNaN。或者更简洁的，因为 NaN 是唯一一个不等于自己的值 直接判断 args 是否全等于 args，不等于就是 NaN 了。

isFinite 判断一个值是否是正常数值。

## null 空值、undefined 未定义、boolean 布尔值

null 表示空值，而 undefined 表示未定义，都是没有的意思，非运算都是返回 false。null 转为数值是 0，undefined 转为数值是 NaN。

六个值转为 boolean 是返回 false 的：undefined、null、false、0、NaN、''

## object 对象

对象是一组键值对组成的无序复合数据集合。键名是字符串，可以不加引号，可以是数字，因为会自动转为字符串，如果键名不符合标识符条件，须要给键名加上引号。

行首是大括号一律解释为对象，理解为一个表达式，不过为了避免歧义，最好在大括号外再套一个圆括号。

delete 命令只有在对象存在某个属性而且不可以删除时，才返回 false，其他都是返回 true。

in 运算符判断对象是否有某个属性，无所谓属性是继承还是自身的

for...in 遍历对象所有属性包括继承来的，不可遍历的属性不会遍历

with 语句内部赋值变量，这个变量必须是当前对象已有的属性，否则会创建一个全局变量。