---
title: RegExp 对象
tags:
  - 正则
category:
date: 2018-12-20 22:08:36
---

> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

正则表达式 regular expression 是一种表达文本模式（即字符串结构）的方法，有点像字符串的模板，常常用来按照“给定模式”匹配文本。

新建正则表达式有两种方法。一种是使用字面量，以斜杆表示开始和结束。另一种是使用 RegExp 构造函数。这两种写法是等价的。他们的主要差别是，字面量方法在**引擎编译代码时新建正则表达式**，而构造函数是在**运行时新建正则表达式**，所以前者的效率更高，而且便利直观。

## 实例属性

3 个只读的修饰符是否设置的属性：RegExp.prototyp.ignoreCase、 RegExp.prototype.global、RegExp.prototype.multiline。

RegExp.prototype.lastIndex：返回一个整数，表示下一次开始搜索的位置。该属性可读写，但是只在进行连续搜索时才有意义。

RegExp.prototype.source 返回正则表达式的字符串形式，不包括反斜杆，该属性只读。

## 实例方法

RegExp.prototype.test 返回一个布尔值，表示当前模式是否匹配参数字符串。如果正则表达式带了 g 修饰符，则每一次 test 方法都从上一次结束的位置开始向后匹配。带有 g 修饰符时，可以通过正则实例对象的 lastIndex 属性指定开始搜索的位置。

```js
var r = /x/g
var s = '_x_x'

r.lastIndex = 4 // 从第 5 个位置开始搜索
r.test(s) // 这个位置是空的，所以返回 false
```

注意 带有 g 修饰符时，正则表达式内部会记住上一次的 lastIndex 属性，这时不应该更换要匹配的字符串，否则会有一些难以察觉的错误。

```js
var r = /bb/g;
r.test('bb') // true
r.test('-bb-') // false 
```

如果正则构造函数模式时是一个空字符串，则匹配所有字符串。

```js
new RegExp('').test('abc') // true
/''/.test('abc') // false
```

RegExp.prototype.exec 用来返回匹配结构。如果发现匹配，就返回一个数组，成员是匹配成功的子字符串，否则返回 null

如果正则表达式包含圆括号，即含有“组匹配”，则返回的数组会包括多个成员。第一个成员是整个匹配成功的结果，后面的成员就是圆括号对应的匹配成功的组。也就是说，第二个成员对应第一个括号，第三个成员对应第二个括号，以此类推。整个数组的 length 属性等于组匹配的数量再加 1。

```js
var s = '_x_x'
var r = /_(x)/
r.exec(s) // ['_x', 'x']
```

exec 方法的返回数组还包含两个属性：

- input：整个原字符串
- index：整个模式匹配成功的开始位置（从 0 开始计数）

```js
var r = /a(b+)a/
var arr = r.exec('_abbba_aba_')

arr // ["abbba", "aba"]

arr.index // 1
arr.input // "_abbba_aba_"
```

如果正则表达式加上 g 修饰符，则可以使用多次 exec 方法，下一次搜索的位置从上一次匹配成功结束的位置开始。

```js
var reg = /a/g
var str = 'abc_abc_abc'

var r1 = reg.exec(str) // ["a", index: 0, input: "abc_abc_abc", groups: undefined]
r1.index // 0
reg.lastIndex // 1

var r2 = reg.exec(str) // ["a", index: 4, input: "abc_abc_abc", groups: undefined]
r1.index // 4
reg.lastIndex // 5

var r3 = reg.exec(str) // ["a", index: 8, input: "abc_abc_abc", groups: undefined]
r1.index // 8
reg.lastIndex // 9

var r4 = reg.exec(str) // ["a", index: 8, input: "abc_abc_abc", groups: undefined]
r4 // null
reg.lastIndex // 0
```

正则实例对象的 lastIndex 属性重置为 0，lastIndex 不仅可读，还可写。设置了 g 修饰符的时候，只要手动设置了 lastIndex 的值，就会从指定位置开始匹配。

## 字符串的正则方法

String.prototype.match：返回一个数组，成员是所有匹配的子字符串。如果没有匹配值返回 null。如果正则表达式带了 g 修饰符，会一次性返回所有匹配成功的结果，也因此 lastIndex 是无效的。

String.prototype.search：按照给定的正则表达式或者字符串搜索，返回一个整数，表示匹配开始的位置。如果没有就返回 -1。如果参数不传或者空字符串，返回 0。String.prototype.indexOf，String.prototype.lastIndexOf 也是搜索子字符串，不过参数必须是字符串，不支持正则。可用来消除首尾两端空格。

String.prototype.replace：按照给定的正则表达式替换，返回替换后的字符串。

第二个参数可以用美元符号：

- `$&` 匹配的子字符串
- <code>$`</code> 匹配结果前面的文本
- `$'` 匹配结果后面的文本
- `$n` 匹配成功的第 n 组内容，n 是从 1 开始的自然数
- `$$` 指代美元符号 $

```js
'hello world'.replace(/(\w+)\s(\w+)/, '$2 $1')
// "world hello"

'abc'.replace('b', '[$`-$&-$\']')
// "a[a-b-c]c"
```

第二个参数可以是函数

```js
var a = 'The quick brown fox jumped over the lazy dog.';
var pattern = /quick|brown|lazy/ig;

a.replace(pattern, function replacer(match) {
  return match.toUpperCase();
});
// The QUICK BROWN fox jumped over the LAZY dog.
```

第二个参数函数可以接受多个参数。其中，第一个参数是捕捉到的内容，第二个参数开始是捕捉到的组匹配（有多少个组匹配，就有多少个对应的参数）。最后面还有 2 个参数，对应匹配到的子字符串在原字符串中的偏移量，最后一个参数是被匹配的原字符串。

```js
function replacer(match, p1, p2, p3, offset, string) {
  // p1 is nondigits, p2 digits, and p3 non-alphanumerics
  return [p1, p2, p3].join(' - ');
}
var newString = 'abc12345#$*%'.replace(/([^\d]*)(\d*)([^\w]*)/, replacer);
console.log(newString);  // abc - 12345 - #$*%
```

String.prototype.split：按照给定的正则进行字符串分割，返回一个数组，包含分给后的各个成员。

```js
'aaa**a*'.split(/a*/)
// ["", "*", "*", "*"]
```

第一个分隔符是`aaa`，第二个分隔符是0个`a`（即空字符），第三个分隔符是`a`，所以将字符串分成四个部分。

如果正则表达式带有括号，则括号匹配的部分也会作为数组成员返回。

```js
'aaa*a*'.split(/(a*)/)
// [ '', 'aaa', '*', 'a', '*' ]
```

## 匹配规则

某个字符只表示它字面的含义，就叫做字面量字符

元字符：

- 点字符.，点字符匹配除了回车符、换行符、行分隔符和段落分隔符以外的所有字符。大于 0xFFFF 的字符，点字符不能正确匹配，会认为是两个字符。
- 位置字符：^ 表示字符串开始位置，`$` 表示字符串结束的位置
- 选择符 |

转义字符，在 RegExp 构造正则时，转义需要使用两个斜杆，因为字符串内部会先转义一次。

```js
(new RegExp('1\+1')).test('1+1')
// false

(new RegExp('1\\+1')).test('1+1')
// true
```

不能打印的特殊字符：

- `\cx` 表示 `ctrl-[x]` 其中 x 时 A - Z 之间的任意一个英文字母，用来匹配控制字符
- `[\b]` 匹配退格键 U+0008
- `\n` 换行符
- `\r` 回车符
- `\t` 制表符 tab U+0009
- `\v` 垂直制表符 U+000B
- `\f` 换页符 U+000C
- `\0` null 字符 U+0000
- `\xhh` 匹配一个两位十六进制表示的字符 （\x00 - \xFF）
- `\uhhhh`  匹配一个四位十六进制表示的 Unicode 字符 （\u0000 - \uFFFF）

脱字符 ^：

脱字符只有在字符类的第一个位置才有特殊含义，否则就是字面含义。

如果方括号内的第一个字符是 [^]，则表示除了字符类之中的字符，其他字符都可以匹配。
如果方括号内没有其他字符，即只有[^]，就表示匹配一切字符，其中包括换行符。相比点号作为元字符是不包括换行符的。

```js
var s = 'Please yes\nmake my day!';

s.match(/yes.*day/) // null
s.match(/yes[^]*day/) // [ 'yes\nmake my day']
```

连字符 - ：

某些情况下，对于连续序列的字符，连字符 - 用来提供简写形式，表示字符的连续范围。比如 [a-d] 表示 [abcd]。
[1-31] 不代表 1 到 31，只表示 1 - 3 之间的数值。
可以用来指定 Unicode 字符的范围。

[A-z] 表面上看是选中大写 A 到小写 z 之间的 52 个字母，但是由于在 ASCII 编码之中，大写字母与小写字母之间还有其他字符，结果可能会匹配出错。

```js
/[A-z]/.test('\\')  // true
```

预定义模式：几个常见模式的简写方式

- `\d` : 匹配 0 - 9 之间的任一数值
- `\D`： 匹配 0 - 9以外的字符
- `\w`： 匹配任意字母、数字、下划线
- `\W`： 除了字母、数字、下划线以外的字符
- `\s`：匹配空格（包括换行符、制表符、空格符等）
- `\S`：匹配除了空格以外的字符
- `\b`：匹配词的边界
- `\B`：匹配非词的边界，即在词的内部匹配

```js
// \s 的例子
/\s\w*/.exec('hello world') // [" world"]

// \b 的例子
/\bworld/.test('hello world') // true
/\bworld/.test('hello-world') // true
/\bworld/.test('helloworld') // false

// \B 的例子
/\Bworld/.test('hello-world') // false
/\Bworld/.test('helloworld') // true
```

通常，正则表达式遇到换行符 \n 就会停止匹配。

```js
var html = "<b>Hello</b>\n<i>world!</i>";

/.*/.exec(html)[0]
// "<b>Hello</b>"
```

因为点字符不匹配换行符，所以没能匹配到整个 HTML 语句。

改用 `\s` 字符类，就可以了：

```js
var html = "<b>Hello</b>\n<i>world!</i>";

/[\S\s]*/.exec(html)[0]
// "<b>Hello</b>\n<i>world!</i>"
```

`[\S\s]` 指代一切字符！`[\D\d]` 和 [\W\w] 也能达到这个效果。

重复类 `{m, n}`: 精确匹配不少于 n 次，不多余 m 次。

量词符：

- `?` 表示某个模式出现 0 或 1 次，等同于{0, 1}
- `*` 表示某个模式出现 0 次或多次，等同于 {0,}
- `+` 表示某个模式出现 1 次或多次，等同于 {1,}

贪婪模式：

量词符默认情况下都是最大可能匹配，即匹配直到下一个字符不满足匹配位止。这被称为贪婪模式。

```js
var s = 'aaa';
s.match(/a+/) // ["aaa"]
```

上面代码中，模式是`/a+/`，表示匹配1个`a`或多个`a`，那么到底会匹配几个`a`呢？因为默认是贪婪模式，会一直匹配到字符`a`不出现为止，所以匹配结果是3个`a`。

如果想将贪婪模式改为非贪婪模式，可以给量词符后面加一个问号

```js
'abb'.match(/ab+/) // ["abb"]
'abb'.match(/ab+?/) // ["ab"]

'abb'.match(/ab*b/) // ["abb"]
'abb'.match(/ab*?b/) // ["ab"]

'abb'.match(/ab?b/) // ["abb"]
'abb'.match(/ab??b/) // ["ab"]
```

修饰符：

g 修饰符可以多次调用 test 方法。exec 也可以多次调用。

```js
var regex = /b/g;
var str = 'abba';

regex.test(str); // true
regex.test(str); // true
regex.test(str); // false
```

i 修饰符：不区分大小写

m 修饰符：表示多行模式

```js
/^b/m.test('a\nb') // true
```

加上`m`修饰符以后，`^`和`$`还会匹配行首和行尾，即`^`和`$`会识别换行符（`\n`）。

```js
/world$/.test('hello world\n') // false
/world$/m.test('hello world\n') // true
```

组匹配：

```js
var m = 'abcabc'.match(/(.)b(.)/);
m
// ['abc', 'a', 'c']
```

上面代码中，正则表达式`/(.)b(.)/`一共使用两个括号，第一个括号捕获`a`，第二个括号捕获`c`。

注意，使用组匹配时，不宜同时使用`g`修饰符，否则`match`方法不会捕获分组的内容。

```js
var m = 'abcabc'.match(/(.)b(.)/g);
m // ['abc', 'abc']
```

使用正则表达式的`exec`方法，配合循环，才能读到每一轮匹配的组捕获。

```js
var str = 'abcabc';
var reg = /(.)b(.)/g;
while (true) {
  var result = reg.exec(str);
  if (!result) break;
  console.log(result);
}
// ["abc", "a", "c"]
// ["abc", "a", "c"]
```

组匹配非常有用，下面是一个匹配网页标签的例子。

```js
var html = '<b class="hello">Hello</b><i>world</i>';
var tag = /<(\w+)([^>]*)>(.*?)<\/\1>/g;

var match = tag.exec(html);

match[1] // "b"
match[2] // " class="hello""
match[3] // "Hello"

match = tag.exec(html);

match[1] // "i"
match[2] // ""
match[3] // "world"
```

非捕获组：`(?:x)` 表示非捕获组，表示不返回该组匹配的内容，即匹配的结果中不计入这个括号。

```js
// 正常匹配
var url = /(http|ftp):\/\/([^/\r\n]+)(\/[^\r\n]*)?/;

url.exec('http://google.com/');
// ["http://google.com/", "http", "google.com", "/"]

// 非捕获组匹配
var url = /(?:http|ftp):\/\/([^/\r\n]+)(\/[^\r\n]*)?/;

url.exec('http://google.com/');
// ["http://google.com/", "google.com", "/"]
```

先行断言：`x(?=y)` 称为先行断言，x 值有在 y 前面才匹配，y 不计入返回结果。

```js
// 匹配 c 前面的 b
var m = 'abc'.match(/b(?=c)/);
m // ["b"]
```

先行否定断言：`x(?!y)`， x 只有不在 y 前面才匹配，y 不计入返回结果。

```js
// 匹配不在小数点前面的数值
/\d+(?!\.)/.exec('3.14')
// ["14"]
```

“先行否定断言”中，括号里的部分是不会返回的。

```js
// 匹配不在 c 前面的 b
var m = 'abd'.match(/b(?!c)/);
m // ['b']
```

