---
title: JavaScript 语言的历史
tags:
  - JavaScript
  - ECMAScript
date: 2018-11-15 16:30:18
---

> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

## 诞生

JavaScript 因互联网而生

90 年年底，欧洲核能研究组织科学家 Tim Berners Lee 在全世界最大的网络 -- 互联网的基础上发明了万维网，所有的科研人员可以在自己的终端里浏览其他科研人的网页文件。

92 年年第，美国国家超级电脑应用中心开发了第一个图形界面的窗口浏览器 Mosaic。

94 年 10 月，美国国家超级电脑应用中心的一个主程 Marc Andreessen 和 风险投资家 Jim Clark 成立 Mosaic Communication，后来改名 Netscape。

94 年 12 月，Netscape 发布了他们的第一版浏览器 Netscape Navigator 1.0。

Netscape 公司很快发现 Navigator 需要一个可嵌入网页的脚本语言，用来控制浏览器行为。PS：这就是最初 JavaScript 被开发的初衷。谁能想到现在已经发扬光大到这个程度了。

95 年，Netscape 开发人员 Brendan Eich 只用了 10 天就设计完成了这个脚本语言的第一版。它是一个大杂烩“

- 基本语法：借鉴 C 和 Java
- 数据结构：借鉴 Java，将值分为原始值和对象两大类
- 函数的用法：借鉴 Scheme 和 Awk，将函数当作一等公民，并引入闭包
- 原型继承模型：借鉴 Self
- 正则：借鉴 Perl
- 字符串和数组处理：借鉴 Python

就像 [王银](http://www.yinwang.org/blog-cn/2013/03/29/scripting-language) 说的

> 其实“脚本语言”与“非脚本语言”并没有语义上，或者执行方式上的区别。它们的区别只在于它们设计的初衷：脚本语言的设计，往往是作为一种临时的“补丁”。它的设计者并没有考虑把它作为一种“通用程序语言”，没有考虑用它构建大型的软件。

Netscape 公司的这种浏览器脚本语言，最初名字 Mocha
95 年 9 月 改为 LiveScript
12 月，Netscape 与 Sun 公司合作，该脚本语言名字改为 JavaScript

95 年 12 月 4 日，Netscape 与 Sun 联合发布 JavaScript

96 年 3 月，Navigator 2.0 内嵌了 JavaScript

## 与 ECMAScript 的关系

96 年 8 月，微软模仿 JavaScript 发布 JScript，并内嵌于 IE 3.0，Netscape 公司面临失去浏览器脚本语言主导权

96 年 11 月，Netscape 把 JavaScript 提交给 ECMA 国际标准化组织，希望 JavaScript 能够成为国际标准，抵抗微软。ECMA 的 39 号技术委员会负责制定和审核这个标准。

97 年 7 月，ECMA 发布 262 标准文件，规定了浏览器脚本语言的标准，并将这种语言称为 ECMAScript，此次版本为 ECMAScript 1.0

ECMA-262 被 国际标准组织 ISO 批准，标准号为 ISO-16262。

## JavaScript 的版本

97 年 7 月， ECMA 发布的 ECMAScript 1.0

98 年 6 月，ECMASript 2.0

99 年 12 月，ECMAScript 3.0，成为 JavaScript 的通行标准，得到广泛支持。

07 年 10 月，ES 4.0 草案发布，因为对 3.0 的修改太多激进，Yahoo、Microsoft、Google 为首的大公司反对大幅提升，主张小幅改动；而 JavaScript 创造者 Brendan Eich 为首的 Mozilla 公司，则坚持当前的草案。

08 年 7 月，由于各方争论过于激进，ECMA 开会决定，终止 ES 4.0 的开发，其中涉及现有功能改善的一小部分发布为 ES 3.1，而将其他激进的设想扩大范围放入以后的版本，名为 Harmony。后来 ES 3.1 改名为 ES 5.

09 年 12 月，ES 5.0 发布。
Harmony 则一分为二，一些较为可行的设想定名为 JavaScirpt.next 继续开发，后来演变成为 ES 6；而一些不是很成熟的设想，则被视为 JavaScript.next.next，在更远的将来再考虑推出。

11 年 6 月，ES 5.1 发布，并且成为 ISO 国际标准（ISO/IEC 16262:2011）。到 12 年年底，所有主流浏览器都支持 ES 5.1 的全部功能。

13 年 3 月，ES 6 草案冻结，不再添加新功能。新功能放入 ES 7.

13 年 12 月，ES 6 草案发布，然后 12 个月的讨论期，听取各方反馈

15 年 6 月，ES 6 正式发布，更名为 ES 2015

## 周边大事记

96 年 CSS 1.0 发布

97 年 DHML 发布，允许动态改变网页内容。

> 动态HTML技术根据运行地点，分为客户端脚本（也称浏览器脚本）和服务器脚本。
>
> 1. 客户端脚本
>    客户端脚本包括JavaScript技术。是指在某个页面中，通过鼠标点击或键盘操作，与网页产生交互动作；或者在特定时间激发某个事件。客户端脚本在用户的计算机系统里运行。常用于多媒体展示和远程脚本调用。
> 2. 服务器脚本
>    服务器脚本包括ASP, ColdFusion, Perl, PHP, Ruby, WebDNA等技术，一般通过Common Gateway Interface (CGI)来产生动态页面。通过在HTML表单中填写数据，更改URL地址中的参数，更改浏览器的类型等，每次都可能产生不同的页面，称之为动态页面。

98 年，Netscape 开源了浏览器，导致了 Mozilla 项目的诞生。

99 年，IE 5 部署了 XMLHttpRequest 接口，允许 JavaScript 发出 HTTP 请求，为后来 Ajax 应用创造了条件

2000 年，浏览器引擎 KHTML 由 KDE （是一个国际性的自由软件社区）重写，为后来 Webkit 和 Blink 引擎打下基础。10 月 23 日，KDE 2.0 发布，第一次将 KHTML 浏览器 Konqueror 包括其中。

2001 年 微软发布 IE 6

2001 年 Douglas Crockford 提出 JSON 格式，用于取代 XML 格式

2002 年 Mozilla 发布 Firefox 1.0

2003 年 苹果发布 Safari 1.0

04 年 4 月 1 日，Google 发布 Gmail，促成了 Web Application 概念的诞生。

04 年，Dojo 框架诞生，为不同浏览器提供统一接口，并为主要功能提供了便利的调用方法。标志着 JavaScript 编程框架时代来临啦

04 年，WHATWG 成立，致力于 HTML 标准化

05 年，苹果在 KHTML 引擎基础上，建立了 Webkit 引擎

05 年 Ajax Asynchronous JavaScript and XML 诞生，Jesse James Garrett 发明了这个词汇。2 月 Google Maps 项目大量采用这个技术，导致了 Ajax 开始流行，促成了 Web 2.0 时代的来临。

> Web 2.0的应用可以让人了解到目前万维网正在进行的一种改变——从一系列网站到一个成熟的为最终用户提供网络应用的服务平台。

05 年，Apache 发布 CouchDB 数据库，是一个基于 JSON 格式的数据库，可以用 JavaScript 函数定义视图和索引。本质上有别于传统的关系型数据库，标识着 NoSQL 类型的数据库的诞生。

06 年 jQuery 诞生，作者 John Resig，它提供了非常强大易用的操作 DOM 的接口，后来成为了使用最广泛的函数库，并且让 JavaScript 语言的应用难度大大降低，推动了 JavaScript 的流行。

06 年 IE 7 发布

06 年 Google 推出 Google Web Toolkit，提供将 Java 编译成 JavaScript 的先河

07 年 Webkit 引擎在 iPhone 手机上部署

07 年 Douglas Crockford 发表了 《JavaScript：The good parts》，08 年 由 O'Reilly 出版社出版。标志软件行业开始严肃对待 JavaScript，对它的语法开始重新认识。

08 年 V8 编译器诞生，Google 为 Chrome 开发的，特点是让 JavaScript 运行变得非常快。它提高了 JavaScript 的性能，推动了语法的改进和标准化，改变了外界对 JavaScript 的不佳印象。同时 V8 是开源的

09 年，Nodejs 诞生，创始人 Ryan Dahl，JavaScript 可以用于服务端编程

09 年，PhoneGap 诞生，HTML 5 和 JavaScript 可以开发 iOS 和 Andriod 应用

09 年，Google 发布 Chrome Os，允许 JavaScript 直接编写应用程序，类似项目还有 Mozilla 的 Firefox OS

10 年，三个重要项目诞生，分别是 NPM、BackboneJS 和 RequireJS，标志 JavaScript 进入模块化时代

11 年，Windows 8 发布，并直接提供 JavaScript 系统支持

11 年，Google 发布 Dart 语言，目的是为结束 JavaScript 在浏览器脚本中的垄断，它提供更合理、强大的语法和功能。Chromium 浏览器内置 Dart 虚拟机，Dart 程序可以被编译成 JavaScript

12 年，单页面应用程序框架崛起，AngularJS 和 Ember 都发布了 1.0 版

12 年，TypeScript 发布，是 JavaScript 的超集，JavaScript 可以直接在 TypeScript 中运行，而 TypeScript 可以被编译成 JavaScript

12 年，Mozilla 提出 asm.js 规范，给其他语言提供一个编译规范，是其可以被编译成高效的 JavaScript 代码。Mozilla 还发起了 Emscripten 项目 提供一个跨语言的编译器，能够将 LLVM 的位代码 bitcode 转为 JavaScript 代码，LLVM 大部分都是 C/C++ 写的，所以这就意味着 C/C++ 可以在浏览器中运行。还有 LLJS 项目 用于把 JavaScript 转为 C，而River Trail 用于多核心处理器的 ECMAScript 扩展。

13 年 Mozilla 发布手机操作系统 Firefox OS

13 年 ECMA 推出 JSON 国际标准

13 年 5 月，Facebook 发布 UI 框架库 React，引入新的 JSX 语法，使得 UI 层可以用组件开发，同时引入了网页应用是状态机的概念。

14 年，微软推出 JavaScript 的 Windows 库 WinJS，标识微软公司全面支持 JavaScript 与 Windows 操作系统的融合。

14 年 11 月，由于对 Joyent 公司垄断 Node 项目、以及该项目进展缓慢的不满，一部分核心开发离开了 Node.js，创造了 io.js 项目，很快 io.js 2.0 发布。三个月后，Joyent 公司把 Node 转交给 Node 基金会。随后 io.js 项目回归 Node，两个版本合并

15 年 3 月，Facebook 发布 React Native，它能把 JavaScript 转为 iOS 平台的 Objective-C，也能转为 Android 平台的 Java，从而为 JavaScript 开发高性能的原生 App 打开了一条路

15 年 4 月，Angular 宣布 2.0 将基于 TypeScript 开发，意味着 JavaScript 引入强类型

15 年 6 月，ECMAScript 6 改名为 ECMAScript 2015

15 年 6 月，Mozilla 在 asm.js 基础上发布 WebAssembly 项目。这是一种 JavaScript 引擎的中间码格式，全部都是二进制，类似于 Java 的字节码，有利于移动设备加载 JavaScript 脚本，执行速度提高了 20+ 倍。这意味着将来的软件，会发布 JavaScript 二进制包。

16 年 6 月，ECMAScript 2016 标准发布，与 ECMAScript 2015 相比只增加了两个较小的特性：Array.prototype.includes，求幂运算符 `**`

17 年 6 月，ECMAScript 2017 发布，正式引入 async 函数，使得阴部操作的写法出现了根本的变化

17 年 11 月，所有主流浏览器全部支持 WebAssembly，这意味着任何语言都可以编译成 JavaScript 在浏览器运行

18 年 6 月，ECMAScript 2018 发布

真是一段激动人心的历史