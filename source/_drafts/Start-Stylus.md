---
title: Why stylus
date: 2017-12-12 14:53:03
tags: [stylus, sass, scss, less]
---
### 基于SASS，优于SASS？
### 可选的冒号，分号，大括号。我喜欢不要分号，大括号。不过冒号还是要吧，不然有点乱。
### Mixins之可重用的function，这个SASS/LESS也都是支持的。
### Transparent minxins 形参
### 变量 可选的变量前缀$
    ```stylus
        #prompt
            position: absolute
            top: 150px
            left: 50%
            width: w = 200px
            margin-left: -(w / 2)
    ```
### 通过@访问当前block下的属性值 @属性
### 自定义可复用的名/值对 超棒啊
    ```
    -pos(type, args)
        i = 0
        position: unquote(type)
        {args[i]}: args[i + 1] is a 'unit' ? args[i += 1] : 0
        {args[i += 1]}: args[i + 1] is a 'unit' ? args[i += 1] : 0

        absolute()
            -pos('absolute', arguments)

        fixed()
            -pos('fixed', arguments)

        #prompt
            absolute: top 150px left 5px
            width: 200px
            margin-left: -(@width / 2)

        #logo
            fixed: top left
    ```
### 循环迭代 比LESS强啊，有没有！
    ```
    table
        for row in 1 2 3 4 5
            tr:nth-child({row})
            height: 10px * row
    ```
### 属性拼接，那值是不是也可以拼接
    ```
    vendors = webkit moz o ms official
    border-radius()
        for vendor in vendors
            if vendor == official
            border-radius: arguments
            else
            -{vendor}-border-radius: arguments
    #content
        border-radius: 5px
    ```
### 操作符：+ ** * !! .. ... is a 'string' == 太帅了
    ```
    body
        foo: 5px + 10
        foo: 2 ** 8
        foo: 5px * 2
        foo: !!''
        foo: foo and bar and baz
        foo: foo or bar or baz
        foo: 1..5
        foo: 1...5
        foo: 'foo' is a 'string'
        foo: (1 2 3) == (1 2 3)
        foo: (1 2 3) == (1 2)
        foo: ((one 1) (two 2)) == ((one 1) (two 2)) 
        foo: ((one 1) (two 2)) == ((one 1) (two)) 
        foo: ((one 1) (two 2))[0]
        foo: 3 in (1 2 3 4)
    ```
### 强制类型转换
    ```
    body
        foo: foo + bar
        foo: 'foo ' + bar
        foo: 'foo ' + 'bar'
        foo: 'foo ' + 5px
        foo: 2s - 500ms
        foo: 5000ms == 5s
        foo: 50deg
    ```
### %符
    ```
    body
        foo: '%s / %s' % (5px 10px)
        foo: 'MS:WeirdStuff(opacity=%s)' % 1
        foo: unquote('MS:WeirdStuff(opacity=1)')
    ```
### 颜色值操作
    ```
    body
        foo: white - 50%
        foo: black + 50%
        foo: #eee - #f00
        foo: #eee - rgba(black,.5)
        foo: #cc0000 + 30deg
    ```
### 有返回值的function
### 无序参数
    ```
    fade-out(color, amount = 50%)
        color - rgba(0,0,0,amount)

    body
        foo: fade-out(#eee)
        foo: fade-out(#eee, 20%)
        foo: fade-out(#eee, amount: 50%)
        foo: fade-out(color: #eee, amount: 50%)
        foo: fade-out(amount: 50%, #eee)
        foo: fade-out(amount: 50%, color: #eee)
    ```
### 内建60个以上的颜色函数：light, alpha, dark, lighten, + 50%...
### &的用法