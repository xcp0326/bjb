---
title: JavaScript 操作 CSS
category:
date: 2018-12-28 11:37:54
tags:
---

> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

HTML 元素的 style 属性：JavaScript 可以用标签元素的 getAttribute、setAttriubte 和 remoteAttriubte 三个方法来操作。style 不仅可以使用字符串读写，它本身还是一个对象，部署了 CSSStyleDeclaration 接口。

## CSSStyleDeclaration 接口

有三个地方部署了这个接口：

1. 元素节点的 style 属性 Element.style
2. CSSStyle 实例的 style 属性 ***❓没找到 CSSStyle 相关的文章 ❓***
3. window.getComputedStyle() 方法的返回值

CSSStyleDeclaration 接口可以直接读写 CSS 的样式属性，不过，连词号需要变成骆驼拼写法。

CSSStyleDeclaration.cssText 属性用来读写当前规则的所有样式文本。删除一个元素的所有行内样式，最简便的方法就是设置 cssText 为空字符串。

CSSStyleDeclaration.length 属性返回一个整数值，表示当前规则包含了多少条样式声明

CSSStyleDeclaration.parentRule 属性返回当前规则所属的那个样式块 （CSSRule 实例）。如果不存在所属的样式块，该属性返回 null。该属性为只读，且只在使用 CSSRule 接口时才有意义。

CSSStyleDeclaration.getPropertyPriority() 接受 CSS 样式的属性名作为参数，返回一个字符串，表示有没有设置 important 优先级。有返回 important，没有则返回空。

CSSStyleDeclaration.getPropertyValue() 返回参数的 CSS 属性值

CSSStyleDeclaration.item() 接受一个整数值作为参数，返回该位置的 CSS 属性名

CSSStyleDeclaration.removeProperty() 删除 CSS 规则里面的属性名参数的属性值返回这个属性原来的值

CSSStyleDeclaration.setProperty() 设置新的 CSS 属性。第一个参数表示属性名，第二个参数表示属性值，第三个参数唯一合法值是 important

## CSS 对象

CSS.escape() 转义 CSS 选择器里面的特殊字符

CSS.supports() 返回一个布尔值，表示当前环境是否支持某个 CSS 规则

## window.getComputedStyle()

返回浏览器计算后得到的最终规则。返回一个 CSSStyleDeclaration 实例，包含了指定节点的最终样式信息。

CSSStyleDeclaration 实例是一个活的对象，任何对于样式的修改，会实时反映到这个实例上面。另外这个实例是只读的。

getComputedStyle() 的第二个参数是表示当前元素的伪元素（比如：`:before`、`:after` 等）

CSS 规则的简写形式无效。比如读取 margin 的值不能直接读，只能读 marginLeft、marginTop 等。

getComputedStyle 方法返回的 CSSStyleDeclaration 实例的 cssText 属性无效，返回 undefined。

## CSS 伪元素

节点元素的 style 属性无法读写伪元素的样式，这时候要用的 window.getComputedStyle() 。JavaScript 获取伪元素，可以使用下面的方法：

```js
var test = document.querySelector('#test');

var result = window.getComputedStyle(test, ':before').content;
var color = window.getComputedStyle(test, ':before').color;
```

此外，也可以使用 CSSStyleDeclaration 实例的 getPropertyValue 方法，获取伪元素的属性

```js
var result = window.getComputedStyle(test, ':before')
  .getPropertyValue('content');
var color = window.getComputedStyle(test, ':before')
  .getPropertyValue('color');
```

## StyleSheet 接口

代表网页的一张样式表，包括 link 元素加载的样式表和 style 元素内勤的样式表。document 对象的 styleSheets 属性返回当前页面的所有 StyleSheet 实例，是一个伪数组。

如果 style 元素嵌入的样式表，还可以通过节点元素的 sheet 属性获取 StyleSheet 实例。

```js
// HTML 代码为 <style id="myStyle"></style>
var myStyleSheet = document.getElementById('myStyle').sheet;
myStyleSheet instanceof StyleSheet // true
```

StyleSheet.disabled 是否禁用该样式表。可以通过 JavaScript 里面设置 disabled 为 true，等同于 link 里面将这张表设为 alternate stylesheet，即该样式不会生效。

StyleSheet.href 返回样式表的网址

StyleSheet.media  返回一个伪数组对象 MediaList 实例，成员是表示适用媒介的字符串：screen、print、handheld、all

StyleSheet.title

StyleSheet.type

StyleSheet.parentStyleSheet CSS 的 @import 加载其他样式表

StyleSheet.ownerNode 返回 StyleSheet 对象所在 DOM 节点，通常是 link 或者 style 节点

CSSStyleSheet.cssRules 属性指向一个类似数组的对象（CSSRuleList 实例），里面每一个成员就是当前样式表的一条 CSS 规则。使用该规则的 cssText 属性，可以得到 CSS 规则对应的字符串。每条 CSS 规则有个 style 属性指向一个对象，可以i 用来读写具体的 CSS 命令。

CSSStyleSheet.ownerRule 通过 @import 规则引入的 CSS 文件，它的 ownerRule 属性会返回一个 CSSRule 实例，代表那行 @import 规则。

CSSStyleSheet.insertRule() 方法用于在当前样式表插入一个新的 CSS 规则

CSSStyleSheet.deleteRule() 传入要删除的规则在 cssRules 对象中位置删除这条规则

## 添加样式表

JavaScript 添加 link 或者 style 标签，再加入对应的 href 或者 innerHTML 值

## CSSRuleList 接口

CSSRuleList 是一个伪数组，表示一组 CSS 规则，成员都是 CSSRule 实例。获取 CSSRuleList 实例，一般都是通过 StyleSheet.cssRules 属性。

```js
// HTML 代码如下
// <style id="myStyle">
//   h1 { color: red; }
//   p { color: blue; }
// </style>
var myStyleSheet = document.getElementById('myStyle').sheet;
var crl = sheet.cssRules
crl instanceOf CSSRuleList // true
```

## CSSRule

一条 CSS 规则包括两个部分：CSS 选择器和样式声明。

```css
.myClass {
  color: red;
  background-color: yellow;
}
```

JavaScript 通过 CSSRule 接口操作 CSS 规则，一般通过 CSSRuleList 接口。CSSStyleSheet.cssRules 获取 CSSRuleList 实例。

CSSRule.cssText 返回当前规则的文本

CSSRule.parentStyleSheet 返回当前规则所在 StyleSheet 实例

CSSRule.parentRule 返回包含当前规则的父规则，如果不存在父规则（即当前规则是顶层规则），则返回 null。父规则最常见的情况是，当前规则包含在 @media 规则代码块中。

CSSRule.type 返回一个当前规则的类型的整数：普通样式规则 1 CSSStyleRule 实例，@import 规则 3，@media 规则 4 CSSMediaRule 实例，@font-size 规则 5。

### CSSStyleRule 接口

如果一条普通 CSS 规则的样式（不含特殊的 CSS 命令），那么除了 CSSRule 接口，它还部署了 CSSStyleRule 接口。

CSSStyleRule.selectorText 返回当前规则的选择器，可读写

CSSStyleRule.style 返回一个 CSSStyleDeclaration 实例，选择器后面的大括号里的部分。而 CSSStyleDeclaration 实例的 cssText 属性可以返回样式声明的字符串格式。

### CSSMediaRule接口

如果一条 CSS 规则是 @media 代码块，那么它除了 CSSRule 接口，还部署了 CSSMediaRule 接口。

该接口主要提供了 media 属性和 conditionText 属性。前者返回代表 @media 规则的一个对象 MediaList 实例，后者返回 @media 规则的生效条件。

```js
// HTML 代码如下
// <style id="myStyle">
//   @media screen and (min-width: 900px) {
//     article { display: flex; }
//   }
// </style>
var styleSheet = document.getElementById('myStyle').sheet;
styleSheet.cssRules[0] instanceof CSSMediaRule
// true

styleSheet.cssRules[0].media
//  {
//    0: "screen and (min-width: 900px)",
//    appendMedium: function,
//    deleteMedium: function,
//    item: function,
//    length: 1,
//    mediaText: "screen and (min-width: 900px)"
// }

styleSheet.cssRules[0].conditionText
// "screen and (min-width: 900px)"
```

## window.macthMedia()

用来将 CSS 的 MediaQuery 条件语句，转换成一个 MediaQueryList 实例。就算参数是非法 MediaQuery 条件语句，不报错。

### MediaQueryList 接口

MediaQueryList.media 返回一个字符串，表示对应的 MediaQuery 条件语句

MediaQueryList.matches 表示当前页面是否符合指定的 MediaQuery 条件语句

```js
var result = window.matchMedia("(max-width: 700px)");

if (result.matches){
  var linkElm = document.createElement('link');
  linkElm.setAttribute('rel', 'stylesheet');
  linkElm.setAttribute('type', 'text/css');
  linkElm.setAttribute('href', 'small.css');

  document.head.appendChild(linkElm);
}
```

MediaQueryList.onChange 如果条件适配环境发生变化，会触发 change 事件。该函数的参数是 change 事件对象（MediaQueryListEvent 实例），该对象与 MediaQueryList 实例类似，也有 media 和 matches 属性。

MediaQueryList.addListener() 和 MediaQueryList.removeListener() 用来为 change 事件添加或者撤销监听函数。