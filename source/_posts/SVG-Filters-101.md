---
title: 译文：SVG Filters 101
tags:
  - SVG
  - blur
  - effect
  - filters
  - SVG Filter
  - Sara Soueidan
category:
  - 译文
date: 2019-01-16 13:44:47
---


> 原文：https://tympanus.net/codrops/2019/01/15/svg-filters-101/
> 作者：[Sara Soueidan](https://tympanus.net/codrops/author/sarasoueidan/)

关于SVG过滤器的系列文章的第一篇文章。本指南将帮助您了解它们是什么，并向您展示如何使用它们来创建自己的视觉效果。

![SVGFilters101_featured](https://codropspz-tympanus.netdna-ssl.com/codrops/wp-content/uploads/2019/01/SVGFilters101_featured.jpg)

CSS目前为我们提供了一种方法，通过滤镜属性和随附的滤镜功能，将颜色效果应用于图像，如饱和度，亮度和对比度，以及其他效果。

我们现在在CSS中有11个滤镜功能，可以实现从模糊到改变颜色对比度和饱和度等各种效果。如果您想了解有关它们的更多信息，我们在CSS参考中有[一个专用条目](https://tympanus.net/codrops/css_reference/filter/)。

虽然功能强大且非常方便，但CSS过滤器也非常有限。我们能够用它们创建的效果通常适用于图像，并且仅限于颜色处理和基本模糊。因此，为了创造更强大的效果，我们可以应用于更广泛的元素，我们需要更广泛的功能。这些功能现在已经可用 - 并且已经在SVG中使用了十多年。在本文中，这是关于SVG过滤器的系列文章中的第一篇，您将了解SVG过滤器函数 - 称为“primitives” - 以及如何使用它们。

CSS过滤器是从SVG导入的。它们是SVG中存在的滤镜效果子集的相当优化的版本，并且已经在SVG规范中存在多年。

SVG中的滤镜效果比CSS中更多，SVG版本功能更强大，其复杂效果远远超过CSS快捷键。例如，目前可以使用CSS blur()过滤器函数来模糊元素。使用此函数应用模糊效果将为其应用的元素创建均匀的高斯模糊。下图显示了在CSS中对图像应用6px模糊的结果：

![Screen Shot 2019-01-02 at 12.46.04](https://codropspz-tympanus.netdna-ssl.com/codrops/wp-content/uploads/2019/01/Screen-Shot-2019-01-02-at-12.46.04.png)

blur()函数创建一个模糊效果，该效果均匀地应用于图像上的两个方向（X和Y）。但是此功能仅仅是SVG中可用的模糊滤镜基元的简化且有限的快捷方式，它允许我们均匀地模糊图像，或沿X轴或Y轴应用单向模糊效果。

![The result of applying a blur along the x and y axes, respectively, using SVG filters.](https://codropspz-tympanus.netdna-ssl.com/codrops/wp-content/uploads/2019/01/Screen-Shot-2019-01-02-at-12.48.12.png)

SVG过滤器可以应用于HTML元素以及SVG元素。可以使用url()过滤器函数将SVG过滤器效果应用于CSS中的HTML元素。例如，如果您的SVG中定义了ID为“myAwesomeEffect”的滤镜效果（我们将在稍后讨论在SVG中定义滤镜效果），您可以将该效果应用于HTML元素或图像，如下所示：

```css
.el {
   filter: url(#myAwesomeEffect);
}
```

最重要的是，正如您将在本系列中看到的那样，**SVG滤镜能够使用几行代码在浏览器中创建Photoshop级效果**。我希望这个系列将有助于揭开和解锁部分SVG过滤器的潜力，并激励您开始在自己的项目中使用它们。

但是你是不是要问对于浏览器支持呢......？

## 浏览器支持

[浏览器对大多数SVG过滤器的支持非常好](https://caniuse.com/#feat=svg-filters%E2%80%9D)。但是，如何应用效果可能会因几个浏览器而异，具体取决于浏览器对SVG过滤器效果中使用的inidvidual过滤器primitives的支持，以及取决于任何可能的浏览器 bugs。当SVG过滤器应用于SVG元素而不是HTML元素时，浏览器支持也可能会有所不同。

我建议您将滤镜效果视为一种增强效果：您几乎总能在完全可用的无滤镜体验之上将效果应用为增强效果。（那些了解我的人会知道我支持逐步增强的方法来尽可能地构建UI。）因此，我们不会太关注本系列中的浏览器支持。

最后，尽管SVG Filter支持通常很好，**但请记住，我们将在本系列后面介绍的一些效果可能被认为是实验性的**。如果有的话，我会提到任何重大问题或错误。

那么，如何在SVG中定义和创建滤镜效果？

##  `<filter>` 标签

就像SVG中的线性渐变，蒙版，图案和其他图形效果一样，过滤器有一个方便命名的专用元素：`<filter>`元素。

`<filter>`元素不会直接呈现；它的唯一途径是可以使用SVG中的filter属性或CSS中的url()函数引用。这些元素（除非明确引用，否则不呈现的元素）通常被定义为SVG中<defs>元素内的模板。但是SVG `<filter>`不需要包含在defs元素中。无论是否将过滤器包装在defs元素中，都不会显示它。

原因是过滤器必须在源图像运用，除非您通过调用该源图像上的过滤器显式定义该源图像，否则它不会有任何要呈现的内容。

定义SVG过滤器并将其应用于SVG中的源图像的非常基本的最小代码示例如下所示：

```xml
<svg width="600" height="450" viewBox="0 0 600 450">
	<filter id="myFilter">
  	<!-- 这里写过滤的效果 -->
  </filter>
  <image xlink:href="..."
         width="100%" height="100%" x="0" y="0"
         filter="url(#myFilter)"></image>
</svg>
```

上面的代码示例中的过滤器此时不执行任何操作，因为它是空的。要创建过滤器效果，您需要在`<filter>`内创建该效果而定义一系列一个或多个过滤器操作。

## 过滤元

因此，在SVG中，每个`<filter>`元素都包含一组过滤器基元作为其子项。**每个filter primitives对一个或多个输入执行单个基本图形操作，从而产生图形结果**。

filter primitives以其执行的任何图形操作方便地命名。例如，将高斯模糊效果应用于源图形的primitives称为feGaussianBlur。所有primitives共享相同的前缀：**fe**，这是“过滤效果”的缩写。 （同样，SVG中的名称可以方便地选择，以类似于元素是什么或做什么。）

以下代码段显示了如果该过滤器将5px高斯模糊应用于图像，简单过滤器的外观：

```xml
<svg width="600" height="450" viewBox="0 0 600 450"></feGaussianBlur>
    <filter id="myFilter">
        <feGaussianBlur stDeviation="5"></feGaussianBlur>
    </filter>
    <image xlink:href="..." 
           width="100%" height="100%" x="0" y="0"
           filter="url(#myFilter)"></image>   
</svg>
```

目前在SVG过滤器[规范中定义了17个过滤器基元](https://www.w3.org/TR/filter-effects-1/#FilterPrimitivesOverview)，它们具有极其强大的图形效果，包括但不限于噪声和纹理生成，光照效果，颜色操作（逐个通道），等等。

filter primitives通过将源图形作为输入并输出另一个来进行工作。并且一个过滤器效果的输出可以用作另一个过滤器效果的输入。这非常重要且非常强大，因为它意味着您拥有几乎无数的滤镜效果组合，因此您可以创建几乎无数的图形效果。

每个过滤器基元可以采用一个或两个输入并仅输出一个结果。过滤器基元的输入在名为in的属性中定义。操作的结果在result属性中定义。如果滤镜效果采用第二个输入，则在in2属性中设置第二个输入。操作的结果可以用作任何其他操作的输入，但是如果未在in属性中指定操作的输入，则先前操作的结果将自动用作输入。如果未指定基元的结果，则其结果将自动用作后面基元的输入。 （当我们开始研究代码示例时，这将变得更加清晰。）

除了使用其他基元的结果作为输入之外，过滤器基元还接受其他类型的输入，其中最重要的是：

- SourceGraphic：应用整个过滤器的元素;例如，图像或文本。
- SourceAlpha：这与SourceGraphic相同，只是此图形仅包含元素的alpha通道。例如，对于JPEG图像，它是一个黑色矩形，与图像本身的大小相同。

您会发现有时您会想要使用源图形作为输入，有时只使用其alpha通道。我们将在本文和以下帖子中介绍的示例将清楚地了解何时使用哪些。

此代码段是一个带有一堆过滤器基元的过滤器的示例，如子项所示。不要担心原语和它们的作用。此时，请注意如何定义和使用某些基元的输入和输出。我会添加了一些注释来注明。

```xml
<svg width="600" height="400" viewBox="0 0 850 650">
	<filter id="filter">
  	<feOffset in="sourceAlpha" dx="20" dy="20"></feOffset>
    
    <!--因为前面的过滤器没有定义结果，所以后面的过滤器没有输入集，自动使用上述原语的结果作为以下过滤器的输入-->
    <feGaussianBlur stdDeviation="10" result="DROP"></feGaussianBlur>
    
    <!--在所有大写字母中设置/定义结果名称是使它们更多的好方法可区分，使整体代码更具可读性-->
    <feFlood flood-color="#000" result="COLOR"></feFlood>
    
    <!--该原语使用前两个原语的输出作为输入并输出新效果-->
    <feComposite ion="DROP" in2="COLOR" operator="in" result="SHADOW1"></feComposite>
    
    <feComponentTransfer in="SHADOW1" result="SHADOW">
    	<feFuncA type="table" tableValues="0 0.5"></feFuncA>
    </feComponentTransfer>
    
    <!--无论如何，您可以使用任意两个结果作为任何基元的输入他们在DOM中的顺序。以下原语是使用两个先前生成的一个很好的例子输出作为输入。-->
    <feMerge>
    	<feMergeNode in="SHADOW"></feMergeNode>
      <feMergeNode in="SourceGraphic"></feMergeNode>
    </feMerge>
  </filter>
  <image xlink:href="..."
         x="0" y="0" width="100%" height="100%" filter="url(#filter)"></image>
</svg>
```

现在，在转到我们的第一个过滤器示例之前，我想简要介绍的最后一个概念是过滤区域的概念。

## 过滤区域

该组过滤操作需要一个区域来操作它们可以应用的区域。例如，您可能有一个包含许多元素的复杂SVG，并且您希望仅将滤镜效果应用于特定区域或该SVG内的一个或一组元素。

在SVG中，元素具有“区域”，其边界由元素的边界框的边界定义。 Bounding Box（也缩写为“**bbox**”）是**元素周围最小的拟合矩形**。因此，例如对于一段文本，最小拟合矩形看起来像下图中的粉红色矩形。

![The smallest fitting rectangle around a piece of text.](https://codropspz-tympanus.netdna-ssl.com/codrops/wp-content/uploads/2019/01/filter-region.png)

请注意，此矩形可能会垂直包含一些空白区域，因为在计算边界框的高度时会考虑文本的行高。

**元素的默认过滤器区域是元素的边界框**。因此，如果要对我们的文本应用滤镜效果，效果将仅限于此矩形，并且将截断超出其边界的任何滤镜结果。虽然合情合理，但这并不是很实用，因为许多滤镜会在边界框的边界之外略微影响像素，默认情况下，这些像素最终会被切断。

例如，如果我们对我们的文本应用模糊效果，您可以看到模糊在文本边界框的左右边缘被截断：

![Image showing how The blur effect applied to the text is cut off on both the right and left side of the textâs bounding box area.](https://codropspz-tympanus.netdna-ssl.com/codrops/wp-content/uploads/2019/01/Screen-Shot-2019-01-05-at-19.30.28.png)

那么我们如何防止这种情况发生呢？答案是：通过扩展过滤区域。我们可以通过修改`<filter>`元素上的x，y，width和height属性来扩展应用滤镜的区域。

根据规范，

> 通常需要在过滤器区域中提供填充空间，因为过滤器效果可能会影响位于给定对象上的紧配合边界框之外的位。出于这些目的，可以为“x”和“y”提供负百分比值，为“宽度”和“高度”提供大于100％的百分比值。

**默认情况下，过滤器的区域在所有四个方向上的边界框的宽度和高度延伸10％**。换句话说，x，y，width和height属性的默认值如下：

```xml
<filter x="-10%" y="-10%" width="120%" height="120%" 
        filterUnits="objectBoundingBox">
    <!-- filter operations here -->
</filter>
```

如果在`<filter>`元素上省略这些属性，则默认情况下将使用这些值。您也可以根据需要覆盖它们以扩展或缩小区域。

要记住的一件事是x，y，width和height属性中使用的单位取决于正在使用的filterUnits值。filterUnits属性定义x，y，width和height属性的坐标系。它需要以下两个值之一：

- objectBoundingBox：这是默认值。当filterUnits是objectBoundingBox时，x，y，width和height属性的值是元素边界框大小的百分比或分数。这也意味着如果您愿意，可以使用分数作为值而不是百分比。
- userSpaceOnUse：当filterUnits设置为userSpaceOnUse时，x，y，width和height属性的坐标是相对于当前正在使用的用户坐标系设置的。也就是说，它相对于SVG中使用的当前坐标系，其使用像素作为单位，并且通常相对于SVG本身的大小，假设viewBox值与初始坐标系的值匹配。（您可以在几年前写的[这篇文章](https://www.sarasoueidan.com/blog/svg-coordinate-systems/)中了解有关SVG中坐标系统的所有知识。）

```xml
<!-- Using objectBoundingBox units -->
<filter id="filter" 
        x="5%" y="5%" width="100%" height="100%">

<!-- Using userSpaceOnUse units -->
<filter id="filter" 
        filterUnits="userSpaceOnUse" 
        x="5px" y="5px" width="500px" height="350px">
```

## 快速提示：使用feFlood可视化当前过滤器区域

如果您需要查看过滤器区域的范围，可以通过使用颜色填充过滤器区域来对其进行可视化。方便地，存在一个名为feFlood的过滤器原语，其唯一目的就是这样做：使用您在flood-color属性中指定的颜色填充当前过滤器区域。

因此，假设我们有一段文本需要可视化其过滤区域，代码看起来很简单：

```xml
<svg width="600px" height="400px" viewBox="0 0 600 400">
    <filter id="flooder" x="0" y="0" width="100%" height="100%">
        <feFlood flood-color="#EB0066" flood-opacity=".9"></feFlood>
    </filter>

    <text dx="100" dy="200" font-size="150" font-weight="bold" filter="url(#flooder)">Effect!</text>
</svg>
```

正如您在上面的代码片段中看到的那样，feFlood原语也接受flood-opacity属性，您可以使用该属性使泛色图层变为半透明。

上面的代码片段以粉红色为过滤区域泛滥。但事情就是这样：当你用颜色淹没这个区域时，你真的会用颜色淹没它，意味着颜色将覆盖过滤器区域中的所有内容，包括您之前创建的任何元素和效果，以及文本本身。毕竟，这就是 flooding 的定义，对吧？

![Before and after flooding the text's filter region with color.](https://codropspz-tympanus.netdna-ssl.com/codrops/wp-content/uploads/2019/01/Group.jpg)

<center>***在使用颜色填充文本的过滤器区域之前和之后。***</center>

为了改变这种情况，我们需要将颜色层移动到“后面”并在顶部显示源文本层。

每当您想要在SVG过滤器中显示多个内容层时，您可以使用<feMerge>过滤器原语。顾名思义，feMerge原语用于将元素或效果的图层合并在一起。

<feMerge>原语没有in属性。要合并图层，在feMerge中使用两个或多个<feMergeNode>，每个<feMergeNode>都有自己的in属性，表示我们要添加的图层。

层（或“节点”）堆叠取决于<feMergeNode>源顺序 - 第一个<feMergeNode>将在第二个“后面”或“下面”呈现。最后一个<feMergeNode>表示最顶层。等等。

因此，在我们的文本示例中，泛色是一个图层，源文本（源图形）是另一个图层，我们希望将文本放在泛色的顶部。因此，我们的代码如下所示：

```xml
<svg width="600px" height="400px" viewBox="0 0 600 400">
	<filter id="flooder">
		<feFlood flood-color="#EB0066" flood-opacity=".9" result="FLOOD"></feFlood>
		
		<feMerge>
				<feMergeNode in="FLOOD" />
				<feMergeNode in="SourceGraphic" />
			</feMerge>
	</filter>
	
	<text dx="100" dy="200" font-size="150" font-weight="bold" filter="url(#flooder)">Effect!</text>
</svg>
```

注意我是如何在result属性中命名feFlood的结果的，这样我就可以在<feMergeNode>中引用该名称作为输入。由于我们希望在泛色颜色之上显示源文本，因此我们使用SourceGraphic引用此文本。[查看结果演示](https://codepen.io/SaraSoueidan/pen/eaf2baffb8eac468fff24904e6f4bbe9)

现在我们已经通过这个演示快速了解了SVG滤镜的世界，让我们创建一个简单的SVG投影。

## 给图像应用阴影

让我先从一个快速免责声明开始：你最好使用CSS drop-shadow()过滤器函数创建一个简单的阴影。SVG过滤方式更加冗长。毕竟，正如我们前面提到的，CSS过滤器功能是方便的快捷方式。但是我想把这个例子作为一个简单的入口点来介绍我们将在后面的文章中介绍的更复杂的滤镜效果。

那么，一个投影效果怎么实现的了？

投影通常是在元素后面或下面的浅灰色层，与元素本身具有相同的形式（或形状）。换句话说，您可以将其视为元素的模糊灰色副本。

在创建SVG过滤器时，我们需要分步思考。实现特定效果需要哪些步骤？对于投影，可以通过模糊元素的黑色副本然后着色该黑色副本（使其变为灰色）来创建元素的模糊灰色副本。然后，新创建的模糊灰色副本位于源元素后面，并在两个方向上稍微偏移。

因此，我们将首先获取元素的黑色副本并使其模糊。可以使用元素的alpha通道创建黑色副本，使用SourceAlpha作为我们过滤器的输入。

feGaussianBlur原语将用于将高斯模糊应用于SourceAlpha图层。您需要的模糊量在stdDeviation（标准偏差的简称）属性中指定。如果为stdDeviation属性提供一个值，则该值将用于将均匀模糊应用于输入。您还可以提供两个数值 - 第一个将用于在水平方向上模糊元素，第二个将用于应用垂直模糊。对于投影，我们需要应用统一模糊，因此我们的代码将从此开始：

```xml
<svg width="600" height="400" viewBox="0 0 850 650">
	<filter id="drop-shadow">
    <!--抓取源图像的blakc副本并将其模糊10-->
  	<feGaussianBlur in="SourceAlpha" stdDeviation="10" result="DROP"></feGaussianBlur>
  </filter>
  <image xlink:href="..."
         x="0" y="0" width="100%" height="100%" filter="url(#drop-shadow)"></image>
</svg>
```

上面的代码片段会产生以下效果，此时只渲染图像的模糊Alpha通道：

![screenshot of the filter effect after applying a drop shadow to the alpha channel of the image](https://codropspz-tympanus.netdna-ssl.com/codrops/wp-content/uploads/2019/01/Screen-Shot-2019-01-07-at-18.19.56.png)

接下来，我们想要更改阴影的颜色并使其变灰。我们将通过将泛光颜色应用于滤镜区域，然后使用我们创建的投影图层合成该泛色图层来实现。

**合成是将图形元素与其背景相结合**。背景是元素背后的内容，是元素与之合成的内容。在我们的过滤器中，泛色是上层，模糊的阴影是它的背景（因为它位于它的后面）。我们将在即将发表的文章中更多地看到feComposite原语，所以如果您不熟悉合成是什么以及它是如何工作的，我在博客上有[一篇非常全面的介绍性文章](https://www.sarasoueidan.com/blog/compositing-and-blending-in-css/)，我建议您查看。

feComposite原语具有operator属性，用于指定我们要使用的复合操作。

通过使用in复合运算符，泛色图层将被“裁剪”，并且只渲染与我们的阴影图层重叠的颜色区域，**两层将在它们相交的地方混合**，这意味着灰色将用于着色我们的黑色阴影。

feComposite原语需要两个输入才能操作，在in和in2属性中指定。第一个输入是我们的颜色层，第二个输入是我们的模糊阴影背景。使用operator属性中指定的复合操作，我们的代码现在如下所示：

```xml
<svg width="600" height="400" viewBox="0 0 850 650">
  <filter id="drop-shadow">
  	<feGaussianBlur in="SourceAlpha" stdDeviation="10" result="DROP"></feGaussianBlur>
    <feFlood flood-color="#bbb" result="COLOR"></feFlood>
    <feComposite in="COLOR" in2="DROP" operator="in" result="SHADOW"></feComposite>
  </filter>
  <image xlink:href="..."
         x="0" y="0" width="100%" height="100%" filter="url(#drop-shadow)"></image>
</svg>
```

注意feGaussianBlur和feFlood原语的结果如何用作feComposite的输入。我们的演示现在看起来像这样：

![the result of colorizing the drop shadow using feFlood and feComposite](https://codropspz-tympanus.netdna-ssl.com/codrops/wp-content/uploads/2019/01/Screen-Shot-2019-01-07-at-20.06.49.png)

在我们将原始图像分层到投影上方之前，我们希望垂直和/或水平偏移后者。你有多少抵消阴影以及在哪个方向完全取决于你。对于这个演示，我假设我们有一个来自屏幕左上角的光源，所以我会将它向下移动几个像素。

要在SVG中偏移图层，我们使用feOffset原语。除了in和result属性之外，此基元还有两个主要属性：dx和dy，它们分别决定了沿x轴和y轴偏移图层的距离。

在投影位移后，我们将使用feMerge将其与源图像合并，类似于我们在上一节中合并文本和泛色的方式 - 一个mergeNode将我们的投影作为输入，另一个mergeNode将使用SourceGraphic作为输入对源图像进行分层。我们的最终代码现在看起来像这样：

```xml
<svg width="600" height="400" viewBox="0 0 850 650">
    <filter id="drop-shadow">
        
        <!-- Get the source alpha and blur it; we'll name the result "DROP"  -->
        <feGaussianBlur in="SourceAlpha" stdDeviation="10" result="DROP"></feGaussianBlur>

        <!-- flood the region with a ligh grey color; we'll name this layer "COLOR" -->
        <feFlood flood-color="#bbb" result="COLOR"></feFlood>

        <!-- Composite the DROP and COLOR layers together to colorize the shadow. The result is named "SHADOW"  -->
        <feComposite in="COLOR" in2="DROP" operator="in" result="SHADOW"></feComposite>

        <!-- Move the SHADOW layer 20 pixels down and to the right. The new layer is now called "DROPSHADOW"  -->
        <feOffset in="SHADOW" dx="20" dy="20" result="DROPSHADOW"></feOffset>

        <!-- Layer the DROPSHADOW and the Source Image, ensuring the image is positioned on top (remember: MergeNode order matters)  -->
        <feMerge>
            <feMergeNode in="DROPSHADOW"></feMergeNode>
            <feMergeNode in="SourceGraphic"></feMergeNode>
        </feMerge>
    </filter>

    <!-- Apply the filter to the source image in the `filter` attribute -->
    <image xlink:href="..." x="0" y="0" width="100%" height="100%" filter="url(#drop-shadow)"></image>
</svg>
```

以下是上述代码的现场演示：

<p data-height="586" data-theme-id="0" data-slug-hash="yGwQJZ" data-default-tab="html,result" data-user="dsphoebe" data-pen-title="Drop Shadow: Tinted shadow with feComposite" style="height: 586px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid black; margin: 1em 0; padding: 1em;" class="codepen"><span>See the Pen <a href="https://codepen.io/dsphoebe/pen/yGwQJZ/">Drop Shadow: Tinted shadow with feComposite</a> by dsphoebe (<a href="https://codepen.io/dsphoebe">@dsphoebe</a>) on <a href="https://codepen.io">CodePen</a>.</span></p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

这就是你如何使用SVG过滤器在SVG中应用滤镜效果。您会发现此效果适用于所有主流浏览器。

## 还有另一种方式......

还有另一种更常见的创建投影的方法。您可以将透明度应用于其上，而不是创建黑色阴影并对其应用颜色以使其更亮，从而使其变得半透明，从而更轻盈。

在之前的演示中，我们学习了如何使用feFlood将颜色应用于投影，这是一种您可能会发现自己需要并经常使用的着色技术。这就是我认为有必要覆盖的原因。学习它也很有用，因为如果你想创建一个阴影，无论出于什么原因，它有一个彩色阴影，例如，而不是黑色或灰色的阴影。

要更改图层的不透明度，可以使用feColorMatrix基元或feComponentTransfer基元。我将在后续文章中更详细地讨论feComponentTransfer原语，因此我现在将使用feColorMatrix减少阴影的不透明度。

feColorMatrix原语值得一篇文章。现在，我强烈推荐阅读[Una Kravet的文章](https://alistapart.com/article/finessing-fecolormatrix)，这是一个非常好的例子。

简而言之，此滤镜将矩阵变换应用于输入图形中每个像素的R（红色），G（绿色），B（蓝色）和A（Alpha）通道，以生成具有一组新颜色和alpha值。换句话说，您使用矩阵操作来操纵对象的颜色。基本颜色矩阵如下所示：

```xml
<filter id="myFilter">
    <feColorMatrix
      type="matrix"
      values="R 0 0 0 0
              0 G 0 0 0
              0 0 B 0 0
              0 0 0 A 0 "/>
    </feColorMatrix>
</filter>
```

我再次建议阅读[Una的文章](https://alistapart.com/article/finessing-fecolormatrix)，以了解有关此语法的更多信息。

由于我们只想降低阴影的不透明度，我们将使用不会改变RGB通道的单位矩阵，但我们将减少该矩阵中alpha通道的值：

```xml
<filter id="filter">

  <!-- Get the source alpha and blur it,  -->
  <feGaussianBlur in="SourceAlpha" stdDeviation="10" result="DROP"></feGaussianBlur>

  <!-- offset the drop shadow  -->
  <feOffset in="SHADOW" dx="20" dy="20" result="DROPSHADOW"></feOffset>

  <!-- make the shadow translucent by reducing the alpha channel value to 0.3  -->
  <feColorMatrix type="matrix" in="DROPSHADOW" result="FINALSHADOW" 
                 values="1 0 0 0 0 
                         0 1 0 0 0 
                         0 0 1 0 0 
                         0 0 0 0.3 0">
  </feColorMatrix>

  <!-- Merge the shadow and the source image  -->
  <feMerge>
    <feMergeNode in="FINALHADOW"></feMergeNode>
    <feMergeNode in="SourceGraphic"></feMergeNode>
  </feMerge>
</filter>
```

[这是我们的最后演示](https://codepen.io/SaraSoueidan/pen/aae04119fab3cdb5861afdc64db3d565)

## 结束语

在本系列中，我将尝试避开过滤操作的技术定义，并坚持简化和友好的定义。通常情况下，你不需要深入了解引擎盖下发生的细节，因此深入了解这些细节只会增加文章的复杂性，可能会使它们不易消化，并且几乎不会带来任何好处。在我看来，了解过滤器的作用以及如何使用过滤器就足以充分利用它所提供的功能。如果您想了解更多细节，我建议您从[参考规范](https://www.w3.org/TR/filter-effects-1/)开始。也就是说，规范可能证明没有什么帮助，所以你可能最终会在这方面进行自己的研究。我将在本系列的最后一篇文章中提供一系列优秀的资源供进一步学习。

现在我们已经介绍了SVG过滤器的基础知识以及如何创建和应用SVG过滤器，我们将在后续文章中使用更多过滤器基元来研究更多的效果示例。敬请关注。