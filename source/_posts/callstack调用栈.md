---
title: 函数调用栈 call stack
tags:
  - 函数
  - call stack
  - stack
category:
  - JavaScript
date: 2018-12-20 13:26:43
---

函数有一个 call stack 调用栈，这个 stack 和内存里面的栈 stack 没关系，call stack 是一种数据结构，它的特点就是先入后出。

栈会记住每次的调用位置。

调用函数时，等执行完后退出该函数，跳回栈记住的调用的位置，跳出该函数，再执行该位置后的其他逻辑。

第一个例子：

```js
function a() { console.log('a') return 'a'}
function b() { console.log('b') return 'b'}
function c() { console.log('c') return 'c'}
a.call()
b.call()
c.call()
```

a.call() 调用栈会记住这个位置，然后进入 a 函数，执行代码，发现有 console.log('a') 的调用，栈把 console.log 的这个调用位置加入到栈里面以记住，console.log 执行完了，栈清除它的这个位置，然后继续执行 a 里面的代码，发现有个 return，说明 a 函数也执行完了，跳回栈 a 函数在调用栈中存的位置，清除 a 在栈中占用的位置，继续执行 a 后面的逻辑，即 b.call()，这时候，调用栈会记住 b 调用的这个位置，然后进入 b 函数，执行代码，发现有个 console.log('b') 的调用，栈把 console.log 的这个调用位置加入到栈里面，console.log 执行完了，栈清除 console.log 在调用栈中存的位置，继续执行 b 后买呢的代码，发现有个 return，说明函数也执行完了，就跳回 b 函数在调用栈中存的位置，清除 b 在栈中占用的位置，继续执行 b 后面的逻辑，即 c.call()，这时候，调用栈会记住 c 调用的这个位置，然后进入 c 函数执行代码，发现有个 console.log('c') 的调用，栈把 console.log 的这个调用位置加入到栈里面，console.log 执行完了，栈清除 console.log 在调用栈中存的位置，继续执行 console.log 后面的逻辑，发现有个 return，说明函数也执行完了，就跳回 c 函数所在的栈中记住的位置，清除 c 函数在栈中存的位置，继续执行 c 后面的代码，发现 c 后面没有代码了，调用栈的工作结束了。

第二个例子：

```js
function a() {
  console.log('a1')
  b.call()
  console.log('a2')
  return 'a'
}
function b() {
  console.log('b1')
  c.call()
  console.log('b2')
  return 'b'
}
function c() {
  console.log('c1')
  return 'c'
}
a.call()
console.log('end')
```

首先是调用 a.call() 调用 a，执行 a 函数，调用栈记住 a.call() 的位置，引擎进入 a 代码执行里面的代码，发现有个 console.log 于是，调用栈记住 console.log 的位置，console.log 执行完，清除 console.log 占用的栈空间，继续执行 console.log 后面的代码，发现有个 b.call()，调用栈记住 b.call() 的位置，引擎进入 b 代码执行里面的代码，发现有个 console.log('b1')，于是，调用栈记住 console.log 的位置，console.log 执行完，清除 console.log 占用的栈空间，继续执行 console.log 后面的代码，发现有个 c.call()，调用栈记住 c.call() 的位置，引擎进入 c 代码执行里面的代码，发现有个 console.log('c1')，调用栈记住 console.log 的位置，console.log 执行完，清除 console.log 占用的栈空间，继续执行 console.log 后面的代码，发现是 return 'c'，那么 c 函数执行完了，跳回调用栈中 c 的位置，清除栈中 c 的位置，继续执行 c.call() 后面的逻辑，执行 console.log，栈记住 console.log 的位置，执行完 console.log，退回 console.log 的位置，清除栈中 console.log 的位置，继续执行后面的逻辑，发现是 return 'b'，那么 b 函数执行完了，跳回调用栈中 b 的位置，清除栈中 b 的位置，继续执行 a.call() 后面的逻辑，执行 console.log，栈记住 console.log 的位置，执行完 console.log，退回 console.log 的位置，清除栈中 console.log 的位置，继续执行后面的逻辑，发现是 return 'a'，那么 a 函数执行完了，跳回调用栈中 a 的位置，继续执行 a.call() 后面的逻辑，并清除栈中 a 的位置，a.call() 后面是一个 console.log，调用栈会记住 console.log 的位置，console.log 执行完，清除栈中的 console.log 的位置，该位置后面没有代码了，调用栈的工作结束了。

第三个例子：

```js
function sum(n) {
  if (n === 1) {
    return 1
  } else {
    return n + sum(n - 1)
  }
}
sum.call(undefined, 5)
```

sum.call(5) 调用栈记住该位置，
进入代码执行 sum 里面的逻辑，发现 n 不等于 1，执行 else 里面的代码，发现 return n + sum(n - 1)，有个 sum(n -1)，然后调用栈记住 sum(n -1) 的位置，其实就是 sum(4)
进入代码执行 sum 里面的逻辑，发现 n 不等于 1，执行 else 里面的代码，发现 return n + sum(n - 1)，有个 sum(n -1)，然后调用栈记住 sum(n -1) 的位置，其实就是 sum(3)
进入代码执行 sum 里面的逻辑，发现 n 不等于 1，执行 else 里面的代码，发现 return n + sum(n - 1)，有个 sum(n -1)，然后调用栈记住 sum(n -1) 的位置，其实就是 sum(2)
进入代码执行 sum 里面的逻辑，发现 n 不等于 1，执行 else 里面的代码，发现 return n + sum(n - 1)，有个 sum(n -1)，然后调用栈记住 sum(n -1) 的位置，其实就是 sum(1)
进入代码执行 sum 里面的逻辑，发现 n 等于 1，return 1，跳回调用栈记住 sum(n -1) 的位置，清空 sum(n - 1) 的位置，跳回 sum(n - 1)，清空sum(n - 1)
跳回 sum(n - 1)，清空sum(n - 1)
跳回 sum(n - 1)，清空sum(n - 1)
跳回 sum(5)，清空sum(5)
最后栈内没有调用了，没有其他代码了。
调用栈的工作结束了。

什么叫 stack overflow 栈溢出，调用栈太多了，就会导致栈溢出。