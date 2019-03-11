---
title: Document 节点
category:
date: 2018-12-25 11:40:06
tags:
---

> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

document 节点对象代表整个文档，window.document 属性指向这个对象。只要浏览器开始载入 HTML 文档，该对象就存在了，可以直接使用。

iframe 框架里面的网页，使用 iframe 节点的 contentDocument 属性获取 document 节点

AJAX 操作返回的文档，使用 XMLHttpRequest 对象的 responseXML 属性获取 document 节点

document 对象继承了 EventTarget 接口、Node 接口、ParentNode 接口。以及一些自己的属性和方法。

快捷属性

- document.defaultView 返回 document 所属的 window 对象，如果当前文档不属性 window 对象，就返回 null。
- document.doctype document 对象的第一个子节点是 document.doctype，第二个是 html 标签节点
- document.documentElement 返回当前文档的根节点 root。通常是 document 节点的第二个节点，紧跟在 document.doctype 节点后面。HTML 网页的该属性，一般就是 html 标签节点
- document.head 指向 head 标签节点、document.body 指向 body 标签节点。如果改写它们的值，相当于移除所有子节点
- document.scrollingElement 当前文档整体滚动时，到底是哪个元素在滚动。标准模式下，这个属性返回文档的根元素 document.documentElement 即 html 标签元素。兼容模式下，返回的是 body 标签元素，如果该元素不存在返回 null。
- Document.activeElement 返回当前焦点的 DOM 元素。通常这个属性返回的是 input、textarea、select 等表单元素，如果当前没有焦点元素，则返回 body 标签元素或者 null。
- document.fullscreenElement 返回当前以全屏状态展示的 DOM 元素。如果不是全屏，返回 null。确定视频是否打开全屏：`document.fullscreenElement.nodeName === 'video'` 返回 true 则是全屏。

节点集合属性

document.links、document.forms、document.images、document.embeds、document.plugins、document.scripts、document.styleSheets 返回的都是 HTMLCollection 实例，所以都可以通过 id 或者 name 索引到对应的成员。

文档静态信息属性

- document.documentURI 继承自 Document 接口 可用于所有文档、 document.URL 继承自 HTMLDocument 接口，只能用于 HTML 文档。那对前端来说其实是一样的。
- document.domain 当前文档域名，不包含协议和端口。只有在次级域名菜可以修改这个值，设置完后会导致端口被改为 null。
- document.location
- document.lastModified
- document.title、document.characterSet
- document.referrer 与 HTTP 头信息的 Referer 字段一致
- document.dir 表示文字方向 rtl、ltr
- document.compatMode 浏览器处理文档的模式，可能的值为 BackCompat 向后兼容模式和 CSS1Compat 严格模式。一般网页设置了明确的 DTD，document.compatMode 的值都为 CSS1Compat。

文档状态属性

- document.hidden 窗口最小化、浏览器切换 tab 时返回 true，是 Page Visibility API 引入的，一般配合这个 API 使用

- document.visibilityState 返回文档的可见状态，可以在页面加载时，防止加载某些资源；或者页面不可见时，停掉一些页面功能。

  - visible 页面可见或者部分可见
  - hidden 页面不可见
  - prerender 页面正在渲染，对用户来说不可见
  - unloaed 页面从内存卸载

- document.readyState 返回文档当前的加载状态，loading 加载 HTML 代码阶段（尚未完成解析）、interactive 加载外部资源阶段、complete 加载完成。

  这个属性变化的过程如下：

  1. 浏览器开始解析 HTML 文档，document.readyState 属性等于 loading
  2. 浏览器遇到 HTML 文档中的 script 标签元素，并且没有 async 或者 defer 属性，就暂停解析，开始执行脚本，这时 document.readyState 属性还是等于 loading
  3. HTML 文档解析完成，document.readyState 属性变成 interactive
  4. 浏览器等待图片、样式表、字体文件等外部资源加载完成，一旦全部加载完成，document.readyState 属性变成 complete。

- document.cookie

- document.designMode 控制当前文档是否可编辑，默认为 off，如果设置为 on，用户可以编辑整个文档的内容。

  >  有意思，什么场景 🤔 
  >
  > on 的时候，可以调用 document.execCommaned() 方法来控制文本。

- document.implementation 返回一个 DOMImplementation 对象，有三个方法，主要用于创建独立于当前文档的新的 Document 对象。

  - DOMImplementation.createDocument()：创建一个 XML 文档
  - DOMImplementation.createHTMLDocument()：创建一个 HTML 文档
  - DOMImplementation.createDocumentType()：创建一个 DocumentType 对象

  ```js
  var doc = document.implementation.createHTMLDocument('Title');
  var p = doc.createElement('p');
  p.innerHTML = 'hello world';
  doc.body.appendChild(p);
  
  // 替换当前文档为新建的 doc 文档。有意思啊。
  document.replaceChild(
    doc.documentElement,
    document.documentElement
  );
  ```

方法

- document.open()、document.close() 

- document.write()、document.writeLn()

- document.querySelector()、document.querySelectorAll() 参数为 CSS 选择器

- document.getElementsByTagName() 返回符合条件的标签节点，返回一个 HTMLCollection 实例，如果参数是`*`则返回所有 HTML 元素

- document.getElementsByClassName() 参数如果是多个 class，用空格隔开，则返回同时具有多个 className 的标签节点

- document.getElementsByName() 返回具有某个 name 属性的标签节点的 NodeList 实例

- document.getElementById() 返回 id 为参数的标签元素，效率比 document.querySelector() 高得多

- document.elementFromPoint()、document.elementsFromPoint() 返回页面指定 x、y 值的最上层的元素节点、元素节点的数组

- document.caretPositionFromPoint() 返回一个 CaretPosition 对象，包含了指定坐标点在节点对像内部的位置信息。CaretPosition 对象就是光标插入点的概念，用于确定光标点在文本对象内部的具体位置。

  ```js
  var range = document.caretPositionFromPoint(clientX, clientY);
  ```

  range 是指定坐标点的 CaretPosition 对象。该对象有两个属性：

  - CaretPosition.offsetNode：该位置的节点对象
  - CaretPosition.offset：该位置在 offsetNode 对象内部，与起始位置相距的字符数

- document.createElement()

- document.createTextNode() 传入的参数如果有标签节点会被转义，不对引号转义，不能用来对 HTML 属性赋值

- document.createAttribute()

- document.createComment()

- document.createDocumentFragment() DocumentFragment 是存在于内存的 DOM 片段，不属于当前文档，常常用来生成一段较复杂的 DOM 结构，然后再插入当前文档。这样做的好处在于，因为 DocumentFragment 不属于当前文档，对它的任何改动，都不回引发网页的重新渲染，比直接修改当前文档的 DOM 有更好的性能表现。

- document.createEvent() 创建一个事件对象 Event 实例，可以被 element.dispatchEvent 方法使用，触发指定事件。参数为事件类型：UIEvents、MouseEvents、MutationEvents、HTMLEvents 等

  ```js
  var event = document.createEvent('Event');
  event.initEvent('build', true, true);
  document.addEventListener('build', function (e) {
    console.log(e.type); // "build"
  }, false);
  document.dispatchEvent(event);
  ```

- document.addEventListener()、document.removeEventListener()、document.dispatchEvent() 用于处理 document 节点事件。都继承自 EventTarget 接口

  ```js
  // 触发事件
  var event = new Event('click');
  document.dispatchEvent(event);
  ```

- document.hasFocus() 当前文档之中是否有元素被激活或获得焦点。有焦点的文档必定被激活，但是被激活的文档未必有焦点。比如，用户点击按钮，从当前窗口跳出一个新窗口，该新窗口就是激活的，但是不拥有焦点。

- document.adoptNode() 将参数节点及其子节点从原来所在文档或者 DocumentFragment 移除，让它归属当前 document 对象，返回插入后的新节点。插入的节点对象的 ownerDocument 属性会变成当前的 document 对象，而 parentNode 属性为 null（因为只有插入到文档树后才有 parentNode）。document.adoptNode 方法只是改变节点的归属，并没有将这个节点插入到文档树，所以如果要插入文档树，还要用 appendChild 或者 insertBefore 方法将新节点插入到当前文档树。

- document.importNode() 方法则是从原来所在文档或者 DocumentFragment 里面，拷贝某个节点及其子节点，让它归属当前 document 对象。拷贝的节点对象的 ownerDocument 属性会变成当前的 document 对象，而 parentNode 属性为 null（因为只有插入到文档树后才有 parentNode）。第一个参数为节点，第二个参数为是否是深拷贝。

- document.createNodeIterator() 返回一个子节点遍历器。第一个参数为要遍历的根节点，第二个参数为所要遍历的节点类型。几种主要的节点类型写法如下：

  - 所有节点：NodeFilter.SHOW_ALL
  - 元素节点：NodeFilter.SHOW_ELEMENT
  - 文本节点：NodeFilter.SHOW_TEXT
  - 评论节点：NodeFilter.SHOW_COMMENT

  document.createNodeIterator 方法返回一个遍历器对象（NodeFilter 实例）。该实例的 nextNode() 方法和 previousNode() 方法，可以用来遍历所有子节点。

  ```js
  var nodeIterator = document.createNodeIterator(document.body);
  var pars = [];
  var currentNode;
  
  while (currentNode = nodeIterator.nextNode()) {
    pars.push(currentNode);
  }
  ```

  nextNode 方法先返回遍历器内部指针所在的节点，然后会将指针移向下一个节点
  previousNode 方法先将指针移向上一个节点，然后返回该节点

  ```js
  var nodeIterator = document.createNodeIterator(
  	document.body,
    NodeFilter.SHOW_ELEMENT
  )
  
  var currentNode = nodeIterator.nextNode()
  var previousNode = nodeIterator.previousNode()
  
  // nextNode 方法先返回遍历器内部指针所在的节点，然后会将指针移向下一个节点
  // previousNode 方法先将指针移向上一个节点，然后返回该节点
  // 此时 currentNode === previousNode
  currentNode === previousNode // true
  ```

  遍历器返回的第一个节点是传入的参数节点。

- document.createTreeWalker() 返回一个 DOM 的子树遍历器。与 document.createNodeIterator 方法基本类似，区别在于它返回的是 TreeWalker 实例，而后者是返回的 NodeIterator 实例。另外返回的遍历器不包含参数节点。

- document.execCommand()、document.queryCommandSupported()、document.queryCommandEnabled() 如果 document.designMode 为 on，即整个文档用户都可编辑，如果标签节点的 contenteditable 为 true，即这个标签可编辑。这两种情况下，可以使用 document.execCommand() 方法，改变内容的样式，比如 document.execCommand('bold') 会使得字体加粗。

  ```js
  document.execCommand(command, showDefaultUI, input)
  ```

  该方法接受三个参数：

  - command：字符串，表示要实施的样式

  - showDefaultUI：布尔值，表示是否使用用户默认的界面，建议设置为 false

  - input：字符串，表示该样式的辅助内容，比如生成超级连接时，这个参数就是所要链接的网址。如果第二个参数为 true，那么浏览器会弹出提示框，要求用户在提示框内输入该参数。但是不是所有浏览器都支持这么做，为了兼容性，还是需要自己部署获取这个参数比较好。

    ```js
    var url = window.prompt('请输入网址')
    if (url) {
      document.execCommand('createLink', false, url)
    }
    ```

  document.execCommand() 的返回值是一个布尔值。如果为 false，表示这个方法无法生效。

  这个方法大部分情况下，只对选中的内容生效。如果有多个内容可编辑区域，那么只对当前焦点所在的元素生效。

  document.execCommand() 方法可以执行的样式改变有很多种，下面是其中的一些：bold、insertLineBreak、selectAll、createLink、insertOrderedList、subscript、delete、insertUnorderedList、superscript、formatBlock、insertParagraph、undo、forwardDelete、insertText、unlink、insertImage、italic、unselect、insertHTML、redo。


  document.queryCommandEnabled() 方法返回一个布尔值，表示浏览器是否允许使用这个方法。

  document.queryCommandSupported() 方法返回一个布尔值，表示当前是否可用某种样式改变。比如，加粗只有存在文本选中时才可用，如果没有选中文本，就不可用。

- document.getSelection() 这个方法指向 window.getSelection() 方法。