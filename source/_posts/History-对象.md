---
layout: post
title: History 对象
date: 2019-04-29 11:08:32
tags:
---


> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

- window.history.pushState(state, title, url)
- window.history.replaceState(state, title, url)

state: 一个与添加的记录相关联的状态对象，主要用于 popstate 事件。该事件触发时，该对象会传入回调函数。也就是说，浏览器会将这个对象
序列化以后保留在本地，重新载入这个页面的时候，可以拿到这个对象。如果不需要这个对象，此处可以填 null。如果设置了这个对象，使用了这个方法后，
可以通过 history.state 获取当前 history 的状态对象。

不能插入跨域的网址，会报错。这样设计的目的是，防止恶意代码让用户以为他们是在另一个网站上，因为这个方法不会导致页面跳转。

#### popstate 事件
同一个文档的浏览历史出现变化时，就会触发 popstate 事件。仅仅调用 pushState 方法或者 replaceState 方法，并不会触发该事件，只有用户点击
浏览器倒退按钮和前进按钮，或者使用 JavaScript 调用 history.back()、history.forward()、history.go() 方法时才会触发。

```js
window.addEventListener('popstate', function(event) {
  console.log('location: ' + document.location);
  console.log('state: ' + JSON.stringify(event.state)); // event.state 就是 pushState 和 replaceState 方法设置的 state
})
```
