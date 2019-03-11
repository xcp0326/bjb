---
title: console 对象与控制台
category:
date: 2018-11-24 10:39:06
---

> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

## console 对象

console 对象是 JavaScript 的原生对象，它有点像 Unix 系统的标准输出 stdout 和标准错误 stderr，可以输出各种信息到控制台。并且还提供了很多有用的辅助方法。

console 的常见用途有两个：

- 调试程序，显示网页代码运行时的错误
- 提供了一个命令行接口，用来与网页代码互动

console 对象的浏览器实现，包含在浏览器自带的开发者工具之中。以 Chrome 浏览器的开发者工具 Developer Tools 为例，可以使用下面三种方法来打开它。

1. Alt + Command + i
2. 浏览器菜单选择“工具/开发者工具”
3. 在一个页面元素上，打开右键菜单，选择其中的“Inspect Element”

打开开发者工具以后，顶端有多个面板。

- Elements 查看网页的 HTML 源码和 CSS 代码
- Resources 查看网页加载的各种资源文件（比如代码文件、字体文件、CSS 文件等），以及在硬盘上创建的各种内容（比如本地缓存、Cookie、Local Storage 等）。
- Network 查看网页的 HTTP 的通信情况。
- Sources 查看网页加载的脚本源码
- Timeline 查看各种网页行为随时间变化的情况
- Performance 查看网页的性能情况，比如 CPU 和内存消耗
- Console 用来运行 JavaScript 命令

## Console 对象的静态方法

### console.log、console.info、console.debug

如果 console.log 方法第一个参数是格式字符串，使用了格式占位符，将依次用后面的参数替代占位符，然后再输出

```js
console.log('%s + %s = %s', 1, 1, 2)
// 1 + 1 = 2
```

上面代码中，console.log 方法的第一个参数有三个占位符 %s，第二、三、四个参数会替换掉三个占位符。

Console 还有其他的占位符对应不同的数据类型

- %s 字符串
- %d 整数
- %i 整数
- %f 浮点数
- %o 对象的链接
- %c CSS 格式的字符串

```js
console.log(
  '%cThis text is styled!',
  'color: red; background: yellow; font-size: 24px;'
)
```

如果参数是一个对象，console.log 会显示该对象的值

```js
console.log({foo: 'bar'})
// Object {foo: "bar"}
console.log(Date)
// function Date() { [native code] }
```

上面的代码 Date 输出的对象值是一个构造函数

console.info 是 console.log 的别名，用法完全一样。只不过 console.info 方法会在输出信息的前面，加上一个蓝色图标

console.debug 方法与 console.log 方法类似，会在控制台输出调试信息。但是在默认情况下，console.debug 输出的信息不会显示，只有在打开显示级别在 verbose 的情况下才会显示。

console 对象的所有方法，都可以被覆盖。因此可以按照自己的需要，定义 console.log 方法。

```js
['log', 'info', 'warn', 'error'].forEach(function(method) {
  console[method] = console[method].bind(
    console,
    new Date().toISOString()
  );
});

console.log("出错了！");
// 2018-11-23T15:28:49.233Z 出错了！
```

上面调用 bind 方法，给 console 传入了参数 new Date().toISOString()，标准规定给 bind 方法传入的第 1 个参数以后的参数全部是放在被 bind 的方法的实参之前传递给该方法。所以这里最后显示为 2018-11-23T15:28:49.233Z 出错了！

### console.warn()，console.error()

warn 方法和 error 方法也是在控制台输出信息，它们与 log 方法的不同之处在于 warn 方法输出信息时，在最前面加上一个黄色三角表示警告；error 方法输出信息时，在最前面加一个红色的叉，表示出错。同时，还会高亮显示输出文字和错误发生的堆栈。其他方面都一样。

```js
console.error('Error: %s (%i)', 'Server is not responding', 500)
// Error: Server is not responding (500)
console.warn('Warning! Too few nodes (%d)', document.childNodes.length)
// Warning! Too few nodes (1)
```

可以这样理解，log 方法是写入标准输出（stdout），而 warn 方法和 error 方法是写入标准错误（stderr）。

### console.table

对于某些复合类型的数据，console.table 方法可以将其转为表格显示。

**将数组内容显示为表格**

```js
var languages = [
  { name: "JavaScript", fileExtension: ".js" },
  { name: "TypeScript", fileExtension: ".ts" },
  { name: "CoffeeScript", fileExtension: ".coffee" }
];

console.table(languages);
```

最后在 console 控制台显示为

| index | name           | fileExtension |
| ----- | -------------- | ------------- |
| 0     | "JavaScript"   | ".js"         |
| 1     | "TypeScript"   | ".ts"         |
| 2     | "CoffeeScript" | ".coffee"     |

**将对象内容显示为表格**

```js
var languages = {
  csharp: {name: "C#", paradigm: "object-oriented"},
  fsharp: {name: "F#", paradigm: "functional"}
}
console.table(languages);
```

最后在 console 控制台显示为

| index  | name | paradigm          |
| ------ | ---- | ----------------- |
| csharp | "C#" | "object-oriented" |
| fsharp | "F#" | "functional"      |

### console.count()

count 方法用于计数，输出它被调用的次数。

```js
function greet(user) {
  console.count()
  return 'Hi, ' + user
}
greet('胖达')
// : 1
// "Hi, 胖达"
greet('pandada')
// : 2
// "Hi, pandada"
```

可以接受一个字符串作为参数作为标签，对执行次数进行分类。

```js
function greet(user) {
  console.count(user)
  return 'Hi, ' + user
}
greet('胖达')
// 胖达: 1
// "Hi, 胖达"
greet('pandada')
// pandada: 1
// "Hi, pandada"
```

### console.dir()，console.dirxml()

dir 方法用来对一个对象进行检查，并以易于阅读和打印的格式显示。

```js
console.log({f1: 'foo', f2: 'bar'})
// Object {f1: "foo", f2: "bar"}

console.dir({f1: 'foo', f2: 'bar'})
// Object
//   f1: "foo"
//   f2: "bar"
//   __proto__: Object
```

对输出 DOM 对象非常有用，因为会显示 DOM 对象的所有属性。

```js
console.dir(document.body)
```

![](/images/console/dir-example.png)

Node 环境下还可以指定以代码高亮的形式输出。

```js
console.dir(obj, {colors: true})
```

dirxml 方法主要用于以目录树的形式，显示 DOM 节点

```js
console.dirxml(document.body)
```

![](/images/console/dirxml-example.png)

如果参数不是 DOM 节点，而是普通的 JavaScript 对象，console.dirxml 等同于 console.dir。

### console.assert()

console.assert 方法主要用于程序运行过程中，进行条件判断，如果不满足条件，就显示一个错误，但不会中断程序执行。这样就相当于提示用户，内部状态不正确。

它接受两个参数，第一个参数是表达式，第二个参数是字符串。只有当第一个参数为 false，才会提示有错误，在控制台输出第二个参数，否则不会有任何结果。

```js
console.assert(false, '判断条件不成立')
// Assertion failed：判断条件不成立

// 相当于
try {
  if (!false) {
    throw new Error('判断条件不成立')
  }
} catch (e) {
  console.error(e)
}
```

### console.time()，console.timeEnd()

计算出一个操作所花费的准确时间。

```js
console.time('test ')
for (var i=0; i<100;i++) {
  console.log('..for..')
}
console.timeEnd('test')
// test: xxxxms
```

### console.group()，console.groupEnd()，console.groupCollapsed()

console.group 和 console.groupEnd 用于讲 console 信息分组。只在输出大量信息时有用，分在一组的信息，可以用鼠标折叠/展开。

console.groupCollapsed 方法与 console.group 方法很类似，唯一的区别是该组内容在第一次显示是收起的 collapsed，而不是展开的。

### console.trace()，console.clear()

console.trace 方法显示当前执行的代码在堆栈中的调用路径。

```js
console.trace()
// console.trace
// (anonymous) @ VM1469:1
```

console.clear 方法用于清除当前控制台的所有输出，将光标回置到第一行。如果用户选中了控制台的“Preserve log”选项，console.clear 方法将不起作用。

## 控制台命令行 API

`$_` 返回上一个表达式的值

`$0` - `$4` 控制台保存了最近 5 个在 Elements 面板中的 DOM 元素，`$0` 代表倒数第一个（最近一个），`$1` 代表倒数第二个，以此类推直达 `$4`

`$(selector)` 返回第一个匹配的元素，等同于 `document.querySelector()`

`$$(selector)` 返回选中的 DOM 对象，等同于 `document.querySelectorAll()`

`$x(path)` 返回一个数组，包含匹配特定 XPath 表达式的所有 DOM 元素。

```js
$x('//p[a]') // 返回所有包含 a 元素的 p 元素
```

`inspect(object)` 打开相关面板，并选中相应的元素，显示它的细节。 DOM 元素在 Elements 面板中显示，比如 inspect(document) 会在 Elements 面板中显示 document 元素。

`getEventListeners(object)` 返回一个对象，该对象的成员为 object 登记了回调函数的各种事件（比如 click 或者 keydown），每个事件对应一个数组，数组的成员为该事件的回调函数。

`keys(object)`，`values(object)` 返回一个包含键名，键值的数组

`monitorEvents(object[,events])`，`unmoitorEvents(object[,events])` 监听某个对象的某个事件。事件发生时，会返回一个 Event 对象，包含这个事件的相关信息。unmountorEvent 方法用于停止监听。

```js
monitorEvents(window, "resize");
monitorEvents(window, ["resize", "scroll"])
monitorEvents($0, 'mouse');
unmonitorEvents($0, 'mousemove');
monitorEvents($("#msg"), "key");
```

`clear()` 清除控制台的历史 console.clear() 别名

`copy(object)` 复制 DOM 元素对象到剪贴板

`dir(object)` console.dir 方法别名

`dirxml(object)` console.dirxml 方法别名

## debugger 语句

