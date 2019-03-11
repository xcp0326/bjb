---
title: CSS 变量
tags:
  - CSS
  - CSS Variables
  - CSS Custom Properties
date: 2018-11-25 17:29:12
---


![alt](https://i.imgur.com/a7sW8y2.gif)

96 年 W3C 发布第一版 CSS，到 97 年 发布第二版，一直到现在的 CSS3。CSS 的核心功能并没有升级，第 1 版的标签转到 CSS 2 的选择器，第 3 版的更多的选择器和属性的支持。书写 CSS 依然是高重复量的工作。

所以后来 2007 年 Hampton Catlin 用 Ruby 开发了第一款 CSS 预处理器 Sass，语法前卫，对新手 CSSer 写 Sass 需要一点时间。

在 2009 年 Alexis Sellier 也是用 Ruby 开发了 LESS，后来改用 JavaScript 重写的，它灵感来自 SASS，有许多 SASS 的变量，函数等，而且它还有和 CSS 非常匹配的语法，所以对新人用来很好上手，Bootstrap 最初就是用的 LESS。不过 SASS 后来又受到 LESS 的影响，把 SASS 的语法修改得匹配 CSS，就是后来的 SCSS。

后来 2010 年，tj 大神用 JavaScript 开发了 Stylus，Stylus 主要活跃在 Node 项目中。对 JavaScript 更友好，还专门有 JavaScript API。

我比较中意 Stylus，主要是语法简洁，不用括号，冒号，分号。

当然后来又出现了 CSS 后处理器，不过我没有用过。

终于 [2012 年 我们迎来了 CSS 变量](https://www.w3.org/TR/2012/WD-css-variables-20120410/)，并相继在 [2013 年 Firefox 29 上支持了 CSS 变量](https://mcc.id.au/blog/2013/12/variables)，[2016 年 Chrome 49 嵌入 CSS 变量](https://developers.google.com/web/updates/2016/02/css-variables-why-should-you-care)，[2017 年Microsoft EDGE 15 嵌入 CSS 变量的支持](https://blogs.windows.com/msedgedev/2017/03/24/css-custom-properties/)。

也是到 2016 年大家才开始关注到 CSS 变量，😄 大概开发中用 Chrome 的人更多吧。

![](https://code.dsphoebe.com/images/css-variables/chrome-css-variables.png)

既然大家都嵌入了 CSS 变量，那我们就毫无顾忌的用起来吧！！

![](https://code.dsphoebe.com/images/css-variables/caniuse.png)

## 什么是 CSS 变量

CSS 变量是指由 CSS 开发者和用户定义的实体，自定义属性来设置变量，然后用 var() 方法来访问这个自定义变量。不过后来经过扩展和重构，改名为 CSS 自定义属性。我们会发现 CSS 自定义属性 这个名字更合适。

## 语法

CSS 变量用 `--*`定义变量，用`var(--*)`使用变量，`*`表示变量名。因为 Sass 用了 `$` 而 Less 用了 `@`，为了不产生冲突，CSS 变量用 `--` 作为前缀。

**首先声明一个变量**

```css
:root {
    --main-bg-color: brown;
}
```

这里 `:root` 是 CSS 结构性伪类，它匹配的是文档树的根元素，对 HTML 来说就是 `<html>`。不过 `:root` 的优先级要比 `html` 选择器的优先级要高，因为伪类选择器的优先级要高于元素选择器优先级。在 JavaScript 中 `:root` 对应 的是 `document.documentElement`。

**然后使用这个变量**

```js
div {
  background-color: var(--main-bg-color);
}
```

效果如下

<p data-height="191" data-slug-hash="EOLqNJ" data-default-tab="css,result" data-user="dsphoebe" data-pen-title="learn CSS variables" class="codepen">See the Pen <a href="https://codepen.io/dsphoebe/pen/EOLqNJ/">learn CSS variables</a> by dsphoebe (<a href="https://codepen.io/dsphoebe">@dsphoebe</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

### 命名规范

- 大小写敏感
- 不能包含`$`、`[`、`^`、`(`、`%`等特殊字符
- 支持数字，字母，下划线，横线
- 支持中文、日文或者韩文

```css
:root {
  --color: red;
  --Color: yellow; /* 和上面的 --color 是两个不同的变量 */
  --$: 12px; /* 无效 */
	--1: blue; /* 是有效的 */ 
  --main_color: green;
  --中国: 'China'; /* 有效的 */
}
```

### var 函数

#### 自定义属性的默认值

var 函数的语法是`var[<自定义属性>[,<自定义属性的默认值>]]` ，意思就是如果自定义属性不存在，就会使用自定义属性默认值来渲染。

```css
:root {
    /*--main-bg-color: brown;*/
}
div {
  background-color: var(--main-bg-color, green);
}
```

这个时候 div 的背景颜色就是绿色的，因为它没有找到 `--main-bg-color`

#### 第二个参数可以多层嵌套

```css
body {
  padding: 0;
  margin: 0;
  text-align: center;
  line-height: 5;
}
:root {
  --bg1: red;
  --bg2: yellow;
/*   --bg3: blue; */
/*  --bg4: dark;  */
}
div {
  height: 100vh;
  background-color: var(--bg4, var(--bg3, var(--bg2))) 
}
```

上面的 div 显示的颜色值是黄色，--bg4 和 --bg3 都不存在，就用的 —bg3 的默认值 --bg2 了。

#### 返回值后面会带一个空格

```css
:root {
  --font-size: 12;
}
div {
  font-size: var(--font-size)px; 
}
```

上面的 div 的 font-size 不会是 12px，因为 var 的返回值后面会带一个空格，div 的 font-size 值为 `12 px`，浏览器是不能识别的，是无效字号。

##### 两种方案绕过返回值后面的空格问题

###### 第一种：定义的 CSS 变量就带了 px

```css
:root {
  --font-size: 12px;
}
div {
  font-size: var(--font-size);
}
```

###### 第二种：使用 calc 函数计算带上 px

```css
:root {
  --font-size: 12;
}
div {
  font-size: calc(var(--font-size) * 1px);
}
```

### 变量组合

变量可以和其他变量组合使用 `--variable-name:var(--another-variable-name)`

```css
:root {
  --color: red;
}
.box {
  --box-color: var(--color);
}
.box .header {
  color: var(--box-color);
}
```

### 其他注意点

- 定义 CSS 变量时，如果值是带单位的，不要加引号，否则 var 函数调用的时候会不能识别。

```css
:root {
	--height: '100px';
  --width: 100px;
}
div {
  height: var(--height); /* 无效 */
  width: var(--width);
}
```

div 的高度不会如期显示为 CSS 变量定义的 100px，但是 width 却是正确的。

- var 函数不能嵌套 url 方法，url 方法要放到 CSS 变量里面。而且需要是绝对地址？

## 作用域

CSS 预处理器的作用域是静态的，语法作用域是限制在调用该变量的当前文件中。而原生 CSS 变量是自定义属性，与普通 CSS 属性一样，作用域是 DOM，是能继承或者覆盖父级或者祖先级元素的同类属性值的。

> 在 CSS 中，一个元素的实际属性是由其自身属性以及其祖先元素的属性层叠得到的，CSS 变量也支持层叠的特性，当一个属性没有在当前元素定义，则会转而使用其祖先元素的属性。在当前元素定义的属性，将会覆盖祖先元素的同名属性。

```css
:root {
    --main-bg-color: brown;
}
div {
  --main-bg-color: red;
  background-color: var(--main-bg-color); 
}
```

<p data-height="249" data-slug-hash="EOLqeY" data-default-tab="css,result" data-user="dsphoebe" data-pen-title="Inherit CSS variables" class="codepen">See the Pen <a href="https://codepen.io/dsphoebe/pen/EOLqeY/">Inherit CSS variables</a> by dsphoebe (<a href="https://codepen.io/dsphoebe">@dsphoebe</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

我们除了在 `:root` 里面定义了一个 `--main-bg-color` 之外，在 `div`里面也定义了一个 `--main-bg-color`，理所当然 当我们在 `div` 里面使用这个这个 CSS 属性的时候 `:root` 下的 `--main-bg-color` 会被 `div` 下定义的 `--main-bg-color` 覆盖的。这里的 `:root` 是 CSS 变量的最大作用域，也就是全局作用域。

## JavaScript 的交互

与 JavaScript 交互是 CSS 变量的另一大优势：

```js
element.style.getPropertyValue('--color');
```

获取全局 CSS 变量，通过 `document.documentElement`

```js
var rootStyle = getComputedStyle(document.documentElement);
var cssVariable = root.getPropertyValue('--color').trim();
root.setProperty('--color', 'green');
root.removeProperty('--color');

console.log(cssVariable) // 'red'
```

修改 CSS 变量的值：

```js
document.documentElement.style.setProperty('--color', 'black')
```

## HTML 的 style 属性中使用

CSS 变量可以直接在 HTML 的 style 属性作为内嵌样式使用

```css
:root {
  --color: red;
}
body {
  color: var(--color);
}
```

```html
<p>红色的了</p>
<p style="--color:yellow;">黄色的啊</p>
```

## 浏览器支持情况 

可以用`@support` 检测 CSS 是否支持 CSS 变量

```css
@supports ((--css: 'variables')) {
  /* 已经支持了 */
}
@supports (not (--css: 'variables')) {
  /* 不支持啊 */
}
```

JavaScript 检测是否支持 CSS 变量

```js
if (window.CSS && window.CSS.supports && window.CSS.supports('--css', 'variables')) {
  alert('已经支持了')
} else {
  alert('不支持啊')
}
```

对于老版本的浏览器没有 `CSS.supports()` 方法的，可以用[这个方法](https://gist.github.com/wesbos/8b9a22adc1f60336a699)

```js
function testCSSVariables() {
  var color = 'rgb(255,198,0)';
  var el = document.createElement('span');
  
  el.style.setProperty('--color', color);
  el.style.setProperty('background', 'var(--color)');
  document.body.appendChild(el);
  
  var styles = getComputedStyle(el);
  var doesSupport = styles.backgroundColor === color;
  document.body.removeChild(el);
  return doesSupport;
}
testCSSVariables();
```

## 动画

CSS 变量实现动画，不会需要过多的 DOM 操作。它结合 CSS calc 函数的加、减、乘、除实现动画。

<p data-height="265" data-slug-hash="mmQNZJ" data-default-tab="css,result" data-user="tutsplus" data-pen-title="CSS Variables Animation" class="codepen">See the Pen <a href="https://codepen.io/tutsplus/pen/mmQNZJ/">CSS Variables Animation</a> by Envato Tuts+ (<a href="https://codepen.io/tutsplus">@tutsplus</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

更帅的，结合 Rxjs，CSS Variables 开发的动画。😭 看来要抓紧时间学下 Rxjs 了。

<p data-height="265" data-slug-hash="rmQXbK" data-default-tab="css,result" data-user="tutsplus" data-pen-title="Sunset/Sunrise Animation with CSS Variables" class="codepen">See the Pen <a href="https://codepen.io/tutsplus/pen/rmQXbK/">Sunset/Sunrise Animation with CSS Variables</a> by Envato Tuts+ (<a href="https://codepen.io/tutsplus">@tutsplus</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## 下一篇：Houdini

## 参考链接

[CSS 变量教程](http://www.ruanyifeng.com/blog/2017/05/css-variables.html)
[小tips:了解CSS/CSS3原生变量var](https://www.zhangxinxu.com/wordpress/2016/11/css-css3-variables-var/)
[Houdini：CSS 领域最令人振奋的革新](https://zhuanlan.zhihu.com/p/20939640)