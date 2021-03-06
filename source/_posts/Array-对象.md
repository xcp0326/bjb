---
title: Array 对象
category:
date: 2018-11-28 14:23:08
---

> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

Array 是 JavaScript 的原生对象，同时也是一个构造函数，可以用它生成新的数组。可以省略前面的 new 命令。

如果参数是一个正整数，返回数组的成员都是空位。虽然读取的时候返回都是 undefined，其实该位置上并没有任何值。虽然可以取到 length 属性，但取不到键值。

Array.prototype.valueOf 方法返回数组本身，而 Array.prototype.toString 方法返回的是数组的字符串形式。

Array.prototype.push 方法在数组末端添加一个或多个元素，并返回数组的长度，它会改变原数组

Array.prototype.pop 方法删除数组最后一个元素，并返回该元素，它会改变原数组，对空数组使用 pop 方法，不报错会返回 undefined。pop 方法可以遍历并清空数组。

Array.prototype.shift 方法删除数组第一个元素，并返回这个元素，它会改变原数组。shift 方法可以遍历并清空一个数组。

Array.prototype.unshift 方法在数组起始位置添加一个或者多个元素，并返回添加新元素后的数组长度，它会改变原数组。

Array.prototype.join 方法以指定参数作为分隔符，将所有数组成员连接成一个字符串返回。如果不提供参数，默认用逗号分隔。数组成员是 undefined、null 或者空位，会转成空字符串。通过 call 方法，join 方法也可以用字符串或类似数组的对象。

Array.prototype.concat 方法合并多个数组，将新数组成员添加到原数组成员的后面，返回返回一个新的数组，原数组不变。
concat 也接受其他类型的值作为参数，添加到目标数组的尾部，比如字符串，对象，二维数组等。
如果数组成员包括对象，concat 方法返回当前数组的一个浅拷贝，也就是新数组拷贝的是对象的引用。

Array.prototype.reverse 反转数组，返回反转后的数组，它会改变原数组。

Array.prototype.slice 提取目标数组的一部分，返回一个新数组，原数组不变。
第一个参数是起始位置，默认为 0。第二个参数是终止位置（不包括该位置的元素本身），默认为数组长度。如果省略第二个参数，则一直返回到原数组的最后一项。
参数可以负数，表示倒数计时的位置。
如果第一个参数大于等于数组长度，或者第二个参数小于第一个参数，则返回空数组。
slice 的一个重要应用，是将类数组转为数组。

Array.prototype.splice 用于删除原数组的一部分成员，可以同时在删除的位置添加新的数组成员，返回值是被删除的元素组成的数组，它会改变原数组。
splice 的第一个参数是删除的起始位置（从 0 开始），第二个参数是被删除的元素的个数。如果后面还有更多的参数，则表示这些就是要被插入数组的新元素。
如果第二个参数设为 0，则不删除元素。
如果提供了一个参数，则等同于将原数组拆分成两个数组。如果这个参数为 0，则会清空这个数组。

Array.prototype.sort 默认按照各个字符（数字会被转换为字符串）的 Unicode 码点顺序原地排序数组，它会改变原数组。
sort 方法传入一个函数作为参数，调用这个函数，按照函数的返回值进行排序。如果返回值小于 0，第一个元素排在前面，如果大于 0，第一个元素排在后面。如果比较的是数字，函数返回升序是 a - b，降序为 b - a。String.prototype.localCompare
使用映射改善排序

Array.prototype.map 方法执行指定的函数（map 第一个参数），函数按照指定的上下文（map 的第二个参数）调用，返回一个执行完后了的新数组。

Array.prototype.filter 方法过滤出来符合参数函数的数组成员组成一个新的数组。

Array.prototype.some，Array.prototype.every 类似断言，返回布尔值，表示判断数组成员是否符号条件。some 只要有一些事符合条件就是 true，every 是所有都要符合条件才能返回 true。

Array.prototype.reduce，Array.prototype.reduceRight 方法依次处理数组的每个成员，最终累计为一个值。
四个参数分别为：
1、累计的变量，默认为数组的第一个成员
2、当前变量，默认为数组的第二个成员，如果累计的变量设置了初始值，当前变量就从数组的第一个成员开始
3、可选参数 - 前位置，从 0 开始
4、可选参数 - 原数组
如果是空数组，累计的变量初始值必须传，否则报错。
这两个方法是遍历数组的方法，所以实际上也还可以做一些遍历相关的操作。比如 找出字符长度最长的数组成员。

Array.prototype.indexOf, Array.prototype.lastIndexOf 找出数组中从指定位置（可选第二个参数，默认为 0）找到给定元素（第一个参数）的位置，如果没有返回 -1。内部是用的严格相等来索引的，NaN 不等于自身，不能用来搜索 NaN。

这些方法中，有 map 返回执行完 callback 后的一个新的数组，filter 返回符合某个条件的成员组成的新数组，sort 返回原地排序好了的数组，slice 切出来的新数组，splice 是执行了删除元素后的元素组成的数组（没有删除只有插入就返回空数组），reverse 反转后的数组，concat 合并后的新数组。因为它们返回的都是数组，可以对它们的返回值再接着进行其他方法操作，可以链式使用。

