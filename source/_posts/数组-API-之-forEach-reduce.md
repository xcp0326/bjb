---
title: 数组 API 之 forEach、reduce
tags:
  - forEach
  - reduce
category:
  - JavaScript
date: 2018-12-18 23:31:15
---


## forEach 的实现

一个数组 arr 调用 forEach 方法，并传入 callback 方法：
```js
  const arr = [1,2,3]
  arr.forEach(function(value, index) {
    console.log(value, index)
  })
  // 1 0
  // 2 1
  // 3 2
```
可以看到 callback 里面并没有传入 arr，那 callback 是怎么获取到数组 arr 的 value 和 index 的了。

它是通过 this 关键字来传入的。JavaScript 已经帮我们把 arr 传入了，类似调用了函数的 call 方法，并把 call 方法的第一个参数 this 指向 arr。如下：
```js
  arr.forEach(function(){})
  // 等同于
  arr.forEach.call(arr, function(){})
```

我们的 forEach 的实现
```js
  // 我们的 forEach 实现
  obj.forEach = function(x) {
    for (let i = 0; i < obj.length; i++) {
      x(obj[i], i)
    }
  }
  // 有个 obj
  var obj = {
    0: 1,
    1: 2,
    length: 2
  }
  // 调用 forEach 方法：
  obj.forEach(function(value, key) {
    console.log(value, key)
  })
  // 1 0
  // 2 1
```
所以从数据结构来看，数组和对象其实没有差别。

## arr.sort
arr.sort 原地排序自身，可以省空间。数组的其他 API 都不会修改原数组。

数组的 API 返回一个数组，所以可以操作成链式操作。

## reduce
举个列子，范伟火车上打劫 “IC、IP、IQ 卡通通告诉我密码。” 😂，火车上有 10 个人。

| 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    | 9    | 10   |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 100  | 200  | 30   | 10   | 20   | 200  | 100  | 30   | 10   | 30   |

范伟手里最开始是么有钱的，钱数为 0，即初始值为 0
当打劫完第 1 个人，钱数变为了 0 + 100 = 100
打劫完第 2 个人，钱数变成了 100 + 200 = 300
打劫完第 3 个人，钱数就变成了 300 + 30 = 330
…
就是也就是累加。把之前打劫的钱加上新的打劫的钱。

```js
const arr = [100, 200, 30, 10, 20, 200, 100, 30, 10, 30]
arr.reduce((preValue, currentValue) => {
  return preValue + currentValue
}, 0)
// 730
```

map 和 filter 都可以用 reduce 表示。

### map 用 reduce 表示

数组所有值乘以 2

```js
const arr = [100, 200, 30, 10, 20, 200, 100, 30, 10, 30]
arr.reduce((prevValue, currentValue) => {
  prevValue.push(currentValue * 2)
  return prevValue
}, [])
```

### filter 用 reduce 表示

比如过滤大于 100 的数值

```js
const arr = [100, 200, 30, 10, 20, 200, 100, 30, 10, 30]
arr.reduce((prevValue, currentValue) => {
  if (currentValue > 100) {
    prevValue.push(currentValue)
  }
  return prevValue
}, [])
```

所以说，map 和 filter 其实这是 reduce 的一个实现。