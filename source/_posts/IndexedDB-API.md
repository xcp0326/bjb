---
layout: post
title: IndexedDB
date: 2019-06-03 15:49:19
tags:
---


现有的浏览器数据储存方案，Cookie 4kb，且每次请求都会发送回服务器；LocalStorage 在 2.5M 到 10M 之间（各浏览器不同），不能搜索不能建索引。这就是 IndexDB 出现的场景。

IndexDB 更接近 NoSQL 数据库

特点：

键值对储存、同源策略、

异步：IndexDB 操作时不会锁死浏览器，用户依然可以进行其他操作，这与 LocalStorage 形成对比，后者的操作是同步的 🤔。异步设计是为了防止大量数据的读写，拖慢网页的表现。

支持事务：transaction，这意味着一系列操作步骤之中，只要有一步失败，整个事务就都取消，数据库回滚到事务发生之前的状态，不存在只改写一部分数据的情况。

储存空间大：一般不少于 250M，甚至没有设置上限

支持二进制储存：不仅可以储存字符串，还可以储存二进制数据（ArrayBuffer 对象和 Blob 对象）

数据库是一系列相关数据的容器。每个域名都可以新建任意多个数据库。IndexDB 数据库有版本的概念。同一个时刻，只能有一个版本的数据库存在。如果修改数据库结构（新增、索引或主键），只能通过升级数据库版本完成。

每个数据库有若干个对象仓库，它类似于关系数据库的表格。

对象仓库保存的是数据记录。每条记录类似于关系数据库的行，但是只有主键和数据体两部分。主键用来建立默认的索引，必须是不同的，否则会报错。主键可以是数据记录里面的一个属性，也可以指定为一个递增的整数编号。比如 id 就可以作为主键。数据体可以是任意数据类型，不限于对象。

为了加速数据的检索，可以在对象仓库里面，为不同的属性建立索引。

数据记录的读写和删改，都要通过事务完成。事务对象提供 error、abort 和 complete 三个时间，用来监听操作结果。

#### 操作流程

```js
const request = window.indexedDB.open(databaseName, version);
let db;
request.onerror = (event) => console.log('数据库打开发生错误');
request.onsuccess = (event) => {
  db = request.result;
  console.log('成功打开一个数据库');
}
// 如果指定的 version 大于数据库版本号，就会升级
request.onupgradeneeded = (event) => {
  db = event.target.result; // 拿到数据库实例，
}
```

#### 新建数据库

打开数据库时，如果指定的数据库不存在就会自动新建并触发 `upgradeneeded` 函数。

新建对象仓库，设置主键，新建索引。下一层对象的属性可以指定为主键。可以自动生成主键（autoIncrement: true)。

```js
request.onupgradeneeded = (event) => {
  db = event.target.result;
  const objectStore = db.createObjectStore('person', {keyPath: 'id'});
  objectStore.createIndex('name', 'name', {unique: false}); 
  objectStore.createIndex('email', 'email', {unique: true});
  // objectStore.createIndex(索引名称，索引所在的属性，配置对象)
}
```

#### 新增数据

通过事务来完成向对象仓库写入数据记录来新增数据。是一个异步操作。

```js
const add = () => {
  const request = db.transaction(['person'], 'readwrite')
  	.objectStore('person')
  	.add({id: 1, name: 'xx', age: 24, email: 'z@z.com'});
  
  request.onsuccess = (event) => console.log('数据写入成功');
  
  request.onerror = (event) => console.log('数据写入失败');
};
add();
```

#### 读取数据

```js
const read = () => {
  const transaction = db.transaction(['person']);
  const objectStore = transaction.objectStore('person');
  const request = objectStore.get(1); // 1 是主键的值.
  
  request.onerror = (event) => console.log('读取失败');
  
  request.onsuccess = (error) => {
    if (request.result) {
      console.log('Name: ' + request.result.name);
    } else {
      console.log('未获得数据记录');
    }
  }
}
read();
```

#### 遍历数据

使用 IDBCursor 指针对象便利数据对象的所有记录

```js
const readAll = () => {
  const objectStore = db.transaction('person').objectStore('person');
  objectStore.openCursor().onsuccess = (event) => {
 		const cursor = event.target.result;
    if (cursor) {
      console.log('Id: ' + cursor.key);
      cursor.continue();
    } else {
      console.log('没有数据了')
    }
  }
}
readAll();
```

openCursor() 方法是一个异步操作，所以要监听 success 事件。

#### 更新数据

```js
const update = () => {
  const request = db.transaction(['person'], 'readwrite')
  	.objectStore('person')
  	.put({id: 1, name: 'zzz', age: 12, email: '1z@z.com'});
  request.onsuccess = (event) => console.log('更新成功');
  request.onerror = (event) => console.log('更新失败');
}
update();
```

#### 删除对象

```js
function remove() {
  var request = db.transaction(['person'], 'readwrite')
    .objectStore('person')
    .delete(1);

  request.onsuccess = function (event) {
    console.log('数据删除成功');
  };
}

remove();
```

索引是让你可以搜索任意字段，也就是说从任意字段拿到数据记录。

对 name 字段建立索引 `objectStore.createIndex('name', 'name', {unique: false})` 

然后就可以用 name 来搜索对应的数据记录了：

```js
const transaction = db.transaction(['person'], 'readonly');
const store = transaction.objectStore('person');
const index = store.index('name');
const request = index.get('李四');

request.onsuccess = (event) => {
  const result = event.target.result;
  if (result) {
    // 
  } else {
    // 
  }
}
```

#### indexedDB.deleteDatabase()

删除数据库后，当前数据库的其他已经打开的链接都会接收到 versionchange 事件

#### indexedDB.cmp()

比较两个值是否为 indexedDB 的相同的主键 🤔 没明白场景是啥

#### IDBRequest 对象

indexedDB.open() 和 indexedDB.deleteDatabase() 会返回这个对象。数据库的操作都是通过这个对象来完成的。是异步操作。通过 readyState （pending | done）表示状态。

- IDBRequest.readyState
- IDBRequest.result
- IDBRequest.error
- IDBRequest.source
- IDBRequest.transaction
- IDBRequest.onsuccess
- IDBRequest.onerror
- IDBOpenDBRequest.onblocked
- IDBOpenDBRquest.onupgradeneeded

#### IDBDatabase 对象

打开数据成功以后，可以从 IDBOpenDBRequest 对象的 result 属性上面，拿到一个 IDBDatabase 对象，

… 

#### IDBObjectStore 对象

#### IDBTransaction 对象

用来异步操作数据库事务，所有数据库的读写操作都要通过这个对象进行。事务 tranaction 这个词先前在 React router 源码见过。事务… 🤔

#### IDBIndex 对象

代表数据库的索引，通过这个对象可以获取数据库里面的记录。数据记录的主键默认是带有索引，IDBIndex 对象主要用于通过除主键意外的其他键，建立索引获取对象。IDBIndex 是持久性（🤔 有不持久的存储）的键值对存储。只要插入、更新或删除数据记录，引用的对象库中的记录，索引就会自动更新。

#### IDBCursor 对象

用来遍历数据仓库 IDBObjectStore 或索引 IDBIndex 的记录。

#### IDBKeyRange 对象

有一个实例方法 includes(key)，返回一个布尔值，表示某个主键是否包含在当前这个主键组之内。🤔 这大概是它的场景吧？

```js
var keyRangeValue = IDBKeyRange.bound('A', 'K', false, false);
keyRangeValue.includes('F')// true
keyRangeValue.includes('W')// false
```

