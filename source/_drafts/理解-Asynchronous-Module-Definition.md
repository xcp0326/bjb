---
title: 理解 AMD
tags: [AMD, Asynchronous Module Definition]
---

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/2/22/Asynchronous_Module_Definition_overview_vector.svg/1280px-Asynchronous_Module_Definition_overview_vector.svg.png)

AMD API指定了一种定义模块及其依赖关系的机制。这些模块可以异步加载而不必担心加载顺序。

一个模块：

```js
define(id?, dependencies?, factory);
```

只有在加载了所有 dependencies 内的依赖项后才会执行 factory 函数。必须在执行模块 factory 函数之前解析依赖关系，并且解析的值应作为参数传递给 factory 函数，其参数位置对应于依赖关系数组中的索引。

所有模块本质上都是异步的，并以非阻塞方式加载到浏览器中。

模块 ID 就是路径，AMD 加载器回根据模块 ID 去加载对应的模块声明文件

模块是必须声明自己的依赖的，否则 Loader 没有办法把依赖的模块加载回来。这就给了我们通过工具分析模块的可能，我们就能在此之上做更多的工作：分析系统的设计是否合理、自动生成线上构建优化方案等。

依赖方式一：通过 define 的 dependencies 参数声明依赖：dependencies 声明的依赖模块，会在 factory 调用时作为参数传递，顺序一致。

依赖方式二：在 factory 中通过 require 声明依赖：Loader 可以通过正则分析 factory function 的 toString 结果，抽取出依赖的模块，并加载和初始化它们。

如果存在dependencies参数，则模块加载器不应该扫描工厂函数中的依赖项

AMD 的一些开发实践：开发时用依赖方式二，上线前工具打包成依赖方式一

装载时依赖 - 模块在初始化过程就需要用到的依赖模块

运行时依赖 - 模块在初始化过程不需要用到，但在后续的运行过程中需要用到的依赖模块