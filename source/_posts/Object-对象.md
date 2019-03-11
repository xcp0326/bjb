---
title: Object 对象
category:
date: 2018-11-26 23:48:43
---

> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

JavaScript 的所有其他对象都是继承自 Object 对象的，即所有对象都是 Object 的实例。

Object 对象的原生方法分成两类：Object 本身的方法与 Object 的实例方法。

**Object 对象本身的方法** 就是定义在 Object 对象上的方法，又称为静态方法。

**Object 的实例方法** 就是定义在 Object.prototype 上的方法，又称为实例方法。

## Object 函数

Object 本身也是一个函数，可以当作工具方法使用，将任意值转为对象。如果参数为空（或者 undefined 和 null），Object() 返回一个空对象。

如果参数是一个原始类型的值，Object 方法将其转换为对应的包装对象的实例

```js
var obj = Object(1)
obj instanceof Object // true
obj instanceof Number // true
```

如果参数是一个对象，直接返回这个对象

```js
function isObject(value) {
  return value === Object(value)
}
isObject([]) // true
```

## Object 构造函数

Object 不仅可以当作工具函数使用，还可以当作构造函数使用。

传入参数的用法和 Object 函数式一样的，如果是对象则直接返回对象，如果是原始类型则返回该类型的包装对象。

不够虽然用法相同，但是 Object(value) 与 new Object(value) 两者的语义是不同的，Object(value) 表示将 value 转换为对象，new Object(value) 则是表示新生成一个对象，它的值是 value。

## Object 的静态方法

### Object.keys()、Object.getOwnPropertyNames()

Object.keys 和 Object.getOwnerPropertyNames 方法都是用来遍历对象的属性的。

Object.keys 方法遍历某个对象自身的可枚举的属性名，返回属性名的数组

Object.getOwnerPropertyNames 方法则是遍历某个对象自身的属性名，包括不可枚举的属性名，返回这些属性名的数组。

```js
Object.keys([1,2])
// [ '0', '1' ]
Object.getOwnPropertyNames([1,2])
// [ '0', '1', 'length' ]
```

### 其他方法

#### 对象属性模型相关方法

Object.getOwnPropertyDescriptor()：获取某个属性的描述对象

Object.defineProperty()：通过描述对象，定义某个属性

Object.defineProperties()：通过描述对象，定义多个属性

#### 控制对象状态的方法

Object.preventExtensions()： 防止对象扩展

Object.isExtensible()：判断对象是否可以扩展

Object.seal()：禁止对对象进行配置

Object.isSealed()：判断一个对象是否可配置

Object.freeze()：冻结一个对像

Object.isFrozen()：判断一个对象是否被冻结

#### 原型链想法方法

Object.create()：传入参数指定原型对象和属性，返回一个新对象

object.getPrototypeOf()：获取对象的 Prototype 对象

## Object 的实例方法

### Object.prototype.valueOf() 

返回当前对象对应的值，默认情况下返回对象本身。

```js
var obj = new Object()
obj.valueOf() === obj // true
```

valueOf 的主要用途是 JavaScript 自动类型转换时候会调用这个方法。

### Object.prototype.toString() 

一般情况下，都是返回当前对象的字符串形式  "[object Object]"。数组、字符串、函数、Date 对象都有自己的 toString 方法，覆盖了 Object.prototype.toString() 方法，它们并不回返回 "[object Object]"

toString 方法会返回对象的类型字符串，可以用来判断一个值的类型。值有自带的 toString 方法，所以这里通过 call 调用 Object.prototype.toString 方法。

```js
Object.prototype.toString.call(2) // "[object Number]"
Object.prototype.toString.call('') // "[object String]"
Object.prototype.toString.call(true) // "[object Boolean]"
Object.prototype.toString.call(undefined) // "[object Undefined]"
Object.prototype.toString.call(null) // "[object Null]"
Object.prototype.toString.call(Math) // "[object Math]"
Object.prototype.toString.call({}) // "[object Object]"
Object.prototype.toString.call([]) // "[object Array]"
Object.prototype.toString.call(/[a-z]/) // "[object RegExp]"
Object.prototype.toString.call(new Date()) // "[object Date]"
Object.prototype.toString.call(Date) // "[object Function]"
```

### Object.prototype.toLocaleString() 

返回当前对象对应的本地字符串形式。数组，数值，日期的实例对象重写了 toLocaleString 方法。

```js
var date = new Date()
date.toString()
// 'Mon Nov 26 2018 23:45:27 GMT+0800 (China Standard Time)'
date.toLocaleString()
// '11/26/2018, 11:45:27 PM'
```

Object.prototype.hasOwnProperty() 判断某个属性是否为当前对象自身的属性

Object.prototype.isPrototypeOf() 判断当前对象是否为另一个对象的原型

Object.prototype.propertyIsEnumerable() 判断某个属性是否可枚举