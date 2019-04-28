---
layout: post
title: Storage 接口
date: 2019-04-28 15:35:58
tags:
---


> <sup>这里是我读[《阮一峰 JavaScript 教程》](https://wangdoc.com/javascript/)做的笔记。</sup>

- Storage.setItem()
- Storage.getItem()
- Storage.removeItem()
- Storage.clear()
- Storage.key() 接受一个从 0 开始的整数作为参数，返回该位置对应的键值。


#### Storage 事件

Storage 接口储存的数据发送变化时，会触发 storage 事件，可以指定这个事件的监听函数。
``` js
window.addEventListener('storage', onStorageChange);
```

onStorageChange 接受一个 event 实例对象作为参数。这个实例对象继承了 StorageEvent 接口，有几个特有的属性，都是只读属性。
-   StorageEvent.key 表示发生变动的键名
-   StorageEvent.newValue 表示新的键值
-   StorageEvent.oldValue 表示旧的键值
-   StorageEvent.storageArea 返回当前域名存储的所有键值对
-   StorageEvent.url 原始触发 storage 事件的网页的网址

不在导致数据变化的当前页面触发，而是在同一个域名的其他窗口触发。
