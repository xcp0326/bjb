---
title: this 关键字
category:
date: 2019-03-04 12:44:24
tags:
---

> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

```js
var person = {
  name: '张三',
  describe: function() {
    return '姓名：' + this.name;
  }
};

person.describe(); // "姓名：张三"
```

this.name 是在 describe 中调用，而 describe 方法所在的当前对象是 person，因此 this.name 就是 person.name。

对象的属性是可以赋值给其他对象的，所以属性所在的当前对象是可变的，即 this 的指向是可变的。

```js
var A = {
  name: '张三',
  describe: function () {
    return '姓名：'+ this.name;
  }
};

var B = {
  name: '李四'
};

B.describe = A.describe;
B.describe()
// "姓名：李四"
```

A.describe 属性被赋值给了 B，于是 B.describe 就表示 describe 方法所在的当前对象 B，所有 this.name 指向的是 B.name

```js
function f() {
  return '姓名：'+ this.name;
}

var A = {
  name: '张三',
  describe: f
};

var B = {
  name: '李四',
  describe: f
};

A.describe() // "姓名：张三"
B.describe() // "姓名：李四"
```

只要函数赋值给另外一个变量，this 的指向就会被改变。

```js
var A = {
  name: '张三',
  describe: function () {
    return '姓名：'+ this.name;
  }
};

var name = '李四';
var f = A.describe;
f() // "姓名：李四"
```

### 为何会存在 this

JavaScript 语言之所以有 this 的设计，跟内存里面的数据结构有关系。

```js
var obj = { foo: 5};
```

上面的代码将一个对象赋值给变量 obj。JavaScript 引擎会先在内存里面，生成一个对象 `{foo:5}` ，然后把这个对象的内存地址赋值给 obj。也就是说，变量 obj 是一个地址，一个引用。后面如果要读取 `obj.foo`，引擎先从 obj 拿到 内存地址，然后再从该地址读出原始的对象，返回它的 foo 属性。

原始的对象以字典结构保存，每一个属性名都对应一个属性描述对象。举例来说，上面例子的 foo 属性，实际上是一下面的形式保存：

```js
{
  foo: {
    [[value]]: 5,
    [[writable]]: true,
    [[enumberable]]: true,
    [[configurable]]: true
  }
}
```

foo 属性的值保存在属性描述对象的 value 属性里面。

如果属性的值是一个函数了？

```js
var obj = {foo: function() {}};
```

这时，引擎会将函数单独保存在内存中，然后将函数的内存地址赋值给 foo 属性的 value 属性。

函数可以在不同的运行环境中执行，所以需要有一种机制，能够在函数体内部获得当前的运行环境 context。所以 this 就出现了，它的设计目的就是在函数体内部，指代函数当前的运行环境。

```js
var f = function() {
  console.log(this.x)
}

var x = 1;
var obj = {
  f: f,
  x: 2
};

f() // 1
obj.f() // 2
```

> 🤔 那和 python 的 self 一样吗？ [Classes](https://docs.python.org/3/tutorial/classes.html)
> self 出现在 class 中。一般 class 所有的实例方法的第一个参数都命名为 self，并指向实例化后 class 的实例。

### 使用场合

1. 指代全局环境

2. 构造函数内部，指代实例化的对象

3. 对象的方法内的 this 指代这个对象
   但是，下面几种用法，会改变 this 的指向：

   ```js
   (obj.foo = obj.foo)() // window
   (false || obj.foo)() // window
   (1, obj.foo)() // window
   ```

   obj.foo 就是一个值。这个值真正调用的时候，运行环境已经不是 obj 了，而是全局环境，所以 this 不再指向 obj。

   可以这样理解，JavaScript 引擎内部，obj 和 obj.foo 储存在两个内存地址，称为地址一和地址而。`obj.foo()` 这样调用时，是从地址一调用地址二，因此地址二的运行环境是地址一，this 指向 obj。但是，上面三种情况，都是直接去除地址二进行调用，这样的话，运行环境就是全局环境，因此 this 指向全局环境。

严格模式下，修改对象的方法内的 this 指向，会报错。

```js
var couner = {
  count: 0
};
counter.inc = function() {
  'use strict';
  this.count++
};
var f = counter.inc;
f();
// TypeError: Cannot read property 'count' of undefined
```

### 绑定 this 的方法

#### Function.prototype.call()

```js
func.call(thisValue, arg1, arg2, ...)
```

call 方法的一个应用是调用对象的原生方法

```js
var obj = {};
obj.hasOwnProperty('toString'); 
obj.hasOwnProperty = function () {
  return true;
}
obj.hasOwnProperty('toString') // true
Object.prototype.hasOwnProperty.call(obj, 'toString') // false
```

#### Function.prototype.apply()

```js
func.apply(thisValue, [arg1, arg2, ...])
```

call 方法中原函数的参数需要一个一个添加，apply 中类数组形式传入。

##### 找出数组中最大的元素

```js
var a = [10, 2,4,15,9]
Math.max.apply(null, a) // 15
```

##### 将数组的空元素变为 undefined

```js
Array.apply(null, ['a',,'b']) // ['a', undefined, 'b']
```
原理是，在 apply 的第二个参数接受的是一个类数组对象，调用的是内置方法 CreateListFromArrayLike 方法 `CreateListFromArrayLike(obj[, elementTypes])` 如果没有定义 elementTypes 时，会被认定为 Undefined、Null、Boolean、String、Symbol、Number、Object。在生成 list 时，调用 `Get(obj, indexName)`。我们这个例子返回的是 elementTypes 里面的 undefined 了。[CreateListFromArrayLike](http://www.ecma-international.org/ecma-262/6.0/#sec-createlistfromarraylike)
然后 空元素与 undefined 差别是，数组的 forEach 方法会跳过空元素，但是不会跳过 undefined。

##### 转换类似数组的对象

```js
Array.prototype.slice.apply({0: 1, length: 1}) // [1]
Array.prototype.slice.apply({0: 1}) // []
Array.prototype.slice.apply({0: 1, length: 2}) // [1, undefined]
Array.prototype.slice.apply({length: 1}) // [undefined]
```

##### 绑定回调函数的对象

```js
var o = new Object();

o.f = function () {
  console.log(this === o);
}

var f = function (){
  o.f.apply(o);
  // 或者 o.f.call(o);
};

// jQuery 的写法
$('#button').on('click', f);
```

点击按钮以后，控制台会显示为 true。由于 apply 方法（或者 call 方法）不仅绑定函数执行时所在对象，还会立即执行函数，因此不得不把绑定语句写在一个函数体内，更简洁的写法是采用下面介绍的 bind 方法。

#### Function.prototype.bind()

```js
var d = new Date()
d.getTime()

var print = d.getTime()
print() // Error....
```

改用 bind 方法

```js
var print = d.getTime.bind(d) 
```

```js
var add = function(x, y) {
  return x * this.m + y * this.n
}
var obj = {
  m: 2,
  n: 2
};
var newAdd = add.bind(obj, 5)
newAdd(5) // 20
```

上面的代码中，bind 方法除了绑定 this 对象，还将 add 函数的第一个参数 x 绑定成 5，然后返回一个新函数 newAdd，这个函数只要再接受一个参数 y 就能运行了。

如果 bind 方法的第一个参数是 null 或 undefined，等于将 this 绑定到全局对象，函数运行时 this 指向顶层对象（浏览器为 window）

```js
function add(x, y) {
  return x + y
}
var plus5 = add.bind(null, 5)
plus5(10) // 15
```

bind 方法每次返回一个新函数，所以在每次添加监听函数的时候，需要赋值给一个变量，好在接下来的时候取消监听。

```js
var listener = o.m.bind(o)
element.addEventListener('click', listener)
//...
element.removeEventListener('click', listener)
```

回调函数中使用

```js
var obj = {
  name: '张三',
  times: [1, 2, 3],
  print: function () {
    this.times.forEach(function (n) {
      console.log(this.name);
    });
  }
};

obj.print()
// 没有任何输出
obj.print = function () {
  this.times.forEach(function (n) {
    console.log(this.name);
  }.bind(this));
};

obj.print()
// 张三
// 张三
// 张三
```

与 call 一起使用

```js
var slice = Function.prototype.call.bind(
  Array.prototype.slice);
slice([1,2,3], 0, 1) // [1]
```

上面代码的含义就是，将 Array.prototype.slice 变成 Function.prototype.call 方法所在的对象，调用时就变成了 Array.prototype.slice.call。类似的写法还可用于其他数组方法。

```js
var push = Function.prototype.call.bind(
  Array.prototype.push);
var pop = Function.prototype.call.bind(
  Array.prototype.pop);

var a = [1 ,2 ,3];
push(a, 4)
a // [1, 2, 3, 4]

pop(a)
a // [1, 2, 3]
```

如果再进一步，将 Function.prototype.call 方法绑定到 Function.prototype.bind 对象，就意味着 bind 的调用形式也可以被改写。

```js
function f() {
  console.log(this.v);
}

var o = { v: 123 };
var bind = Function.prototype.call.bind(
  Function.prototype.bind);
bind(f, o)() // 123
```

