---
date: 2018-10-18 15:48:20
title: CSS 常见布局总结
tags: 
  - CSS
  - Grid
  - Flex
  - BFC
---

## 布局分类

1. 文档流布局：正常文档流布局，内嵌元素从左到右，块级元素从上到下的流动布局。
2. 浮动布局：将 float 属性 设置为 left / right，使块级元素脱离文档流，浮动起来，实现多列布局。
3. Flex 弹性布局：需要给容器元素以及项目元素设置 CSS，感觉不是一两句话能描述清楚，看这里 [Flex 完全指南](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
4. 定位布局：主要是用 position 的两个属性 absolute / relative 实现，也会导致脱离文档流，在不知道父级确切高度的情况下，不要用定位布局。
5. Grid 网格布局：同 Flex 一样，对容器和项目元素设置来实现各种多列多行布局，看这里 [Grid 完全指南](https://css-tricks.com/snippets/css/complete-guide-grid/)



## 常见布局场景

1. 左右布局
2. 左中右布局
3. 水平居中
4. 垂直居中



## 左右布局

对于内嵌元素，其文档流方向本来就是从左到右的，所以不用任何 CSS 辅助，就是左右布局了。

对于块级元素，其文档流方向是从上到下的，那么怎么实现左右布局了，有下面几个方法：浮动，定位，flex 弹性布局，grid 网格布局。



### 浮动实现左右布局

<p data-height="265" data-theme-id="light" data-slug-hash="NOYbaL" data-default-tab="css,result" data-user="dsphoebe" data-pen-title="float - two columns" class="codepen">See the Pen <a href="https://codepen.io/dsphoebe/pen/NOYbaL/">float - two columns</a> by dsphoebe (<a href="https://codepen.io/dsphoebe">@dsphoebe</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

优点：兼容不支持 flex 和 grid 的老版本浏览器

缺点：需要在父级清除浮动，否则会影响接下来的文档流。



### 定位实现左右布局

<p data-height="265" data-theme-id="light" data-slug-hash="ReMoML" data-default-tab="css,result" data-user="dsphoebe" data-pen-title="position - two columns" class="codepen">See the Pen <a href="https://codepen.io/dsphoebe/pen/ReMoML/">position - two columns</a> by dsphoebe (<a href="https://codepen.io/dsphoebe">@dsphoebe</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

优点：兼容不支持 flex 和 grid 的老版本浏览器

缺点：子级容器 absolute 已经脱离了文档流导致父级的容器的高度为 0，最好知道并设置父级的高度，以防后面的文档流上移。



### Flex 弹性左右布局

<p data-height="265" data-theme-id="light" data-slug-hash="aRYBjN" data-default-tab="css,result" data-user="dsphoebe" data-pen-title="flex - two columns" class="codepen">See the Pen <a href="https://codepen.io/dsphoebe/pen/aRYBjN/">flex - two columns</a> by dsphoebe (<a href="https://codepen.io/dsphoebe">@dsphoebe</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

优点：非常简单，直接通过父级 CSS 设置，子级只需设置宽高，就可以完成一个左右布局

缺点：有低版本浏览器兼容性问题



### Grid 网格布局

<p data-height="265" data-theme-id="light" data-slug-hash="qJoqJR" data-default-tab="css,result" data-user="dsphoebe" data-pen-title="grid - two columns" class="codepen">See the Pen <a href="https://codepen.io/dsphoebe/pen/qJoqJR/">grid - two columns</a> by dsphoebe (<a href="https://codepen.io/dsphoebe">@dsphoebe</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

优点：比 Flex 更加简单，直接通过父级 CSS 设置就可以完成一个左右布局

缺点：有低版本浏览器兼容性问题



## 左中右布局

同上面左右布局，只是列分成了三列。这里实现下 Grid 网格布局的 三列布局，超级简单：

<p data-height="265" data-theme-id="light" data-slug-hash="Lgdbay" data-default-tab="css,result" data-user="dsphoebe" data-pen-title="grid - three columns" class="codepen">See the Pen <a href="https://codepen.io/dsphoebe/pen/Lgdbay/">grid - three columns</a> by dsphoebe (<a href="https://codepen.io/dsphoebe">@dsphoebe</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

跟左右布局的两列相比，只是在 grid-template-columns 里面的属性值 从 50% 50% 修改成了 30% 30% 30%。超级简单，有没有。



## 水平居中

内嵌元素居中，就是在其最亲的块级父辈设置 text-align: center; 就可以了。

块级元素，如果知道确定的宽度百分比或者具体 px的话，定义 margin-left，margin-right 分别为 auto，就可以实现水平居中。

其他方式：

### 定位一 （left 50% margin 负一半宽度）

<p data-height="265" data-theme-id="light" data-slug-hash="KGoaOa" data-default-tab="css,result" data-user="dsphoebe" data-pen-title="Horizontal - left , margin" class="codepen">See the Pen <a href="https://codepen.io/dsphoebe/pen/KGoaOa/">Horizontal - left , margin</a> by dsphoebe (<a href="https://codepen.io/dsphoebe">@dsphoebe</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>



### 定位二（left 50% transform translateX -50%）

<p data-height="265" data-theme-id="light" data-slug-hash="ePMvYe" data-default-tab="css,result" data-user="dsphoebe" data-pen-title="Horizontal - left, transform" class="codepen">See the Pen <a href="https://codepen.io/dsphoebe/pen/ePMvYe/">Horizontal - left, transform</a> by dsphoebe (<a href="https://codepen.io/dsphoebe">@dsphoebe</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>



### Flex （给容器定义 justify-content: center，子级居中)

<p data-height="265" data-theme-id="light" data-slug-hash="qJorbQ" data-default-tab="css,result" data-user="dsphoebe" data-pen-title="Horizontal center - flex" class="codepen">See the Pen <a href="https://codepen.io/dsphoebe/pen/qJorbQ/">Horizontal center - flex</a> by dsphoebe (<a href="https://codepen.io/dsphoebe">@dsphoebe</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>



### Grid （justify-self: center;）

<p data-height="265" data-theme-id="light" data-slug-hash="rqdyLd" data-default-tab="css,result" data-user="dsphoebe" data-pen-title="horizontal center - grid" class="codepen">See the Pen <a href="https://codepen.io/dsphoebe/pen/rqdyLd/">horizontal center - grid</a> by dsphoebe (<a href="https://codepen.io/dsphoebe">@dsphoebe</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>





## 垂直居中

内嵌元素居中，一般情况下如果字体设计师设计的字体正常的话，将 height 和 line-height 设置一致就是垂直居中的。



块级元素的话基本和水平居中的块级方案一样：

### 定位一（top: 50% margin 负一半高度）

<p data-height="265" data-theme-id="light" data-slug-hash="dgmvpL" data-default-tab="css,result" data-user="dsphoebe" data-pen-title="Vertical center - top, margin" class="codepen">See the Pen <a href="https://codepen.io/dsphoebe/pen/dgmvpL/">Vertical center - top, margin</a> by dsphoebe (<a href="https://codepen.io/dsphoebe">@dsphoebe</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>



### 定位二（top 50% transform translateY -50%）

<p data-height="265" data-theme-id="light" data-slug-hash="ReMppw" data-default-tab="css,result" data-user="dsphoebe" data-pen-title="vertical center - top, transform" class="codepen">See the Pen <a href="https://codepen.io/dsphoebe/pen/ReMppw/">vertical center - top, transform</a> by dsphoebe (<a href="https://codepen.io/dsphoebe">@dsphoebe</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>



### Flex （给容器定义 align-items center 实现垂直居中）

<p data-height="265" data-theme-id="light" data-slug-hash="WazpEj" data-default-tab="css,result" data-user="dsphoebe" data-pen-title="vertical center - flex" class="codepen">See the Pen <a href="https://codepen.io/dsphoebe/pen/WazpEj/">vertical center - flex</a> by dsphoebe (<a href="https://codepen.io/dsphoebe">@dsphoebe</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>



### Gird（align-self center）

<p data-height="265" data-theme-id="light" data-slug-hash="KGoWRZ" data-default-tab="css,result" data-user="dsphoebe" data-pen-title="vertical center - grid" class="codepen">See the Pen <a href="https://codepen.io/dsphoebe/pen/KGoWRZ/">vertical center - grid</a> by dsphoebe (<a href="https://codepen.io/dsphoebe">@dsphoebe</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>



## BFC 块级格式上下文 Block Format Context 

BFC 是 形成一个独立区域，此区域的子元素不会受到此区域外部的元素 CSS 的影响。

### 形成 BFC 区域的条件：

- 根元素 （HTML 元素），最大的 BFC，它也不可能存在外部的元素。😄 就跟全局闭包一样。
- float left、float right
- overflow scorll、overflow hidden、overflow auto
- position absolute、position fixed、position sticky 难道不是？
- display inline-block、table-cell（是 th，td 的默认的 display 样式）、table（是 table 的默认的 display 样式）、table-row（是 tr 的默认的 display 样式）、table-row-group（是 tbody 的默认的 display 样式）、table-header-group（是 thead 的默认的 display 样式）、table-footer-group（是 tfoot 的默认的 display 样式）、inline-table、flow-root、flex、inline-flex、grid、inline-grid
- **contain layout、content、strict** 没用过
- column-count 或 column-width 不为 auto，column-count 不为 1
- column-span 为 all

### 常见场景/作用

因为是创建一个独立的不会被外部影响，也不影响内部子元素的区域。所以 两个 div 的 margin 叠加时，给其中一个 div 外部包一个 BFC 可以清除叠加。所以清除浮动对文档流的影响时，也是在浮动元素外部包一个 BFC 就可以清除浮动的影响。