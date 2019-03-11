---
title: Date 对象
category:
date: 2018-12-04 15:24:10
---

> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

Date 对象是 JavaScript 原生的时间库。它以国际标准时间（UTC） 1970 年 1 月 1 日 00:00:00 作为时间的零点，可以表示的时间范围是前后各 1 亿天（单位为毫秒）

## 普通函数的用法

Date 对象可以作为普通函数直接调用，返回一个代表当前时间的字符串。即使带有参数，Date 作为普通函数返回的依然是当前时间。

```js
Date()
// 'Tue Dec 04 2018 13:33:36 GMT+0800 (China Standard Time)'
Date(2010, 10,10)
// 'Tue Dec 04 2018 13:33:43 GMT+0800 (China Standard Time)'
new Date()
// 2018-12-04T05:38:16.321Z
```

## 构造函数的用法

Date 还可以当作构造函数使用。对它使用 new 命令，会返回一个 Date 对象的实例。如果不参数，实例代表的就是当前时间。

Date 实例有一个独特的地方。其他对象求值的时候，都默认调用 .valueOf 方法，但是 Date 实例求值的时候，默认调用的是 toString 方法。这导致对 Date 实例求值，返回的是一个字符串，代表该实例对应的时间。

```js
// 然而并不是
var today = new Date()

today
// Node 下返回的格式并不是 toString 后的字符串
// 2018-12-04T05:40:27.815Z

today
// 浏览器下是 toString 后的字符串
// Tue Dec 04 2018 13:43:23 GMT+0800 (中国标准时间)

today.toString()
// 浏览器下是 toString 后的字符串
// "Tue Dec 04 2018 13:43:23 GMT+0800 (中国标准时间)"

today == today.toString() // true
today === today.toString() // false 🤔

today.valueOf()
// 1543902027815
```

作为构造函数时，Date 对象可以接受多种格式的参数，返回一个该参数对应的实例。

```js
// 参数为时间零点开始计算的毫秒数
new Date(1378218728000)
// Tue Sep 03 2013 22:32:08 GMT+0800 (CST)

// 参数为日期字符串
new Date('January 6, 2013');
// Sun Jan 06 2013 00:00:00 GMT+0800 (CST)

// 参数为多个整数，
// 代表年、月、日、小时、分钟、秒、毫秒
new Date(2013, 0, 1, 0, 0, 0, 0)
// Tue Jan 01 2013 00:00:00 GMT+0800 (CST)
```

第一：参数可以是负数，代表 1970 年元旦之前的时间

```js
new Date(-1378218728000)
// Fri Apr 30 1926 17:27:52 GMT+0800 (CST)
```



第二：只要能被 Date.parse 方法解析的字符串，都可以被当作参数

```js
new Date('2013-2-15')
new Date('2013/2/15')
new Date('02/15/2013')
new Date('2013-FEB-15')
new Date('FEB, 15, 2013')
new Date('FEB 15, 2013')
new Date('Feberuary, 15, 2013')
new Date('Feberuary 15, 2013')
new Date('15 Feb 2013')
new Date('15, Feberuary, 2013')
// Fri Feb 15 2013 00:00:00 GMT+0800 (CST)
```

第三：参数为年、月、日等多个整数时，年和月不能省略，其他参数可以省略。如果只传入了年参数，Date 会将其解释为毫秒数。

```js
new Date(2013)
// Thu Jan 01 1970 08:00:02 GMT+0800 (CST)
new Date(2013, 0)
// Tue Jan 01 2013 00:00:00 GMT+0800 (CST)
new Date(2013, 0, 1)
// Tue Jan 01 2013 00:00:00 GMT+0800 (CST)
new Date(2013, 0, 1, 0)
// Tue Jan 01 2013 00:00:00 GMT+0800 (CST)
new Date(2013, 0, 1, 0, 0, 0, 0)
// Tue Jan 01 2013 00:00:00 GMT+0800 (CST)
```

最后 各个参数的取值范围

- 年：使用四位数年份，比如 2000。如果写成两位数或个位数，则加上 1900，比如 1 代表 1901 年。如果是负数，则表示公元前
- 月：0 表示 一月，依次类推，11 表示 12 月
- 日：1 到 31
- 小时：0 到 23
- 分钟：0 到 59
- 秒：0 到 59
- 毫秒：0 到 999

注意，月份从 0 开始计算，但是，天数从 1 开始计算。另外，除了日期的默认值为 1，小时、分、秒和毫秒都是 0。

这些参数如果超出正常范围，会被自动折算。比如月为 15，就折算为下一年的 4 月

```js
new Date(2013, 15)
// Tue Apr 01 2014 00:00:00 GMT+0800 (CST)
new Date(2013, 0, 0)
// Mon Dec 31 2012 00:00:00 GMT+0800 (CST)
```

参数可以是负数，表示扣去的时间。

```js
new Date(2013, -1)
// Sat Dec 01 2012 00:00:00 GMT+0800 (CST)
new Date(2013, 0, -1)
// Sun Dec 30 2012 00:00:00 GMT+0800 (CST)
```

## 日期的运算

类型自动转换，Date 实例如果转为数值，则等于对应的毫秒数；如果转为字符串，则等于对应的日期字符串。所以，两个日期实例对象进行减法运算时，返回的是他们间隔的毫秒数；进行加法运算时，返回的是两个字符串连接而成的字符串。

## 静态方法

Date.now 返回当前时间距离时间零点 （1970 年 1 月 1 日 00:00:00 UTC）的毫秒数，相当于 Unix 时间戳乘以 1000。

Date.parse 方法用来解析日期字符串，返回该时间距离零点的毫秒数。日期字符串应该符合 RFC 2822 和 ISO 8061 这两个标准，即 YYYY-MM-DDTHH:mm:ss.sssZ 格式，其中最后的 Z 表示时区。但是，其他格式也可以被解析。如果解析失败返回 NaN

```js
Date.parse('Aug 9, 1995')
Date.parse('January 26, 2011 13:51:50')
Date.parse('Mon, 25 Dec 1995 13:30:00 GMT')
Date.parse('Mon, 25 Dec 1995 13:30:00 +0430')
Date.parse('2011-10-10')
Date.parse('2011-10-10T14:48:00')
Date.parse('xxx') // NaN
```

Date.UTC 方法接受年、月、日等变量作为参数，返回该时间距离时间零点的毫秒数。Date.UTC 方法的参数，会被解析成为 UTC 时间（世界标准时间），Date 构造函数的参数会被解释为当前时区的时间。

```js
new Date(2013,12,12)
// 2014-01-11T16:00:00.000Z
var dateUTC = Date.UTC(2013, 12, 12)
// 1389484800000
new Date(dateUTC)
// 2014-01-12T00:00:00.000Z
```

## 实例方法

Date 的实例方法，有几十个自己的方法，除了 valueOf 和 toString，可以分为以下三类：

- to 类：从 Date 对象返回一个字符串，表示指定的时间
- get 类：获取 Date 对象的日期和时间
- set 类：设置 Date 对象的日期和时间

### Date.prototype.valueOf

valueOf 返回实例对象距离时间零点对应的毫秒数，该方法等同于 getTime 方法

```js
var d = new Date()
d.valueOf() // 1543905441996 
d.getTime() // 1543905441996
```

### to 类方法

Date.prototype.toString 返回一个完整的日期字符串。toString 是默认的调用方法，所以如果直接读取 Date 实例，就相当于调用这个方法。

Date.prototype.toUTCString 返回对应的 UTC 时间，也就是比北京时间晚 8 个小时

Date.prototype.toISOString 返回对应时间的 ISO8601 格式 YYYY-MM-DDTHH:mm:ss.sssZ，而且时间都是 UTC 时区的时间。

Date.prototype.toJSON 返回一个符合 JSON 格式的 ISO 日期字符串，与 toISOString 方法的返回结果完全相同。

Date.prototype.toDateString 返回日期字符串，不含日期、分、秒

Date.prototype.toTimeString 返回时间字符串，不含年、月、日

**本地时间**

有三种方法可以将 Date 实例转为表示本地时间的字符串

- Date.prototype.toLocalString 完整的本地时间
- Date.prototype.toLocalDateString 本地日期
- Date.prototype.toLocalTimeString 本地时间

三种方法都可以传入两个参数，第一个参数指定所用的语言的字符串，第二个参数指定返回字符串的格式

```js
var d = new Date(2013, 0, 1);

// 时间格式
// 下面的设置是，星期和月份为完整文字，年份和日期为数字
d.toLocaleDateString('en-US', {
  weekday: 'long',
  year: 'numeric',
  month: 'long',
  day: 'numeric'
})
// "Tuesday, January 1, 2013"

// 指定时区
d.toLocaleTimeString('en-US', {
  timeZone: 'UTC',
  timeZoneName: 'short'
})
// "4:00:00 PM UTC"

d.toLocaleTimeString('en-US', {
  timeZone: 'Asia/Shanghai',
  timeZoneName: 'long'
})
// "12:00:00 AM China Standard Time"

// 小时周期为12还是24
d.toLocaleTimeString('en-US', {
  hour12: false
})
// "00:00:00"

d.toLocaleTimeString('en-US', {
  hour12: true
})
// "12:00:00 AM"
```

## get 方法

Date 对象提供了一系列的 get 方法，用来获取实例对象的某个方面的值

- getTime 返回实例距离零点的毫秒数，等同于 valueOf
- getDate 返回实例对象对应月的几号 从 1 开始。
- getDay 返回星期几，星期日为 0，星期一为 1
- ~~getYear 返回距离 1900 的年数，比如 2018 年距离 1900 是 118~~ 标准已经废弃，替换为 getFullYear
- getFullYear 返回四位的年份
- getMonth 返回月份，0 表示 1 月，11 表示 12 月
- getHours 返回小时 0 - 23
- getMilliseconds 返回毫秒数 0 - 999
- getMinutes 返回分钟 0 - 59
- getSeconds 返回秒 0 - 59
- getTimezoneOffset 返回当前时间与 UTC 的时间的差异，以分钟表示，返回结果考虑到了夏令时因素

```js
var d = new Date('January 6, 2013');

d.getDate() // 6
d.getMonth() // 0
d.getFullYear() // 2013
d.getTimezoneOffset() // -480
```

最后一行返回 -480，即 UTC 时间减去当前时间，单位为分钟。-480 表示 UTC 比当前时间少了 480 分钟，即当前时区比 UTC 早 8 小时。

计算本年度还剩下多少天

```js
function leftDays() {
  var today = new Date()
  var endYear = new Date(today.getFullYear(), 11, 31, 23, 59, 59, 999)
  var msPerDay = 24 * 60 * 60 * 1000
  return Math.round((endYear.getTime() - today.getTime()) / msPerDay)
}
leftDays() // 27 😱
```

Date 对象还提供了这些方法对应的 UTC 版本，用来返回 UTC 时间。

- getUTCDate
- getUTCFullYear
- getUTCMonth
- getUTCDay
- getUTCHours
- getUTCMinuts
- getUTCSeconds
- getUTCMilliseconds

```js
var d = new Date('January 6, 2013');

d.getDate() // 6
d.getUTCDate() // 5
```

## set 类方法

Date 对象提供了一系列 set 方法，用来设置实例对象的各个方面

- setDate(date) 返回改变后的毫秒时间戳
- ~~setYear~~ 标准已经废弃，替换为 setFullYear
- setFullYear(year[, mon, date]) 设置四位年份
- setHours(hour[, min, sec, ms])
- setMilliseconds(ms)：0 - 999 之间的值，如果超出会直接自动折算到年、月、日、分、秒上
- setMinutes(min[, sec, ms])
- setMonth(month[, date])
- setSeconds(sec[, ms])
- setTime(milliseconds)

没有 setDay 方法，因为星期几时计算出来的，而不是设置的。

set 方法的参数都是会自动折算的，如果超出应该的数，会向下一个年/月/日/时/分/秒顺延，如果参数是负数，表示上个年/月/日/时/分/秒减去对应数。

set 方法除了 setTime 和 setYear，都有对应的 UTC 版本，设置对应 UTC 时区的时间。