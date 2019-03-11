---
title: JavaScript 的基本语法
tags:
  - 语句
  - 变量
  - 表达式
  - label
  - 标签
  - 循环
  - 三元表达式
date: 2018-11-16 12:49:17
---

> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

## 1. 语句

wiki 上[語句 (程式設計)](https://zh.wikipedia.org/wiki/語句_(程式設計))是这么说的

> 一个语句是指令式编程语言中最小的独立元素，表达程序要运行的一些动作。

JavaScript 程序的执行单位是行，一行一行地执行。一般情况下，JavaScript 每一行就是一个语句。

### 1.1 语句和表达式

语句是为了完成某种任务而进行的操作，比如下面就是一行赋值语句。

```js
var a = 1 + 3;
```

这条语句先用`var` 命令，声明了变量`a`，然后将`1 + 3`的运算结果赋值给变量`a`。

`1 + 3` 叫做表达式，表达式是指一个为了得到返回值的运算式。语句和表达式的区别是，语句时为了进行某种操作，一般情况下没有返回值；而表达式则是为了得到返回值，一定会有一个返回值。凡是 JavaScript 语言中预期为值的地方，都可以使用表达式。比如，赋值语句的等号右边，预期是一个值，因此可以放置各种表达式。

语句以分号结尾，一个分号表示一个语句的结束。用分号结束时，多个语句可以写在一行，也就是多个语句之间用分号隔离开时，这些语句可以写在一行：

```js
var a = 1 + 3; var b = 'abc';
```

定义多个变量时，也可以是用逗号隔开，最后语句再用分号结尾。JavaScript 部署上线 minify 时就是这样的。

```js
var a = 1 + 3, b = 'abc';
```

分号前面没有任何内容，JavaScript 引擎将其视为空语句。

```js
;;; // 啥也不会执行
```

表达式不需要分号结尾。一旦在表达式后添加了分号，则 JavaScript 引擎就将表达式视为语句，这样就会产生一些没有意义的语句

```js
1 + 3;
'abc';
```

上面两行语句只是单纯地产生一个值，对程序并没有任何实际的意义。

## 2. 变量

### 2.1 概念

可以变的量就是变量，它是对“值”的具名引用。变量就是为“值”起名，然后引用这个名字，就等同于引用这个值，对这个名字的操作就是对这个值的操作。变量的名字就是变量名。

```js
var a = 1; // 用 var 命令声明变量 a，然后在变量 a 与数值 1 之间建立引用关系，成为数值 1 赋值给变量 a。
var b = a + 1; // 用 var 命令声明变量 b，然后引用变量 a，再加 1，因为引用变量 a 就是引用的数值 1，所以再加 1，得到的结果就是 2，所以与变量 b 建立引用关系的是 2.
```

变量名是区分大小写的

变量没有声明直接调用，JavaScript 会报错，告诉你变量未定义。

```js
x
// ReferenceError: x is not defined
```

可以在同一条 var 命令中声明多个变量。

```js
var a, b;
```

JavaScript 是动态类型语言，变量的类型是没有限制的，随时可以更改类型。

```js
var a = 1;
a = 'hello';
```

使用 var 重新声明一个已经存在的变量是无效的。

### 2.2 变量提升

JavaScript 引擎的工作方式是，先解析代码，获取所有被声明的变量，然后再一行一行地运行。这造成的结果，就是看起来所有变量的声明语句，都会被提升到代码作用域最开始的位置，这就叫做变量提升。

```js
console.log(a);
var a = 1;
```

JavaScript 引擎执行真正运行的代码是这样的：

```js
var a; 
console.log(a); // console 的时候，a 还没有被赋值，所以打印出来的结果是 undefined
a = 1;
```

## 3. 标识符

标识符 identifier 指的是用来识别各种值的合法名称。最常见的标识符就是变量名，以及后面函数名。

JavaScript 语言的标识符对大小写敏感。

JavaScript 语言的标识符有一套命名规则，不符合规则的就是非法标识符。JavaScript 遇到非法标识符就会报错。

标识符的规则如下：

- 第一个字符，可以是任意的 Unicode 字母（包括英文字母和其他语言的字母），以及美元符号和下划线。
- 第二个字符及后面的字符，除了 Unicode 字母、美元符号和下划线，还可以用数字 0 - 9。

合法的标识符：

```js
var Ʃ = 1 // Ʃ 是拉丁文大写字母 Esh https://unicode-table.com/cn/#01A9
var π = 2 // π 是希腊文大写字母 Pi https://unicode-table.com/cn/#03A0
var $0 = ''
var _xiao = 'xiao'
```

中文是合法的标识符：

```js
var 我就是帅 = true;
```

JavaScript 有一些保留字，不能用作标识符：arguments、break、case、catch、class、const、continue、debugger、default、delete、do、else、enum、eval、export、extends、false、finally、for、function、if、implements、import、in、instanceof、interface、let、new、null、package、private、protected、public、return、static、super、switch、this、throw、true、try、typeof、var、void、while、with、yield

## 4. 注释

```js
// 单行注释
/*
多行
注释
*/
```

JavaScript 兼容 HTML 代码的注释，所以 `<!--` 和 `-->`是合法的单行注释

```js
x = 1; <!-- x = 2;
--> x = 3;
```

上面的代码只有 `x = 1`会执行，其他部分都注释掉了。

注意 `—>`只有在行首才能被当成单行注释，否则正常运算

```js
function countdown(n) {
  while (n --> 0) console.log(n);
}
countdown(3)
// 2
// 1
// 0
```

上面的代码 `n-->0`实际上被当成了 `n-->0`，递减 n `n-—` 然后再 `n > 0` 比较，所以得到了 2、1、0

## 5. 区块

JavaScript 用大括号将多个相关的语句组合在一起，成为区块。但是 JavaScript 的区块并不会构成单独的作用域。JavaScript 里一般在 for、if、while、function 等场景使用区块

## 6. 条件语句

### 6.1 if 结构

if 结构先判断一个表达式的布尔值，然后根据布尔值的真伪，执行不同的语句。

```js
if (m === 3)
  m = m + 1;
```

只有 m 等于 3 时，才会给 m 加上 1

这种写法要求条件表达式后面只能有一个语句。如果像执行多个语句，必须在 if 的条件判断之后，加上大括号，表示代码块（多个语句合并成一个语句）

```js
if (m === 3) {
  m = m + 1
  return m
}
```

如果 if 布尔表达式写成了赋值表达式：

``` js
if (x = 2) {
  console.log('yayaya');
}
```

结果会把 2 赋值给 x，然后再判断 x 的值的布尔值，x 的值是 2，所以它的布尔值为 true，所以这里结果会打印出来 yayaya

### 6.2 if...else 结构

### 6.3 switch 结构

多个 if...else 连在一起使用的时候，可以转为更方便的 switch 结构。

``` js
var a = 1;
if (a == 1) {
  console.log(1)
} else if (a == 2) {
  console.log(2);
}
// 转换成 switch
switch (a) {
  case 1:
    console.log(1)
    break;
  case 2:
    console.log(2)
    break;
  default:
    break;
}
```

switch 结构中，case 代码块之中要加 break 语句，否则会一直执行下去。

switch 语句部分和 case 语句部分都可以使用表达式。

```js
switch (1 + 3) {
  case 2 + 2:
    console.log(1)
   	break;
  default:
    console.log('default');
    break;
}
```

switch 语句后的表达式与 case 语句后的表达式比较运行结果时，是用的严格相等运算符 ===

### 6.4 三元运算符 ?:

## 7. 循环语句

### 7.1 while 循环

```js
var i = 0 // 循环的起点
while (i < 100) { // i < 100 为终止条件
  console.log(i);
  i = i + 1; // 递增表达式
}
```

### 7.2 for 循环

```js
for (var i = 0; i < 100; i = i + 1) {
  console.log(i);
}
```

可以省略任何一个，也可以全部省略，全部省略就形成了一个无限循环。

```js
// 省略一个
var i = 0;
for (; i < 100; i = i + 1) {
  console.log(i);
}
// 省略两个
var i = 0;
for (;i < 100;) {
  console.log(1)
  i = i + 1
}
// 全部省略
for (;;) {
  console.log('死循环')
}
```

### 7.3 do...while 循环

不管真假都会执行一次，while 后面的分号不要省略。

```js
var i = 0;
do {
  console.log(i);
  i = i + 1;
} while (i < 100)
```

### 7.4 break 语句和 continue 语句

break 语句和 continue 语句都是跳转作用，让代码块不按既有的顺序执行。

break 语句用于跳出代码块或者整个循环，continue 语句用于终止本轮循环。不带参数的 break 语句和 continue 语句只能跳出最内层的循环。

### 7.5 标签 label

语句的前面有标签，相当于定位符，用于跳转到程序的任意位置。

```js
foo: {
  console.log(1);
  break foo;
  console.log(2);
}
```

代码执行，break foo 后就会跳出代码块，所以打印结果是 1，不会有 2

```js
top:
for (var i = 0; i < 3; i++) {
  for (var j = 0; j < 3; j++) {
    if (i === 1 && j === 1) continue top;
    console.log('i=' + i + ', j=' + j);
  }
}
// i = 0, j = 0
// i = 0, j = 1
// i = 0, j = 2
// i = 1, j = 0 
// i = 2, j = 0
// i = 2, j = 1
// i = 2, j = 2
```

可以看到 i 等于 1 和 j 等于 1 之后就跳出循环进入到下一轮 i 等于 2 的循环了。