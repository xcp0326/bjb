---
date: 2018-10-15
title: 译文：被神化的和真实的 HTML 可替代标签
tags: 
  - 可替代标签
  - HTML
category:
  - 译文
---

> 原文链接：[Replaced Elements in HTML: Myths and Realities](https://www.sitepoint.com/replaced-elements-html-myths-realities/)
>
> 作者：Adrian Sandu

一些 HTML 元素经常会让前端开发者们头痛，像 iframes，applets，embedded 对象以及表单元素。他们的显示是那么难以控制，而且每个浏览器每个系统显示都不一样。

![Replaced Element Myths](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2017/07/1501638545replaced-myths.png)

以致很多的框架和库都做了替代品，从 jQueryUI 到 Bootstrap 等等。但是你有没有想过为什么这些元素的行为是这样的了？有人会说这就是 W3C 说的「可替代标签」。真的是这么回事吗？



## 什么是可替代标签

[W3C 标准](https://drafts.csswg.org/css2/conform.html)是这么说的：

> 内容超出 CSS 格式模型范围的元素，就像 一个图片，内嵌文档，或者 applet。例如，HTML img 标签的内容由其 src 属性指定的图像替换。可替代标签通常具有内在尺寸：固有宽度，固有高度，和固有比率。例如，位图图像是以绝对单位指定的固定的宽度和固定的高度（显然可以从中确定内在比率）。另一方面，其他文档可能根本就没有内在尺寸，比如 一个空白 HTML 文档。
>
> 如果认为这些尺寸可能会将敏感信息泄漏给第三方，则用户代理可以认为可替代标签没有任何内在尺寸。例如，如果 HTML 文档根据用户的银行余额更改内部大小，则用户代理可能希望表现为该资源没有内在尺寸。
>
> CSS 渲染模型中不考虑可替代标签的内容。

至此，在我们深入标准之前，我们得到一个词：内在尺寸



## 什么是内在尺寸

在 [CSS3 的图像标准](https://www.w3.org/TR/css3-images/#sizing-terms)中这样说的：

> 术语【内在尺寸】是指固有高度，固有宽度和固有纵横比（宽度和高度之间的比率）的集合，对于给定对象，每个可以存在或不存在。这些内在尺寸代表物体本身的优选或自然尺寸；也就是说，它们不是使用对象的上下文的函数。CSS 没有定义如何获取内在尺寸。
>
> 位图是具有所有三个内在唯独的对象的示例。设计用于扩展位图的 SVG 图像可能只具有固有宽高比；也可以仅使用固有宽度或高度创建 SVG 图像。在标准中定义的 CSS 渐变是完全没有内在尺寸的对象的示例。另一个例子是嵌入式文档，例如 HTML 中的 &lt;iframe&gt; 元素。一个对象不能只有两个内在尺寸，因为任何两个维度都是能自动定义出第三个维度。

总结下来：

- 图像具有所有三个尺寸：宽度，高度，宽高比
- SVG 图像可能只有宽高比。由于可以缩放，隐藏宽度和高度不固定
- 使用 iframe 元素插入的空 HTML 页面没有内在尺寸

将对象放置在页面中时，它会向文档传达内在尺寸并适当显示。



## 实际项目中的可替代标签

我们先看下 [HTML 标准中的渲染的相关描述](https://html.spec.whatwg.org/multipage/rendering.html)，可以看出一些 HTML 元素一直是可替代标签，而有其他一些却只有在特定情况下才是可替代标签。

在 [14.4 小节](https://html.spec.whatwg.org/multipage/rendering.html#replaced-elements)给出了我们正确理解每个案例所需的所有信息。

### 嵌入内容

将另一个资源导入文档的任何元素，或者来自插入到文档中的另一个资源的内容。这些外部资源，默认就具有符合定义要求的内在尺寸。

这一类元素主要是：embed，iframe，video。这些元素一直都是可替代标签，因为它们是将外部内容导入到文档中。

特定情况下才是可替代标签：

- applet - 只有作为插件时才会被当作可替代标签
- audio - 仅在“暴露用户界面元素”时才会被作为可替代标签，在满屏幕的时候。
- object - 在表示为图像，插件或者嵌套浏览上下文（类似 iframe）时，将其视为可替代标签
- canvas - 在表示嵌入内容时将其视为可替代标签。也就是说，它包含元素的位图（如果有），或者包含与元素具有相同内在尺寸的透明黑色位图时。

这些上述元素将被视为可替代标签的唯一情况（几乎涵盖了使用这些元素的所有情况）。更多详情查看[HTML 标准中的渲染的相关描述](https://html.spec.whatwg.org/multipage/rendering.html)



### images

img 元素被视为具有内在尺寸的可替代标签。还有 input 的 type 为 image 时也是。

有些稍微复杂的情况：当图片无法在页面中渲染出来的时候，浏览器期望它将呈现的丢失图像必须视为可替代标签，它的内容是元素表示的文本（也可以添加图标）&lt;input type="image"> 将显示为普通按钮。



### 可替代标签的默认大小

这是我们收集的有关内在尺寸的信息变得有用的地方。我们已经确定可替代标签的宽度和高度属性来自它们带入页面的内容的内在尺寸。如果无法计算这些尺寸（例如 iframe 加载空白的 HTML 页面的情况），则浏览器必须回退到[默认规则](https://www.w3.org/TR/CSS21/visudet.html) [以下](https://www.w3.org/TR/CSS21/visudet.html#Computing_widths_and_margins)：

> 否则，如果 width 的计算值为 auto，但不满足上述任何条件，则 width 的使用值将变为 300px。如果 300px 太宽而不适合设备，用户代理应使用比例为 2:1 的最大矩形的宽度，而不是适合设备。

[以下](https://www.w3.org/TR/CSS21/visudet.html#Computing_heights_and_margins)

> 否则，如果 height 的计算值为 auto，但不满足上述任何条件，则 height 的使用值必须设置为最大矩形的高度，该矩形的比例为 2:1，高度不大于 150px，宽度不大于设备宽度。

总结一下：

- 如果对象有明确的宽，高，以及宽高比，则可替代标签的默认大小就是它们
- 如果对象只有宽高比，则在保持上述比例的同时对宽和高使用 auto
- 如果宽，高，宽高比都不可用：
  - 如果视窗大于 300px，则使用宽度 300px，高度 150px
  - 如果视窗小于 300px，则宽度和高度都使用 auto，比例为 2:1



### 最后总结的可替代标签为：

- 嵌入内容：embed，iframe 和 video 一直是可替换元素
- 嵌入内容：在特定情况下是可替代标签 applet，audio，object，canvas
- 图像：img 和 input type="image" 在正确加载或浏览器期望最终呈现时被视为可替代标签



## 其他的 Form 控件

都不是可替代标签。