---
title: 如何在网页中插入 icon
tags:
  - CSS
  - CSS Sprite
  - SVG Sprite
  - SVG icon
  - Symbol
  - SVG
date: 2018-11-05 18:48:38
---


从古至今人类就用图标作为交流工具，通过视觉语言来传达信息。比如 象形文字，皇族图腾等。而现今 Web 和 UX 设计的兴起，在 Web 中几乎没有网站或者 APP 没用使用 icon 了。比如 各类网站的分享到微信的微信 icon，点赞的大拇指 icon 或者 心形 icon，还有各种表情 icon。如何在网页中插入 icon 呢？

## 直接插入

最简单的是用 `<img>` 插入图片 icon 图片，代码如下：

```html
<img src="icon-wechat.png" alt="微信" title="分享到微信" width="25" height="25" aria-hidden="true" /> 分享到微信
```

可能会有这样的一个场景，PM 希望鼠标 hover 的时候，icon-wechat.png 能被体现出来鼠标经过它了，然后设计师就给了一张图 icon-wechat-hover.png，这个时候咱们可以把 icon-wechat-hover.png 放到另一个 `<img>` 里，然后再用 CSS 的伪类`:hover`来实现效果。

```html
<span class="share-2-wechat">
  <img src="icon-wechat.png" alt="微信" title="分享到微信" width="25" height="25" aria-hidden="true" />
  <img src="icon-wechat-hover.png" alt="微信" title="分享到微信" width="25" height="25" aria-hidden="true" />
  分享到微信
</span>
```

```css
.share-2-wechat img:nth-child(2) {
  display: none;
}
.share-2-wechat:hover img:nth-child(2) {
  display: initial;
}
.share-2-wechat:hover img:nth-child(1) {
  display: none;
}
```

[查看DEMO 》](http://js.jirengu.com/caboc)

### 缺点是：

- 对这样一个小小的效果会发送两个 HTTP 请求，[现在 HTTP/2 时代来了也还好了](https://webflow.com/blog/the-pros-and-cons-of-icons-in-web-design)

> 网站为了使请求数减少，通常采用对页面上的图片、脚本进行极简化处理。但是，这一举措十分不方便，也不高效，依然需要诸多HTTP链接来加载页面和页面资源。
>
> HTTP/2引入了**服务器推送**，即服务端向客户端发送比客户端请求更多的数据。这允许服务器直接提供浏览器渲染页面所需资源，而无须浏览器在收到、解析页面后再提起一轮请求，节约了加载时间。

## CSS 背景图

把 icon-wechat.png 作为 .share-2-wechat 的背景，hover 的时候把背景图片换成 icon-wechat-hover.png。

```html
<span class="share-2-wechat">
  分享到微信
</span>
```

```css
.share-2-wechat {
  background: url(icon-wechat.png) 0 center no-repeat;
  padding-left: 30px;
}
.share-2-wehcat:hover {
  background-image: url(icon-wechat-hover.png);
}
```

代码量上看，CSS 背景图的方法要比直接 image 插入的方法代码少了不少，而且代码也更加清晰。但它的缺点和直接 image 插入法一样，会有两个 HTTP 请求。同时因为 hover 的时候图片 icon-wechat-hover.png 其实是还没有下载的，所以这个时候如果网速慢 .share-2-wechat 框框里会出现一个空白期，然后等 icon-wechat-hover.png 加载完了才会显示，即一个闪屏的现象。

## CSS sprite 精灵图

CSS sprite 是把所有的 icon 都合并到一张图片上，然后用 background-position 控制图片的位置以显示对应的 icon。

```css
.share-2-wechat {
  background: url(icons.png) 0 center no-repeat;
  padding-left: 30px;
}
.share-2-wehcat:hover {
  background-position: 0 -25px;
}
```

有许多工具可以实现 CSS sprite 图，以及对应的样式代码。比如：[stitches](http://draeton.github.io/stitches/)。缺点是后期维护修改不方便。如果某个 icon 的位置变了，影响到其他 icon 的位置，那整个 icons.png 相关的 icon 的样式都得修改，工作量不小哇。

## Icon Font 字体图标

如果 icon 的颜色都是单色的时候，可以用这个方法 iconfont

它的大致原理是

1. 把 icon 画到一个字体文件里面，并给每个 icon 设置一个对应的 Unicode。这里的 Unicode 码位必须是 PUA (Private Use Area) 的 E000 - F8FF，Unicode 特意保留的自定义字符的区域，一共有 6400 个码位。网上说：阿里巴巴的 iconfont.cn 没有遵循这个最佳实践，用的是 CJK（Chinese Japanese Korean 中日韩统一表意文字）编码区间（U+3432）。还说，当浏览器加载字体出问题时，会还原成一些奇怪的中文文字，这对读屏软件也非常不友好。好在它的管理后台，可以手动的编辑这个 code point。但是其实他们设置的编码是 E6xx 的码位。

2. CSS 里设置 @fontface 的 src 指定为这个字体文件，定义它的 font-family。

    ``` css
    /* 这里复制的 iconfont.cn 帮生成的字体文件，然后可以直接用的 font-face 代码 */
    @font-face {
      font-family: 'iconfont';
      src: url('//at.alicdn.com/t/font_681540_s4ya5or4w75jyvi.eot');
      src: url('//at.alicdn.com/t/font_681540_s4ya5or4w75jyvi.eot?#iefix') format('embedded-opentype'),
      url('//at.alicdn.com/t/font_681540_s4ya5or4w75jyvi.woff') format('woff'),
      url('//at.alicdn.com/t/font_681540_s4ya5or4w75jyvi.ttf') format('truetype'),
      url('//at.alicdn.com/t/font_681540_s4ya5or4w75jyvi.svg#iconfont') format('svg');
    }
    ```

3. 设置为第 2 步定义的 font-family

    ``` css
    [data-icon]::before {
      font-family: 'iconfont';
    }
    ```

4. content 为图标对应的 Unicode

    ```css
    [data-icon]::before {
      content: attr(data-icon);
    }
    .share-2-wechat:hover {
      color: green;
    }
    ```

    ```html
    <span class="share-2-wechat">
      <i
        aria-hidden="true"
        data-icon="&#xe626;"
      ></i> 分享到微信
    </span>
    ```

[查看 DEMO 》](http://js.jirengu.com/hupaz)

Icon Font 的问题是，字体只能是单色，最多实现渐变色，是不能实现彩色的。而 SVG 是可以实现彩色的 icon 的，可以给它的每个 path 或者其他标签单独加不同的颜色。

## SVG icon

SVG Scalable Vector Graphics 可缩放矢量图形，是一种基于可扩展标记语言 XML，用于描述二维矢量图形的图形格式。也是 W3C 制定的标准之一。

- SVG 可以直接插入到网页，成为 DOM 的一部分，并且可以用 CSS 和 JavaScript 操作
- SVG 也可以写入一个独立文件中，再用 `<img>`、`<object>`、`<embed>`、`<iframe>`等标签插入网页中
- SVG 文件也可以作为 CSS 的 `background`
- SVG 文件还可以被转为 base64 编码，然后作为 Data URI 写入网页

### 彩色的 SVG icon

如下 SVG 代码，可以看出控制 `<path>` 的 fill 颜色就可以产出彩色的 icon 啦：

```html
<svg style="width: 3em; height: 3em;" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg">
  <path d="M910.9 958.5H278.3c-29.9 0-54.2-24.3-54.2-54.2v-787c0-29.9 24.3-54.2 54.2-54.2h491.3c10 0 18.1 8.1 18.1 18.1v135.6H947c10 0 18.1 8.1 18.1 18.1v669.4c0 29.8-24.3 54.2-54.2 54.2zM278.3 99.3c-10 0-18.1 8.1-18.1 18.1v786.9c0 10 8.1 18.1 18.1 18.1h632.6c10 0 18.1-8.1 18.1-18.1V252.9H769.5c-10 0-18.1-8.1-18.1-18.1V99.3H278.3z" fill="#EE7401"></path>
  <path d="M947.4 253H769.9c-10 0-18.1-8.1-18.1-18.1V81.2c0-7.1 4.1-13.5 10.6-16.4 6.4-2.9 14-1.9 19.3 2.8L801.9 85c0.2 0.1 0.3 0.3 0.5 0.4l156.9 135.8c5.7 4.9 7.7 12.9 5.1 20-2.7 7.1-9.4 11.8-17 11.8zM788 216.8h111l-111-96.1v96.1zM664.5 781.5H79.4c-11.4 0-20.7-9.3-20.7-20.7V490.6c0-11.4 9.3-20.7 20.7-20.7h585.2c11.4 0 20.7 9.3 20.7 20.7v270.2c-0.1 11.4-9.4 20.7-20.8 20.7z" fill="#EE7401"></path>
  <path d="M355.4 677.5H288l-14.6 53.4h-42.2L297.8 523h48.9l66.9 207.9H370l-14.6-53.4z m-9-32.3l-6.2-22.8c-6.5-21.6-12.1-45.5-18.3-68h-1.1c-5.3 22.8-11.5 46.4-17.7 68l-6.2 22.8h49.5zM438 522.9h41.6v207.9H438V522.9z" fill="#FFFFFF"></path>
</svg>
```

icon 图就是下图啦：

<svg style="width: 3em; height: 3em;" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg">
  <path d="M910.9 958.5H278.3c-29.9 0-54.2-24.3-54.2-54.2v-787c0-29.9 24.3-54.2 54.2-54.2h491.3c10 0 18.1 8.1 18.1 18.1v135.6H947c10 0 18.1 8.1 18.1 18.1v669.4c0 29.8-24.3 54.2-54.2 54.2zM278.3 99.3c-10 0-18.1 8.1-18.1 18.1v786.9c0 10 8.1 18.1 18.1 18.1h632.6c10 0 18.1-8.1 18.1-18.1V252.9H769.5c-10 0-18.1-8.1-18.1-18.1V99.3H278.3z" fill="#EE7401"></path>
  <path d="M947.4 253H769.9c-10 0-18.1-8.1-18.1-18.1V81.2c0-7.1 4.1-13.5 10.6-16.4 6.4-2.9 14-1.9 19.3 2.8L801.9 85c0.2 0.1 0.3 0.3 0.5 0.4l156.9 135.8c5.7 4.9 7.7 12.9 5.1 20-2.7 7.1-9.4 11.8-17 11.8zM788 216.8h111l-111-96.1v96.1zM664.5 781.5H79.4c-11.4 0-20.7-9.3-20.7-20.7V490.6c0-11.4 9.3-20.7 20.7-20.7h585.2c11.4 0 20.7 9.3 20.7 20.7v270.2c-0.1 11.4-9.4 20.7-20.8 20.7z" fill="#EE7401"></path>
  <path d="M355.4 677.5H288l-14.6 53.4h-42.2L297.8 523h48.9l66.9 207.9H370l-14.6-53.4z m-9-32.3l-6.2-22.8c-6.5-21.6-12.1-45.5-18.3-68h-1.1c-5.3 22.8-11.5 46.4-17.7 68l-6.2 22.8h49.5zM438 522.9h41.6v207.9H438V522.9z" fill="#FFFFFF"></path>
</svg>

### 用 Symbol 实现 SVG Sprite

SVG Sprite 是把所有的 icons 放到 一个SVG 文件里，一个 icon 对应 一个 symbol，用 symbol 的 id 来标识每个 icon，在需要插入 icon 的位置`<use xherf:link="sprites.svg#icon-xx"></use>`插入。

```html
<!-- sprites.svg 里面定义 symbol  -->
<svg>
  <symbol id="icon-ai">
    ...
  </symbol>
  <symbol id="icon-gif">
    ...
  </symbol>
</svg>
<!-- use symbol -->
<svg style="width: 3em; height: 3em;" viewBox="0 0 1024 1024">
  <use xlink:href="sprites.svg#icon-ai"></use>
</svg>
<svg style="width: 3em; height: 3em;" viewBox="0 0 1024 1024">
  <use xlink:href="sprites.svg#icon-gif"></use>
</svg>
```

[查看 DEMO 》](http://demo.dsphoebe.com/svg-sprites/)

## CSS 预编译处理器

如果是用预编译器编写 CSS，个人更倾向于 database64，不管是矢量还是位图。

## CSS 画 icon

还有其他有意思的 [cssicon.space](http://cssicon.space)，作者用 CSS 实现了各种图标。

## 参考链接

[Markup-free icon fonts using unicode-range](http://nimbupani.com/markup-free-icon-fonts-with-unicode-range.html)
[Creating an Icon Webfont](https://glyphsapp.com/tutorials/creating-an-icon-webfont)
[The pros and cons of icons in web design](https://webflow.com/blog/the-pros-and-cons-of-icons-in-web-design)
[HTML for Icon Font Usage](https://css-tricks.com/html-for-icon-font-usage/)
[SVG 图像入门教程](http://www.ruanyifeng.com/blog/2018/08/svg.html)
[Styling SVG <use> Content with CSS](https://tympanus.net/codrops/2015/07/16/styling-svg-use-content-css/)