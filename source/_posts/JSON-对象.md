---
title: JSON 对象
category:
date: 2019-02-27 17:51:24
tags:
---

> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

JSON JavaScript Object Notation 用于数据交换的文本格式。2001 Douglas Crockford 提出，取代繁锁笨重的 XML 格式。

相比 XML 格式，JSON 格式优点：书写简单，一目了然；符合 JavaScript 原生语法，可以由解释引擎直接处理，不用另外添加解析代码（顺便：HTTP 请求头 application/json，前后端通过 JSON 来交互）。JSON 已经被写入标准，主流浏览器都已经支持 JSON 相关接口。

每个 JSON 对象就是一个值，可能是一个数组或对象，也可能是一个原始类型的值。总之，只能是一个值，不能是两个或更多的值。

- 复合类型的值只能是数组或对象，不能是函数、正则表达式、日期对象。[Yahoo 写了个支持函数、正则表达式、日期对象的序列化插件](https://github.com/yahoo/serialize-javascript)
- 原始类型的值只有四种：字符串、数值（必须是十进制，不能使用 NaN，Infinity，-Infinity）、布尔值和 null。不支持 undefined。
- 字符串必须是使用双引号表示，不能使用单引号。
- 对象的键名也必须用双引号包起来
- 数组或对象最后一个成员不能加逗号

### JSON 对象

JSON 对象是 JavaScript 的原生对象，用来处理 JSON 格式数据。它有两个静态方法：JSON.stringify() 和 JSON.parse()

### JSON.stringify()

JSON.stringify 方法用于将一个值转为 JSON 字符串。该字符串符合 JSON 格式，并且可以被 JSON.parse 方法还原。

对于原始类型的字符串，转换结果会带双引号。

```js
JSON.stringify('foo') === "foo" // false
JSON.stringify('foo') === "\"foo\"" // true
```

这是因为将来还原的时候，内层双引号可以让 JavaScript 引擎知道，这是一个字符串，而不是其他类型的值。

```js
JSON.stringify(false) // "false"
JSON.stringify('false') // "\"false\""
```

如果不是内层的双引号，将来还原的时候，引擎就无法知道原始值是布尔值还是字符串。

如果对象的属性是 undefined、函数或者 XML 对象，该属性会被 JSON.stringify 过滤。

```js
var obj = {
  a: undefined,
  b: function() {}
};
JSON.stringify(obj) // "{}"
```

而如果数组的成员是 undefined、函数或者 XML 对象，则这些值会被转成 null

```js
var arr = [undefined, function() {}];
JSON.stringify(arr); // "[null, null]"
```

正则对象会被转成空对象

```js
JSON.stringify(/foo/) // "{}"
```

JSON.stringify 方法会忽略对象的不可遍历的属性

```js
var obj = {}
Object.defineProperties(obj, {
  'foo': {
    value: 1,
    enumerable: true
  },
  'bar': {
    value: 2,
    enumerable: false
  }
});

JSON.stringify(obj) // "{"foo":1}"
```

#### JSON.stringify 的第二个参数

JSON.stringify 方法还可以接受一个数组，作为第二个参数，指定对象的需要转成字符串的属性。

```js
var obj = {
  'prop1': 'value1',
  'prop2': 'value2',
  'prop3': 'value3'
};
var selectedProperties = ['prop1', 'prop2'];
JSON.stringify(obj, selectProperties); 
// "{"prop1":"value1","prop2":"value2"}"
```

第二个参数还可以是一个函数，用来更改 JSON.stringify 的返回值

```js
JSON.stringify({a:1,b:2}, function(key, value) {
  if (typeof value === 'number') {
    value = 2 * value;
  }
  return value;
});
```

这个处理函数是递归处理所有键。

```js
var o = {a: {b: 1}};

function f(key, value) {
  console.log("["+ key +"]:" + JSON.stringify(value));
  return value;
}

JSON.stringify(o, f)
// []:[object Object]
// [a]:[object Object]
// [b]:1
// '{"a":{"b":1}}'
```

如果处理函数返回 undefined 或么有返回值，则该属性会被忽略。

```js
JSON.stringify({a: 'abc', b: 123}, function(key, value) {
  if (typeof (value) === 'string') {
    return undefined;
  }
  return value;
});
// "{"b":123}"
```

#### 第三个参数

第三个参数用于增加返回的 JSON 字符串的可读性。如果是数字，表示每个属性前面添加的空格（最多不超过 10 个）；如果是字符串（不超过 10 个字符串），则该字符串会添加在每行前面。

```js
JSON.stringify({ p1: 1, p2: 2 }, null, 2);
/*
"{
  "p1": 1,
  "p2": 2
}"
*/
```

#### 参数对象的 toJSON 方法

如果参数对象有自定义的 toJSON 方法，那么 JSON.stringify 会使用这个方法的返回值作为参数，而忽略原对象的其他属性。

```js
var user = {
  firstName: '三',
  lastName: '张',

  get fullName(){
    return this.lastName + this.firstName;
  },

  toJSON: function () {
    return {
      name: this.lastName + this.firstName
    };
  }
};

JSON.stringify(user)
// "{"name":"张三"}"
```

Date 对象就有一个自己的 toJSON 方法。

```js
var date = new Date('2015-01-01');
date.toJSON() // "2015-01-01T00:00:00.000Z"
JSON.stringify(date) // ""2015-01-01T00:00:00.000Z""
```

toJSON 方法的一个应用是，将正则对象自动转为字符串。因为 JSON.stringify 默认不能转换正则对象，但是设置了 toJSON 方法以后，就可以转换正则对象了。

```js
var obj = {
  reg: /foo/
}

JSON.stringify(obj) // "{"reg":{}}"

// ！！！！ 但是 JSON.parse 不能返回正则，这个方法还是不行的
RegExp.prototype.toJSON = RegExp.prototype.toString;

JSON.stringify(/foo/) // ""/foo/""
```

### JSON.parse()

如果传入的字符串不是有效的 JSON 格式的字符串，JSON.parse 方法会报错：

```js
JSON.parse("'String'") // Uncaught SyntaxError: Unexpected token ' in JSON at position 0
//    at JSON.parse (<anonymous>)
//    at <anonymous>:1:6
```

这里可以使用 try…catch 代码

```js
try {
  JSON.parse("'String'")
} catch(e) {
  console.log('parsing error')
}
```

JSON.parse 方法可以接受一个处理函数，作为第二个参数，用法与 JSON.stringify 方法类似：

```js
JSON.parse('{"a":1,"b":2}', function(key, value) {
  if (key === 'a') {
    return value + 10;
  }
  return value;
})
// {a: 11, b:2}
```

