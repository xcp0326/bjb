---
layout: post
title: 表单、FormData 对象
date: 2019-05-27 17:38:07
tags:
---


注意：表单里面的 `<button>` 元素的默认 `type` 是 `submit`。表单元素的 `submit()` 方法和 `reset()` 方法。

表单通过浏览器自动实现键值对的数据然后发送给服务器。如果我们想自定义发送给服务器的键值对数据，然后通过 `XMLHttpRequeset.send()` 方法发送，我们就可以用 `FormData` 对象来完成这项工作了。

```js
var formdata = new FormData(form); // form 可选，如果为空，表示空表单。
```

- formdata.get(key)
- formdata.getAll(key)
- formdata.set(key, value)
- formdata.delete(key)
- formdata.append(key, value)
- formdata.has(key)
- formdaata.keys()
- formdata.values()
- formdata.entries()

### 表单的内置验证

表单的自动校验，如果控件通过验证，CSS 的 `:valide` 伪类会被触发，否则 `:invalide` 会被触发，浏览器也会终止表单提交，并显示一个对应的错误。

表单和表单控件都有一个 `checkValidity()` 方法，触发校验。

表单控件元素的 willValidate 属性表示该控件是否会在提交是进行校验。出于某些原因，控件禁止约束验证，比如 控件设有 disabled 属性。[Constraint Validation: Native Client Side Validation for Web Forms](https://www.html5rocks.com/en/tutorials/forms/constraintvalidation/#toc-willValidate)

```html
<div id="one"></div>
<input type="text" id="two" />
<input type="text" id="three" disabled />
<script>
    document.getElementById('one').willValidate; //undefined
    document.getElementById('two').willValidate; //true
    document.getElementById('three').willValidate; //false
</script>
```

控件元素的 `validationMessage` 属性返回控件在提交时自动校验并且不满足校验条件的提示文本。

控件元素的 `setCustomValidity()` 方法定制校验错误提示文本。

```js
fieldset.setCustomValidity(errormessge); // errormessage 为空时，消除校验失败的状态
```

控件元素的 `validatity`  属性返回的是 ValidityState 对象，包含当前校验的信息：

- ValidityState.badInput：浏览器是否不能将用户输入转换成正确的类型，比如用户在数值框输入字符串
- ValidityState.customError：是否已经调用 setCustomValidity() 方法，将校验信息设置为一个非空字符串
- ValidityState.patternMismatch：用户输入是否不满足模式
- ValidityState.rangeOverflow / ValidityState.rangeUnderflow
- ValidityState.stepMismatch：用户输入的值不符合步长的设置（即不能被步长值整除）
- ValidityState.tooLong / ValidityState.tooShort
- ValidityState.typeMismatch：输入的值不符合类型要求
- ValidityState.valid：用户是否满足所有校验条件
- ValidityState.valueMissing：用户没有输入必填的值

表单元素的 HTML 属性 novalidate 关闭浏览器的自动校验

表单的编码格式 enctype 属性：

- 默认 enctype 是 application/x-www-form-urlencoded 格式
- text/plain 表示以纯文本格式发送
- multipart/form-data 数据以混合的格式发送

`<input type="file" multipule>` multipule 是指一次选择多个文件。

