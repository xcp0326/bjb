---
title: 理解 CSS 的堆叠上下文和堆叠顺序
tags:
  - CSS
  - 堆叠顺序
  - 堆叠上下文
  - z-index
date: 2018-11-01 16:16:06
---


## 堆叠顺序

从下面这些例子里面可以看出来，各属性及元素的堆叠顺序。

`background` 高于负 `z-index` :

<p data-height="263" data-theme-id="light" data-slug-hash="VEorav" data-default-tab="css,result" data-user="dsphoebe" data-pen-title="The stacking order: background vs negative z-index" class="codepen">See the Pen <a href="https://codepen.io/dsphoebe/pen/VEorav/">The stacking order: background vs negative z-index</a> by dsphoebe (<a href="https://codepen.io/dsphoebe">@dsphoebe</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

`background` 在 `border` 层下面，就像 Photoshop 里面，新建一个文件，默认的第一个图层而且是不可编辑图层就是带背景的图层，想通的可以这样理解为，设计 CSS 的人也是认为背景是最下面的，这大概是个常识吧。

`border` 高于 `background` ：

<p data-height="200" data-theme-id="light" data-slug-hash="YJmrXd" data-default-tab="css,result" data-user="dsphoebe" data-pen-title="The stacking order" class="codepen">See the Pen <a href="https://codepen.io/dsphoebe/pen/YJmrXd/">The stacking order</a> by dsphoebe (<a href="https://codepen.io/dsphoebe">@dsphoebe</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

内联元素高于 `border`：

<p data-height="300" data-theme-id="light" data-slug-hash="xyvXOj" data-default-tab="css,result" data-user="dsphoebe" data-pen-title="The stacking order: border vs inline/text" class="codepen">See the Pen <a href="https://codepen.io/dsphoebe/pen/xyvXOj/">The stacking order: border vs inline/text</a> by dsphoebe (<a href="https://codepen.io/dsphoebe">@dsphoebe</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

浮动元素高于 `border`：

<p data-height="251" data-theme-id="light" data-slug-hash="yRmzQp" data-default-tab="css,result" data-user="dsphoebe" data-pen-title="The stacking order: border vs float" class="codepen">See the Pen <a href="https://codepen.io/dsphoebe/pen/yRmzQp/">The stacking order: border vs float</a> by dsphoebe (<a href="https://codepen.io/dsphoebe">@dsphoebe</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

内嵌元素高于浮动元素：

<p data-height="239" data-theme-id="light" data-slug-hash="xyvXdJ" data-default-tab="css,result" data-user="dsphoebe" data-pen-title="The stacking order: multiple inline VS float" class="codepen">See the Pen <a href="https://codepen.io/dsphoebe/pen/xyvXdJ/">The stacking order: multiple inline VS float</a> by dsphoebe (<a href="https://codepen.io/dsphoebe">@dsphoebe</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

浮动元素高于块级元素：

<p data-height="265" data-theme-id="light" data-slug-hash="gBVGob" data-default-tab="css,result" data-user="dsphoebe" data-pen-title="The stacking order: float vs block" class="codepen">See the Pen <a href="https://codepen.io/dsphoebe/pen/gBVGob/">The stacking order: float vs block</a> by dsphoebe (<a href="https://codepen.io/dsphoebe">@dsphoebe</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

定位元素高于浮动元素。

<p data-height="236" data-theme-id="light" data-slug-hash="PyMJyN" data-default-tab="css,result" data-user="dsphoebe" data-pen-title="The stacking order: float vs position" class="codepen">See the Pen <a href="https://codepen.io/dsphoebe/pen/PyMJyN/">The stacking order: float vs position</a> by dsphoebe (<a href="https://codepen.io/dsphoebe">@dsphoebe</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>



### 总结为：

从高到低从第一层到第八层：

第一层 正 `z-index`
第二层  `z-index` 0 层
第三层 内嵌元素 / 文本
第四层 浮动元素
第五层 块级元素
第六层 `border` 层
第七层 `background` 层
第八层 负 `z-index` 层

如果相同层级的元素，后出现的会盖住先出现的。

## 堆叠上下文

根据元素自身的属性按照优先级顺序占用堆叠上下文的空间。

堆叠的属性及元素：

- HTML
- `z-index` 值 不为 `auto` 的 绝对/相对定位
- `z-index` 值 不为 `auto` 的 `flex` item 元素
- `opacity` 不为 1
- `transform` 不为 `none`
- `mix`-blend-mode 不为 `normal`
- `filter` 不为 `none`
- `perspective` 不为 `none`
- `isolation` 不为 `isolate`
- `position` 为 `fixed`
- `will-change` 指定了 CSS 属性，即使没指定这些属性的值
- `-webkit-overflow-scrolling` 为 `touch`

### z-index

多个堆叠上下文间，`z-index` 父级越大的小子级大于小父级的大子级。

<p data-height="540" data-theme-id="light" data-slug-hash="xyvPrK" data-default-tab="css,result" data-user="dsphoebe" data-pen-title="The stacking context: z-index" class="codepen">See the Pen <a href="https://codepen.io/dsphoebe/pen/xyvPrK/">The stacking context: z-index</a> by dsphoebe (<a href="https://codepen.io/dsphoebe">@dsphoebe</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

son2 的 z-index 值大于 son1，但是 son1 却显示在 son2 上面一层，因为 father1 的 z-index 值大于 father2 的 z-index 值。

## 深入阅读

[深入理解CSS中的层叠上下文和层叠顺序](https://www.zhangxinxu.com/wordpress/2016/01/understand-css-stacking-context-order-z-index/)