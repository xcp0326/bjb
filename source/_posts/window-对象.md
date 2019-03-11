---
title: window 对象
date: 2019-03-11 14:44:41
tags:
---


浏览器里面，window 对象指的是当前的浏览器窗口。它也是当前页面的顶层对象，所有其他对象都是它的下属。一个变量如果未声明，那么默认就是顶层对象的属性。

window 有自己的实体含义，其实不适合当作最高一层的顶层对象，这是一个语言的设计失误。最早，设计这门语言的时候，原始设想是语言内置的对象越少越好，这样可以提高浏览器的性能。因此，语言设计者 Brendan Eich 就把 window 对象当作顶层对象，所有未声明就赋值的变量都自动变成 window 对象的属性。这种设计使得编译阶段无法检测出未声明变量。

### window 对象的属性

#### window.name

窗口不一定需要名字，这个属性主要配合超链接和表单的 target 属性使用。

该属性只能保存字符串，如果写入的值不是字符串，会自动转成字符串。各个浏览器对这个值得储存容量有所不同，但是一般来说，可以高达几个 MB。

只要浏览器窗口不关闭，这个属性就不会消失的。举例来说，访问 a.com 时，该页面的脚本设置了 window.name，接下来在同一个窗口里面载入了 b.com，新页面的脚本可以读到上一个网页设置的 window.name，就算页面刷新也是。但是如果浏览器关闭后，该属性保存的值就会消失，因为这时窗口已经不存在了。

#### window.closed、window.opener

window.closed 一般用来检查使用脚本大开的新窗口是否关闭。

window.opener 表示打开当前窗口的父窗口。如果当前窗口没有父窗口（即直接在地址栏输入打开），则返回 null。

如果父窗口与当前窗口并不需要通信，建议将子窗口的 opener 属性显式设为 null，这样可以减少一些安全隐患。

通过 opener 属性，可以获得父窗口的全局属性和方法，但只限于两窗口同源的情况，且其中一个窗口由另一个打开。`<a>` 元素添加 `rel="noopener"` 属性，可以防止新打开的窗口获取父窗口，减轻被恶意网址修改父窗口 URL 的风险。

#### window.self、window.window

都是指窗口本身，这两个属性都是只读。❕显然还有很多其他信息 ….

#### window.frames、window.length

1. window.frames 返回页面内所有框架窗口，包括 frame 元素和 iframe 元素。window.frames[0] 获取第一个框架窗口。如果 iframe 设置了 id 或 name 属性，可以通过用属性值引用这个 iframe 窗口。frames[id]、frames[name]、frames.id、frames.name
2. frames 是 window 对象的别名。frames[0] 等价于 window[0]。而 window.length 返回的就是当前网页包含的框架总数。

```js
window.frames.length === window.length === frames.length
```

#### window.frameElement

#### window.top、window.parent

#### window.status

#### window.devicePixelRatio

表示一个 CSS 像素的大小与一个物理像素的大小之间的比率。也就是说，它表示一个 CSS 像素有多少个物理像素组成。它可以用与判断用户的显示环境，如果这个比率较大，就表示用户正在使用高清屏幕，因此可以显示较大像素的图片。

#### 位置大小属性

window.screenX、window.screenY 返回浏览器窗口左上角相对于当前屏幕左上角的水平距离和垂直距离（单位像素）。

window.innerHeight、window.innerWidth 返回网页在当前窗口可见部分的高度和宽度，即“视口”（viewport）的大小（单位像素）。用户放大网页的时候，这两个属性会变小。因为这时网页的像素大小不变，只是每个像素占的屏幕空间变大了，因此可见部分就变小了。这两个属性值包括滚动条的高度和宽度。

window.scrollX、window.scrollY 返回页面的水平和垂直滚动距离，不是整数，而是双精度浮点数。

window.pageXOffset、window.pageYOffset 是 window.scrollX 、window.scrollY 的别名。

#### 组件属性

window.locationbar、window.menubar、window.scrollbars、window.toolbar、window.statusbar、window.personalbar 都有一个 visible 属性。

#### 全局对象属性

window.document、window.location、window.navigator、window.history、window.localStorage、window.sessionStorage、window.console、window.screen

#### window.isSecureContext

返回当前窗口是否是在 HTTPS 协议的加密环境

### window 对象的方法

#### window.alert()、window.prompt()、window.confirm()

window.prompt() 方法第二个参数是输入框的默认值

#### window.open()、window.close()、window.stop()

open 方法容易被滥用，许多浏览器不允许脚本自动新建窗口。只允许在用户点击链接或按钮时，脚本做出反应，弹出新窗口。因此，有必要检查一下打开新窗口是否成功。

close 一般只用来关闭 open 方法新建的窗口；而且只对顶层窗口有效，iframe 框架中的窗口使用该方法无效。

stop 会停止加载图像、视频等正在或等待加载的对象

#### window.moveTo()、window.moveBy()

moveTo 移动到指定的位置，接受两个数值，对应水平距离和垂直距离。

moveBy 移动到一个相对位置

为了防止有人滥用这两个方法，随意移动用户的窗口，目前只有一种情况，浏览器允许用脚本移动窗口：该窗口时用 window.open 方法新建的，并且它所在的 tab 页时当前窗口里面唯一的。除此之外的情况，使用上面两个方法都是无效的。

#### window.resizeTo()、window.resizeBy()

resizeTo 缩放到指定大小

resizeBy 缩放指定的量

#### window.scrollTo()、window.scroll()、window.scrollBy()

scrollTo 接收两个参数时，表示滚动后位于窗口左上角的页面坐标；或者接收一个配置对象作为参数。

```js
window.scrollTo(100, 200)
window.scrollTo({
  top: 100,
  left: 200,
  behavior: 'smooth' // 滚动的方式：smooth、instant、auto
})
```

scroll 是 scrollTo 的别名

scrollBy 滚动指定的距离

滚动某个元素的三个属性和方法：

- Element.scrollTop
- Element.scrollLeft
- Element.scrllIntoView()

#### window.print()

弹出浏览器的打印配置窗口

#### window.focus()、window.blur()

激活窗口，获得焦点/移除焦点

#### window.getSelection()

返回一个 Selection 对象，表示用户现在选中的文本

#### window.getComputedStyle()、window.matchMedia()

getComputedStyle 接收一个元素节点作为参数，返回一个包含该元素的最终样式信息的对象

matchMedia 检查 CSS 的 mediaQuery 语句

#### window.requestAnimationFrame()

与 setTimeout 类似，都是推迟某个函数的执行。不同之处是，setTimeout 必须指定推迟的时间

requestAnimationFrame 则是推迟到浏览器下一次重流时执行，执行完才会进行下一次重绘。重绘通常是 16ms 执行一次，不过浏览器会自动调节这个速率，比如网页切换到后台 tab 页时，就会暂停执行

如果某个函数会改变网页布局，一般放在 window.requestAnimationFrame() 里面执行，这样可以节省系统资源，使得网页效果更加平滑。因为慢速设备会较慢的速率重流和重绘，而速度更快的设备会有更快的速率。

```js
window.requestAnimationFrame(callback)
```

callback 是一个回调函数。callback 执行时，它的参数就是系统传如的一个高精度时间戳（performance.now() 的返回值），单位是毫秒，表示距离网页加载的时间。

window.requestAnimationFrame() 返回值是一个整数，这个整数可以传入 window.cancelAnimationFrame() 用来取消回调函数的执行。

```js
var element = document.getElementById('animate');
element.style.position = 'absolute';

var start = null;

function step(timestamp) {
  if (!start) start = timestamp
  var progress = timestamp - start;
  
  element.style.left = Math.min(progress / 10, 200) + 'px'
  if (progress < 2000) {
    window.requestAnimationFrame(step);
  }
}

window.requestAnimationFrame(step)
```

#### window.requestIdleCallback()

window.requestIdleCallback() 跟 setTimeout 类似，也是将某个函数推迟执行，但是它保证将回调函数推迟到系统资源空闲的时候执行。也就是说，如果某个任务不是很关键，就可以使用 window.requestIdleCallback() 将其推迟执行，以保证网页性能。

它跟 window.requestAnimationFrame() 的区别在于，后者指定回调函数在下一次浏览器重排时执行，问题在于下一次重排时，系统资源未必空闲，不一定能保证在 16ms 之内完成；window.requestIdleCallback() 可以保证回调函数在系统资源空闲时执行。

该方法接受一个回调函数和一个配置对象作为参数。配置丢下可以指定一个推迟执行的最长时间，如果过了这个时间，回调函数不管系统是否有空闲，都会执行。

```js
window.requestIdleCallback(callback[, options])
```

callback 函数执行时，系统会传入一个 IdleDeadline 对象作为参数。IdleDeadline 对象有一个 didTimeout 属性（布尔值，表示是否为超时调用）和一个 timeRemaining 方法（返回该空闲时间段剩余的毫秒数）

options 参数是一个配置对象，目前只有 timeout 一个属性，用来指定回调函数推迟执行的最大毫秒数。

window.requestIdleCallback() 方法返回一个整数，可以传入 window.cancelIdleCallback() 取消回调函数。

```js
requestIdleCallback(myNonEssentialWork)

function myNonEssentialWork(deadline) {
  while(deadline.timeRemaining() > 0) {
    doWhileIfNeeded();
  }
}
```

如果多次执行 window.requestIdleCallback()，指定多个回调函数，那么这些回调函数将排成一个队列，按照先进先出的顺序执行。

### 事件

#### load 事件和 onload 属性

load 事件发生在文档在浏览器窗口加载完毕时。window.onload 属性可以指定这个事件的回调函数。

#### error 事件和 onerror 属性

接受 5 个参数：出错信息、出错脚本的网址、行号、列号、错误对象

老式浏览器只支持前三个参数

并不是所有的错误，都会触发 JavaScript 的 error 事件（即让 JavaScript 报错）。一般来说，只有 JavaScript 脚本的错误，才会触发这个事件，而像资源文件不存在之类的错误，都不会触发。

如果整个页面未捕获错误超过 3 个，就显示警告：

```js
window.onerror = function(msg, url, line) {
  if (onerror.num++ > onerror.max) {
    alert('Error:' + msg + '\n' + url + ':' + line);
    return true;
  }
}
onerror.max = 3;
onerror.num = 0;
```

需要注意的是，如果脚本网址与网页网址不在同一个域（比如使用了 CDN），浏览器根本不回提供详细的出错信息，只会提示出错，错误类型是 Script error，行号为 0，其他信息都没有。这时浏览器防止向外部脚本泄漏信息。一个解决方法是在脚本所在的服务器，设置 Access-Control-Allow-Origin 的 HTTP 头信息。

```js
Access-Control-Allow-Origin: *
```

然后在网页的 script 标签中设置 crossorigin 属性

```html
<script crossorigin="anonymous" src"//example.com/file.js"></script>
```

crossorigin=anonymous 表示，读取文件不需要身份信息，即不需要 cookie 和 HTTP 认证信息，如果设为 crossorigin="use-credentials"，就表示浏览器回上次 cookie 和 HTTP 认证信息，同时还需要服务器端打开 HTTP 头信息 Access-Control-Allow-Credentials。

#### window 对象的事件监听属性

除了具备元素节点都有的 GlobalEventHandlers 接口，window 对象还具有一下的事件监听函数属性：onafterprint、onbeforeprint、onbeforeunload、onhashchange、onlanguagechange、onmessage、onmessageerror、onoffline、online、onpagehide、onpageshow、onpopstate、onstorage、onunhandledrejection 未处理的 Promise 对象的 reject 事件的监听函数、onunload

### 多窗口操作

#### 窗口的引用

top：对应顶层窗口；parent：对应父窗口；self：当前窗口，即自身。与这些变量对应，浏览器还提供一些特殊的窗口名，供 window.open() 方法、a 标签、form 标签等引用：`_top` 顶层窗口，`_parent` 父窗口，`_blank` 新窗口。

#### iframe 元素

对于 iframe 嵌入的窗口，document.getElementById 方法可以拿到该窗口的 DOM 节点，然后使用 contentWindow 属性获得 iframe 节点包含的 window 对象。

```js
var frame = document.getElementById('theFrame');
var frameWindow = frame.contentWindow;
```

frame.contentWindow 可以拿到子窗口的 window 对象。然后，在满足同源限制的情况下，可以读取子窗口内部的属性。

```js
frameWindow.title // 获取子窗口的标题
```

iframe 元素的 contentDocument 属性，可以拿到子窗口的 document 对象。

```js
var frame = document.getElementById('theFrame')
var frameDoc = frame.contentDocument
// 等同于
var frameDoc = frame.contentWindow.document
```

iframe 元素遵守同源政策，只有当父窗口与子窗口在同一个域时，两者之间才可以用脚本通信，否则只有使用 window.postMessage 方法。

iframe 窗口内部，使用 window.parent 引用父窗口。如果当前页面没有父窗口，则 window.parent 属性返回自身。因此，可以通过 window.parent 是否等于 window.self，判断当前窗口是否为 iframe 窗口。

iframe 窗口的 window 对象，有一个 frameElement 属性，返回 iframe 在父窗口中的 DOM 节点，对于非嵌入的窗口，该属性是等于 null

#### window.frames

iframe 元素设置了 name 或 id 属性，name 属性的值回自动成为子窗口的名称，可以用在 window.open 方法的第二个参数，或者 a 和 frame 标签的 target 属性。

