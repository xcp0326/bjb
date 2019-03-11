---
title: 实例对象与 new 命令
category:
date: 2019-02-28 11:34:39
tags:
---

> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

面向对象编程（Object Oritented Programming，缩写 OOP）是目前主流的编程范式。它将真实世界各种复杂的关系，抽象为一个个对象，然后由对象之间的分工与合作，完成对真实世界的模拟。

每个对象都是功能中心，具有明确分工，可以完成接受信息、处理数据、发出信息等任务。对象可以复用，通过继承机制还可以定制。因此，面向对象编程具有灵活、代码可复用、高度模块化等特点，容易维护和开发，比起由一系列函数或指令组成的传统的过程式编程（procedural programming），更适合多人合作的大型软件项目。

那么，“对象”（object）到底是什么？我们从两个层次来理解。

（1）对象是单个实物的抽象。

一本书、一辆汽车、一个人都可以是对象，一个数据库、一张网页、一个与远程服务器的连接也可以是对象。当实物被抽象成对象，实物之间的关系就变成了对象之间的关系，从而就可以模拟现实情况，针对对象进行编程。

（2）对象是一个容器，封装了属性（property）和方法（method）。

属性是对象的状态，方法是对象的行为（完成某项任务）。比如，我们可以把动物抽象成 animal 对象，使用属性记录具体是哪一种动物，使用方法表示动物的某种行为（奔跑、捕猎、休息等等）。

### 构造函数

面向对象编程的第一步，就是要生成对象。前面说过，对象是单个实物的抽象。通常需要一个模板，表示某一个类实物的共同特征，然后对象根据这个模板生成。

典型的面向对象编程语言（比如 C++ 和 Java），都有类（class）这个概念。所谓类就是对象的模板，对象就是类的实例。但是 JavaScript 语言的对象体系，不是基于类的，而是基于构造函数和原型链的。

JavaScript 语言使用构造函数作为对象的模板。所谓构造函数，就是专门用来生成实例对象的函数。它就是对象的模板，描述实例对象的基本结构。一个构造函数，可以生成多个实例对象，这些实例对象都有相同的结构。

构造函数就是一个普通函数，但是有它自己的特征和用法

```js
var Vehicle = function() {
  this.price = 1000;
};
```

Vehicle 就是构造函数。为了与普通函数区别，构造函数名字的第一个字母要大写。

构造函数的特点有两个：

- 函数体内部使用了 this 关键字，代表了所要生成的对象实例
- 生成对象的时候，必须使用 new 命令。

### new 命令

new 命令的作用，就是执行构造函数，返回一个实例对象。

```js
var Vehicle = function() {
  this.price = 1000;
};

var v = new Vehicle();
v.price = 1000;
```

通过 new 命令，让构造函数 Vehicle 生成一个实例对象，保存在变量 v 中。这个新生成的实例对象，从构造函数 Vehicle 得到了 price 属性。new 命令执行时，构造函数内部 this，就代表了新生成的实例对象，this.price 表示实例对象有一个 price 属性，值为 1000。

使用 new 命令时，根据需要，构造函数也可以接受参数。

```js
var Vehicle = function (p) {
  this.price = p;
};

var v = new Vehicle(500);
```

new 命令本身就可以执行构造函数，所以后面的构造函数可以带括号，也可以不带括号。下面两行代码是等价的，但是为了表示这里是函数调用，推荐使用括号。

```js
var v = new Vehicle()
var v = new Vehicle // 不推荐
```

如果忘记了 new 命令，直接调用构造函数，构造函数会变成普通函数，并不会生成实例对象。如下代码，this 这时代表全局对象，将造成一些意想不到的结果。

```js
var Vehicle = function() {
  this.price = 1000;
}
var v = Vehicle()
v // undefined
price // 1000 此时 Vehicle 的 this 指向的是全局对象
```

可以在构造函数体内第一行加上 use strict 给构造函数使用严格模式，这个时候如果忘记了 new，引擎会报错。

或者... 在构造函数内部判断是否使用 new 命令，如果发现没有使用，则直接返回一个实例对象。

```js
function Fubar(foo, bar) {
  if (!(this instance of Fubar)) {
    return new Fubar(foo, bar);
  }
  this._foo = foo;
  this._bar = bar;
}
Fubar(1, 2)._foo // 1
(new Fubar(1, 2))._foo // 1
```

这样就不管加没加 new 命令，都会得到同样的结果了。

#### new 命令的原理

使用 new 命令时，它后面的函数依次执行下面的步骤：

1. 创建一个空对象，作为将要返回的对象实例
2. 将这个空对象的原型，指向构造函数的 prototype 属性
3. 将这个空对象赋值给函数内部的 this 关键字
4. 开始执行构造函数内部的代码

也就是说，构造函数内部，this 指的是一个新生成的空对象，所有针对 this 的操作，都会发生在这个空对象上。构造函数之所以叫“构造函数”，就是说这个函数的目的，就是操作一个空对象（this 对象），将其“构造”为需要的样子。

如果构造函数内部有 return 语句，而且 return 后面跟着一个对象，new 命令会返回 return 语句指定的对象；否则不管 return 语句，返回 this 对象。

```js
var Vehicle = function () {
  this.price = 1000;
  return 1000;
};

(new Vehicle()) === 1000
// false
```

return 返回 1000，new 命令直接忽略掉了这个 return 语句，返回构造后的 this 对象。

但是，如果是 return 语句返回的是一个跟 this 无关的新对象，new 命令会返回这个新对象，而不是 this 对象。

```js
var Vehicle = function (){
  this.price = 1000;
  return { price: 2000 };
};

(new Vehicle()).price
// 2000
```

如果是普通函数（内部没有 this 关键字的函数）使用 new 命令，则会返回一个空对象。

```js
functino a() {
  return 'a'
}
var aa = new a()
aa // {}
typeof aa // "object"
```

new 命令简化的内部流程，可以用下面的代码表示：

```js
function _new(constructor, params) {
  var args = [].slice.call(arguments);
  var constructor = args.shift();
  var context = Object.create(constructor.prototype);
  var result = constructor.apply(context, args);
	return (typeof result === 'object' && result !== null) ? result : context;
}

// 实例
var actor = _new(Person, '张三', 28)
```

#### new.target 

函数内部可以使用 new.target 属性。如果当前函数是 new 命令调用，new.target 指向当前函数，否则为 undefined。

```js
function f(){
  console.log(new.target ==== f)
}
f() // false
new f() // true
```

使用这个属性，可以判断函数调用的时候，是否使用 new 命令

```js
function f() {
  if (!new.target) {
    throw new Error('请使用 new 命令调用！')
  }
  // ...
}
f() // Uncaught Error: 请使用 new 命令调用！
```

### Object.create() 创建实例对象

构造函数作为模板，可以生成实例对象。但是，有时拿不到构造函数，知恩感拿到一个现有的对象。我们希望以这个现有的对象作为模板，生成新的实例丢下，这时就可以使用 Object.create 方法。

```js
var person1 = {
  name: '张三',
  age: 38,
  greeting: function() {
    console.log('Hi! I\'m' + this.name + '.')
  } 
};

var person2 = Object.create(person1)
person2.name // 张三
person2.greeting() // Hi! I'm 张三
```

