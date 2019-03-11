---
title: Promise 对象
date: 2019-03-07 17:03:50
tags:
---


> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

Promise 对象是 JavaScript 的异步操作解决方案，为异步操作提供统一接口。它起到代理作用（proxy）🤔️，充当异步操作与回调函数之间的中介，使得异步操作具备同步操作的接口。Promise 可以让异步操作写起来，就像写同步操作的流程，而不必一层层嵌套回调函数。

Promise 的设计思想是，所有异步任务都返回一个 Promise 实例。Promise 实例有一个 then 方法，用来指定下一步的回调函数。

```js
// 传统写法
step1(function (value1) {
  step2(value1, function(value2) {
    step3(value2, function(value3) {
      step4(value3, function(value4) {
        // ...
      });
    });
  });
});

// Promise 的写法
(new Promise(step1))
  .then(step2)
  .then(step3)
  .then(step4);
```

### Promise 对象的状态

- pending 异步操作进行中
- fulfilled 异步操作成功，Promise 实例传回一个值（如果有）
- rejected 异步操作失败，Promise 实例抛出一个错误

其中 fulfilled 和 rejected 合在一起称为 resolved（表示已定型）

Promise 实例的状态变化只可能发生一次，从 pending 到 fulfilled 或者 pending 到 rejected。

### Promise 构造函数

Promise 构造函数接受一个函数作为参数，该函数的两个参数分别是 resolve 和 reject。它们是两个函数，由 JavaScript 引擎提供，不需要自己实现。

在异步操作成功时调用  resolve 函数，它会将 Promise 实例的状态从 pending 变为 fulfilled，并将异步操作的结果作为参数传递出去。
在异步操作失败时调用 reject 函数，它会将 Promise 实例的状态从 pending 变为 rejected，并将异步操作报出的错误，作为参数传递出去。

### 实例：图片预加载

```js
var preloadImage = function(path) {
  return new Promise(function(resolve, reject) {
    var image = new Image()
    image.onload = resolve
    image.onerror = reject
    image.src = path
  })
}

preloadImage('http://xxx.com/x.jpg')
	.then(function(e) {document.body.append(e.target)})
	.then(function(e) {console.log('加载成功')})
```

### 小结

Promise 的优点在于，让回调函数变成了规范的链式写法，程序流程可以看得很清楚。它有一整套接口，可以实现许多强大的功能，比如同时执行多个异步操作，等到它们的状态都改变以后，再执行一个回调函数；再比如，为多个回调函数中抛出的错误，统一指定处理方法等等。

Promise 还有一个传统写法没有的好处：它的状态一旦改变，无论何时查询，都能得到这个状态。这意味着，无论何时为 Promise 实例添加回调函数，该函数都能正确执行。所以，你不用担心是否错过了某个事件或信号。如果是传统写法，通过监听事件来执行回调函数，一旦错过了事件，再添加回调函数是不会执行的。

### 微任务

Promise 的回调函数不是正常的异步任务，而是微任务（microtask）。它们的区别在于，正常任务追加到下一轮事件循环，微任务追加到本轮事件循环。这意味着，微任务的执行时间一定早于正常任务。

```js
setTimeout(function() {
  console.log(1);
}, 0);

new Promise(function (resolve, reject) {
  resolve(2);
}).then(console.log);

console.log(3);
// 3
// 2
// 1
```

