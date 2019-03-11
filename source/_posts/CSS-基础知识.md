---
title: CSS 基础知识
tags:
  - CSS
date: 2018-11-13 10:57:16
---


## CSS 是什么

CSS 全称 Cascading Style Sheets 层叠样式表，是一种样式表的语言，用来描述 HTML 或 XML 文档的呈现。CSS 描述了在屏幕、纸质、音频等媒体上的元素应该如何渲染。

- 94 年由哈肯&middot;唯姆&middot;莱提出并展示，W3C 也正是这一年由李爵士成立。
- 95 年 W3C 组织 CSS 讨论会
- 96 年 12 月哈肯&middot;唯姆&middot;莱与伯特&middot;波斯发布 CSS 规范第 1 版

## 为什么使用 CSS

样式从内容分离到单独的样式文件中可：

- 避免重复
- 更容易维护
- 为不同目的，使用不同的样式而内容相同

!import 是为用户提供的，相对于开发者的样式，浏览器的样式，用户自定义的样式级别最高。

## CSS 是如何工作的？

当浏览器显示文档时，它必须将文档的内容与其样式信息结合。它分为两个阶段：

1. 浏览器先将标 HTML 和 CSS 转化成 DOM，DOM 代表了计算机内存中的相应文档，因为它已经融合了文档内容和相应的样式表。
2. 浏览器显示 DOM 的内容。

![img](https://mdn.mozillademos.org/files/11781/rendering.svg)

## CSS 语法

### CSS 声明

![](https://mdn.mozillademos.org/files/16188/css_syntax_-_declaration.png)

### CSS 声明块

![img](https://mdn.mozillademos.org/files/3667/css%20syntax%20-%20declarations%20block.png)

### CSS 选择器和规则

![img](https://mdn.mozillademos.org/files/3668/css%20syntax%20-%20ruleset.png)

### CSS 语句

- @ 规则 用来传递元数据、条件信息或其他描述性信息。用 @ 开始，半角分号 ; 结束。
  - @charset 和 @import 元数据
  - @media、 @document、@supports 条件信息
    - @media 浏览器的设备匹配表达式条件运用该规则下的内容
    - @supports 浏览器支持被测功能时运用该规则下的内容
    - @document 当前页面匹配一些条件时运用该规则下的内容
  - @font-face描述性信息

## 选择器

### 简单选择器

- 类型选择器 / 元素选择器
- 类选择器
- ID 选择器
- 通用选择器（`*`）

### 属性选择器

- 存在和值属性选择器：
  - [attr]：选择有 attr 属性的元素
  - [attr="val"]：选择属性 attr 的值为 val 的元素
  - [attr~="val"]：选择属性 attr 的值含有单词 val 的元素
- 子串值属性值 / **伪正则选择器**：
  - [attr|="val"]：选择 attr 属性的值是单词 val 或者以子字符串 val- 开头的元素
  - [attr^="val"]：选择 attr 属性的值以子字符串 val 开头（包含子字符串 val）的元素
  - [attr$="val"]：选择 attr 属性的值以子字符串 val 结尾（包含子字符串 val）的元素。
  - [attr*="val"]：选择 attr 属性的值中包含子字符串 val 的元素
- 大小写匹配属性选择器
  - [attr="val" i]

### 伪类

大部分伪类元素都是针对表单元素的，`:focus` 、`:blur`、`:disabled`、`:enabled`、`:in-range`、`:out-of-range`、`:valide`、`:invalide`、`:indeterminate`、`:default`、`:required`、`:optional`、`:checked`

然后是`<a>`标签的，`:link`、`:visited`、`:hover`、`:active`

所有元素的，`:hover`、`:focus`、`:blur`、`:first-child`、`:last-child`、`:nth-child`、`:nth-last-child`、`:only-of-type`、`:only-child`、`:nth-last-of-type`、`:nth-of-type`、`:first-of-type`、`:last-of-type`、`:target`

空元素 `:empty`

非选择器 `:not()`

`<style>`元素的 `:scope`

根元素 `:root`

打印相关：`:left`、`:right`、`:first`、`:last`、

| 伪类                                  | 描述                                                         |
| ------------------------------------- | ------------------------------------------------------------ |
| `:indeterminate`                      | 表示不确定的表单元素。`<progress>` `<input type="radio">` `<input type="checkbox">` |
| `:only-of-type`                       | 任意一个元素，这个元素没有其他相同类型的兄弟元素             |
| `:in-range` 与 `:out-of-range`        | `<input>`元素的当前值是否处于 `min` 和 `max` 限定的范围之内  |
| `:default`                            | 表示一组相关元素中的默认表单元素                             |
| `:empty`                              | 没有子元素的元素，子元素只可以是元素节点或者文本（包括空格） |
| `:left` `:right`                      | 需要和 `@` 规则 `@page`配套使用，对打印文档的左 / 右侧页设置 CSS 样式 |
| `:link` `:visited` `:hover` `:active` | 出于隐私原因 `:visited` 可以设置的样式只有 color，background-color，border-color，border-bottom-color，border-left-color，border-right-color，border-top-color，column-rule-color，outline-color。`:active` 它让页面能在浏览器监测到激活时给出反馈，比如鼠标交互时，它代表的是用户按下按键和松开按揭之间的时间。LVHA |
| `:root`                               | 文档的根元素，优先级比 `html` 选择器高                       |
| `:first`                              | `@page` 打印文档的第一页样式                                 |
| `:scope`                              | CSS 作用域，可以给 `<style>` 定义 scope 来定义新的参考点， Vue ❓ |
| `:target`                             | 代表一个唯一的页面元素，其 ID 与当前 URL 片段匹配  ❕ 很有意思的一个伪类，可以用它来实现无 JavaScript 的弹窗 |
| `:valid` `:invalid`                   | 通过验证 未通过验证的 `<input>`或者 `<form>` 元素            |
| `:only-child`                         | 独生子元素，等效于 `:first-child:last-child` 或者 `:nth-child(1):nth-lastchild(1)`，只是 `:only-child`  优先级更高 |
| :not()                                | 可以将一个或多个以逗号分隔的选择器作为其参数，选择器中不得包含另一个否定选择符或者伪元素 |

### 伪元素

- ::after，::before，::first-line，::first-letter
- ::selection 只有一小部分 CSS 属性可以用：color，background-color，cursor，outline，text-decoration，text-emphasis-color，text-shadow

### 组合器和选择器组

| 名称           | 组合器 |                              |
| -------------- | ------ | ---------------------------- |
| 选择器组       | A, B   |                              |
| 后代选择器     | A B    |                              |
| 子选择器       | A > B  |                              |
| 相邻兄弟选择器 | A + B  | 上一个兄弟是 A 的 B 元素     |
| 通用兄弟选择器 | A ~ B  | 之前有一个兄弟是 A 的 B 元素 |

### CSS 选择器的优先级

- `style` 属性中的声明在千位加 1；
- 每包含一个 ID 选择器就在百位加 1；
- 每包含一个类选择器、属性选择器、或者伪类在十位加 1；
- 每包含一个元素选择器、或者伪元素就在个位中加 1。

通用选择器（`*`），复合选择器（`+`，`>`，`~`，`''`）和否定伪类选择器（`:not`）不符合上面的规则。

## 继承

- inherit 继承自父元素
- initial 没有浏览器默认值的自然继承属性值与 inherit 一样
- unset 重置为自然值，如果自然继承属性值为 inherit，否则为 initail
- revert 当前节点没有任何样式，设置为用户设置的样式

有意思的一点：没设置`border-color` 的时候，`border-color`继承的是 `color` 的值

## CSS 的值和单位

0 是无单位的，比如 `margin: 0` ；无单位的行高 `line-height: 1.2`； 动画执行的次数 `animation-iteration-count: 5` ；透明度 `opacity: .5` ；

常见单位：

- 绝对单位：px
- 相对单位：
  - em 相对父级 M 的宽度，
  - rem 相对于 `html` M 的宽度，
  - 百分比
  - vw，vh 视窗宽度的 1 / 100，视窗高度的 1 / 100

十六进制值与颜色值：#ff0000，缩写 #f00，颜色值 red

函数：rgb(r,g,b)，rgba(r,g,b,透明度)，hsl()，hsla()，url()，calc()，rotate()，translate()

## CSS 盒模型

border-box 盒模型

![img](https://mdn.mozillademos.org/files/13649/box-model-alt-small.png)

普通盒模型

![img](https://mdn.mozillademos.org/files/13647/box-model-standard-small.png)

框的高度不遵守百分比的长度；框的高度总是采用框内容的高度，除非制定一个绝对的高度值。