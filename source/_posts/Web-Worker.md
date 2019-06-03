---
layout: post
title: Web Worker
date: 2019-06-03 16:33:00
tags:
---


Web Worker 可以在主线程创建 Worker 线程，在主线程运行的同时，Worker 线程在后台同时运行。Worker 线程完成任务后，可以把结果返回给主线程。场景是可以把一些计算密集型或高延迟的任务放在 Worker 线程。而主线程就负责 UI 交互啥的。

Worker 线程一旦新建成功，就会始终运行。

不过，Worker 是有同源限制的，不能访问 DOM，Worker 的全局对象是 WorkerGlobalScope，所有它无法访问到 window 下的大部分对象。Worker 下有定义了 Navigator 和 Location 对象，不能 alert、confirm，有 XMLHttpRequest 对象。Worker 线程是与主线程不在同一个上下文环境时，他们是不能直接通信的，必须通过消息完成（postMessage）。Worker 线程是无法读取本地文件的。

postMessage 发送消息，onMessage 监听发送来的消息。

Worker 内部加载其他脚本，importScripts()

主线程把二进制数据直接转移给 Worker 子线程，一旦转移，主线程将无法再使用之前的二进制数据了，这是为了防止出现多个线程同时修改数据的问题。这种转移数据的方法脚 Transferable Objects。

Worker 载入的是一个单独的 JavaScript脚本文件，但是也可以载入与主线程在同一个网页的代码。

```html
<!DOCTYPE html>
  <body>
    <script id="worker" type="app/worker">
      addEventListener('message', function () {
        postMessage('some message');
      }, false);
    </script>
  </body>
</html>
```

```js
var blob = new Blob([document.querySelector('#worker').textContent]);
var url = window.URL.createObjectURL(blob);
var worker = new Worker(url);

worker.onmessage = function (e) {
  // e.data === 'some message'
};
```

#### Worker 线程完成轮询

```js
function createWorker(f) {
  var blob = new Blob(['(' + f.toString() + ')()']);
  var url = window.URL.createObjectURL(blob);
  var worker = new Worker(url);
  return worker;
}

var pollingWorker = createWorker(function (e) {
  var cache;

  function compare(new, old) { ... };

  setInterval(function () {
    fetch('/my-api-endpoint').then(function (res) {
      var data = res.json();

      if (!compare(data, cache)) {
        cache = data;
        self.postMessage(data);
      }
    })
  }, 1000)
});

pollingWorker.onmessage = function () {
  // render data
}

pollingWorker.postMessage('init');
```

#### Worker 内部再新建 Worker

#### Worker API

Worker 线程的全局对象不是 window，是 WorkerGlobalScope，可以通过 self 直接访问。self.name、self.onmessage、self.onmessageerror、self.close、self.postMessage、self.importScripts