---
title: Grid 入门
tags:
---

## 简介

Grid 布局 又称 CSS 网格布局，用来将页面分割成数个主要区域，或者用来定义组件内部元素间大小，位置和图层之间的关系。



## 基本用法

Grid 布局的使用，需要两层元素，父级容器元素和子级项目元素。

举个简单的两列例子：

<p data-height="300" data-theme-id="light" data-slug-hash="YJeNMo" data-default-tab="css,result" data-user="dsphoebe" data-pen-title="YJeNMo" class="codepen">See the Pen <a href="https://codepen.io/dsphoebe/pen/YJeNMo/">YJeNMo</a> by dsphoebe (<a href="https://codepen.io/dsphoebe">@dsphoebe</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

把 container 分成 4 份，第一个儿子继承的部分是从 第 1 份 到 第 3 份，红色背景部分，而第二个儿子继承的部分是从第 3 份到第 4 份。两个儿子之间还有 30px 的间距。

上面是简单的两列布局，要实现更复杂的布局，来详细了解下它的其他属性。



## 相关属性

Grid 的相关属性，我们要从两个部分来看，container 和 item 两部分。

### container 部分

- display

  - grid 容器为一个块级的网格容器
  - inline-grid 容器为一个内联级别的网格容器

- grid-template-columns 和 grid-template-rows 基于网格列的维度定义网格线的名称和网格轨道的尺寸大小

  - track-size 是指网格轨道尺寸的大小，它的值可以有以下几种形式：
    - | 属性值                                                       | 属性值描述                                                   |
      | ------------------------------------------------------------ | ------------------------------------------------------------ |
      | none                                                         | 所有列和其大小都有 grid-auto-columns 的隐式属性指定          |
      | length                                                       | 非负值大小 比如 100px                                        |
      | 百分比                                                       | 非负值且相对网格容器的百分比。如果是 inline-grid 容器，容器的尺寸大小会依赖网格轨道的大小，则百分比值为 auto |
      | flex                                                         | 非负值，用单位 fr 来定义网格轨道大小的弹性系数。每个定义了 flex 的网格轨道会按照比例分配剩余的可用空间，当外层用一个 minmax() 表示时，它会自动设置为最小值（即 minmax(auto, flex)） |
      | max-content                                                  | 表示以网络项的最大的内容来占据网格轨道                       |
      | min-content                                                  | 表示以网格项的最小的内容来占据网格轨道                       |
      | minmax(min, max)                                             | 定义一个大小范围的属性，大小等于 min 值，并且小于等于 max 值。如果 max 值小于 min 值，则该值会被视为 min 值。最大值可以设置为网格轨道系数值 flex，但最小值则不行。 |
      | auto                                                         | 如果该网格轨道为最大时，该属性等同于 max-content，为最小时，则等同于 min-content |
      | fit-content                                                  | 相当于公式 min(max-content, max(auto, argument))，类似于 auto 的计算（即 minmax(auto，max-content)），除了网格轨道大小值是确定下来的，否则该值都大于 auto 的最小值 |
      | repeat([positive-integer \| auto-fill \| auto-fit], track-list) | 表示网格轨道的重复部分，以一种更简洁的方式去表示大量而且重复列的表达式 |

  - line-name 网格线的名称

- grid-template-areas

- grid-template

- grid-column-gap

- grid-row-gap

- grid-gap

- justify-items

- align-items

- place-items

- justify-content

- align-content

- place-content

- grid-auto-columns

- grid-auto-rows

- grid-auto-flow

- grid

#### 嵌套网格（子网格）



## Grid、 Flex、 Float



## 外部资源

- [网格布局](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Grid_Layout)

