---
layout: post
title: Navigator 对象、Screen 对象
date: 2019-03-14 13:45:39
tags:
---


> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

Navigator.userAgent 返回浏览器的 User Agent 字符串，表示浏览器的厂商和版本信息。这个属性值没有统一规范，上网设备层出不穷；而且用户可以修改它，所以不要这个属性来来识别浏览器。如果要识别浏览器可以使用“功能识别”方法。不过可以用来识别手机浏览器。

```js
var ua = navigator.userAgent.toLowerCase();
if (/mobi|android|touch|mini/i.test(ua)) {
  // 手机浏览器
}
```

Navigator.plugins 返回一个类似数组的对象，成员是 Plugin 实例对象，表示浏览器安装的插件，比如 Flash、ActiveX 等

```js
var pluginsLength = navigator.plugins.length
for (var i = 0; i < pluginsLength; i++) {
  const {name, filename, description, version} = navigator.plugins[i]
}
```

Navigator.platform 返回用户的操作系统信息

Navigator.onLine 返回浏览器是否断线。如果是 false 可以断定用户离线，但是是 true 可能是能连内网却不能连外网。用户变成在线时会触发 online 事件，变成离线时会触发 offline 事件。

```js
window.addEventListener('offline', (e) => console.log('用户离线了'))
window.addEventListener('online', (e) => console.log('用户现在在线了'))
```

Navigator.language 返回浏览器首选语言。Navigator.languages 返回用户可以接受的语言的数组，Navigator.language 是这个数组的第一个成员。HTTP 请求头信息的 Accept-Languge 字段就来自这个数组。如果这个属性发生变化，就会触发 window 对象上的 languagechange 事件。

Navigator.geolocation 在 HTTPS 协议下时，返回一个 Geolocation 对象，包含用户地理位置信息。Geolocation 对象提供了三个方法：Geolocation.getCurrentPosition() 返回用户的当前位置；Geolocation.watchPosition() 监听用户位置的变化；Geolocaiton.clearWatch() 取消 watchPosition() 方法指定的监听函数。这三个方法要求用户授权。

Navigator.cookieEnabled 返回浏览器的 Cookie 功能是否打开。

Navigator.javaEnabled() 浏览器是否能运行 Java Applet 小程序

Navigator.sendBeacon() 用于向浏览器异步发送数据

Screen.height、Screen.width：浏览器窗口所在的屏幕的高度、宽度

Screen.availHeight、Screen.availWidth：屏幕可用高度、屏幕可用宽度

Screen.pixelDepth：屏幕的色彩位数，24 表示屏幕提供 24 位色彩

Screen.colorDepth：是 Screen.pixelDepth 的别名。colorDepth 表示应用程序的颜色深度，pixelDepth 表示屏幕的颜色深度

Screen.orientation：返回一个表示屏幕放心的对象。该对象 type 属性表示屏幕具体方向，landscape-primary 表示横放，landscape-secondary 表示颠倒的横放，portrait-primary 表示竖放，portrait-secondary 表示颠倒的竖放。