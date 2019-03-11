---
title: Object 对象的相关方法
date: 2019-03-06 14:31:34
tags:
---


> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

### Object.getPrototypeOf()

返回参数对象的原型。

```js
var F = function () {}
var f = new F()
Object.getPrototypeOf(f) === F.prototype // true
Object.getPrototypeOf({}) === Object.prototype
Object.getPrototypeOf(Object.prototype) === null
function f() {}
Object.getPrototypeOf(f) === Function.prototype
```

### Object.setPrototypeOf()

为参数对象设置原型，返回该参数对象。接受两个参数，第一个是现有对象，第二个是原型对象。

```js
var a = {}
var b = {x: 1}
Object.setPrototypeOf(a, b)
Object.getPrototypeOf(a) === b // true
a.x // 1
```

用 Object.setPrototypeOf 方法模拟 new 命令：

```js
var F = function () {
  this.foo = 'bar'
}
var f = new F();
// 等同于 ⬇️
// 将一个空对象的原型设为构造函数的 prototype 属性
var f = Object.setPrototypeOf({}, F.prototype)
// 将构造函数内部的 this 绑定到这个空对象
F.call(f)
```

### Object.create()

返回以参数对象为原型的一个实例对象。

```js
function F() {
  this.name = 'F'
}
var f = new F()
var ff = Object.create(f)
// 等价于
var ee = Object.setPrototypeOf({}, f)
ff.constructor === F // true
ff instanceof F // true
```

Object.create 可以用下面的代码代替

```js
if (typeof Object.create !== 'function') {
  Object.create = function(obj) {
    function F() {}
    F.prototype = obj
    return new F()
  }
}
```

下面三种方式生成的新对象是等价的：

```js
var obj1 = Object.create({})
var obj2 = Object.create(Object.prototype)
var obj3 = new Object()
```

如果想要生成一个不具备定义在 Object.prototype 上属性（比如没有 toString 和 valueOf 方法）的对象，可以将 Object.create 的参数设为 null。

Object.create 第二个参数是一个属性描写对象，它所描述的对象属性，会添加到实例对象，作为该对象自身的属性。

### Object.prototype.isPrototypeOf()

判断该对象是否为参数对象的原型

```js
var o1 = {}
var o2 = Object.create(o1)
var o3 = Object.create(o2)

o2.isPrototypeOf(o3)
```

o1 和 o2 都是 o3 的原型。这表明只要实例对象处在参数对象的原型链上，isPrototypeOf 方法都返回 true。

### `Object.prototype.__proto__`

实例对象的 `__proto__` 属性返回该对象的原型。只有浏览器才需要部署，其他环境没有这个属性。它是一个内部属性，要尽量少用，而用 Object.getPrototypeOf() 和 Object.setPrototypeOf() 进行原型对象的读写操作。

### Object.getOwnPropertyNames()

返回对象非继承的自身的属性的键名们

Object.keys 只获取可以遍历的键名们

### Object.prototype.hasOwnProperty()

唯一一个处理对象属性时，不会遍历原型链的方法

### in 运算符和 for…in 循环

in 对象是否有某个属性，不区分是否时在原型链上

for…in 获得对象的所有可遍历属性，自身和继承的。

```js
function inheritedPropertyNames(obj) {
  var props = {}
  while(obj) {
    Object.getOwnPropertyNames(obj).forEach(function(p) {
      props[p] = true
    })
    obj = Object.getPrototypeOf(obj)
  }
  return Object.getOwnPropertyNames(props)
}
```

### 对象的拷贝

- 确保拷贝后的对象，与原对象具有相同的原型
- 确保拷贝后的对象，与原对象具有同样的实例属性

```js
function copyObject(orig) {
  var copy = Object.create(Object.getPrototypeOf(orig))
  copyOwnPropertiesFrom(copy, orig)
  return copy
}

function copyOwnPropertiesFrom(target, source) {
  Object
  	.getOwnPropertyNames(source)
    .forEach(function(propKey) {
    	var desc = Object.getOwnPropertyDescriptor(source, propKey);
    	Object.defineProperty(target, propKey, desc)
  	})
  return target;
}
```

或者 ES2017 Object.getOwnPropertyDescriptors

```js
function copyObject(orig) {
  return Object.create(
    Object.getPrototypeOf(orig),
    Object.getOwnPropertyDescriptors(orig)
  )
}
```

