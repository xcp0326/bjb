---
title: 理解虚拟 DOM
---
### 什么是虚拟 DOM

什么是 DOM？

DOM 操作就是使用浏览器提供的一些 API 操作，选中一个 DOM 元素对它进行操作，增删改查。那这个 DOM 浏览器里面能看到的一个元素。

什么是虚拟 DOM？

假设要操作数据报表，几百条数据，要做一个排序的操作，我们只能对数据排序，我们怎么对 DOM 的排序，操作起来很麻烦。后来有了 Vue，React，这个表格里面的就是数据，我们就只需要对数据进行排序，这个 DOM 就排序了。

所谓虚拟 DOM，就是根据真实的 DOM 创建一个数据结构，这个数据结构和 DOM 是一一映射的。如果只是轻微的改变，就是只需要打补丁到虚拟 DOM 里面。

### 如何实现一个虚拟 DOM

```js
class VNode {
  constructor(tag, children, text) {
    this.tag = tag
    this.text = text
    this.children = children
  }
  render() {
    if (this.tag === '#text') {
      return document.createTextNode(this.text)
    }
    let el = document.createElement(this.tag)
    this.children.forEach(vChild => {
      el.appendChild(vChild.render())
    })
    return el
  }
}

const v = (tag, children, text) => {
  if (typeof children === 'string') {
    text = children
    children = []
  }
  return new VNode(tag, children, text)
}

const vNode = v('div', [
  v('p', [
    v('span', [v('#text', 'xiedaimala.com')])
  ]),
  v('span', [
    v('#text', 'jirengu.com')
  ])
])

const root = document.querySelector('#root')
root.append(vNode.render())
```

### DOM diff

```js
const vNode1 = v('div', [
  v('p', [
    v('span', [v('#text', 'xiedaimala.com')])
  ]),
  v('span', [
    v('#text', 'jirengu.com')
  ])
])

root.append(vNode1.render())

const vNode2 = v('div', [
  v('p', [
    v('span', [v('#text', 'xiedaimala.com')])
  ]),
  v('span', [
    v('#text', 'xxxx')
  ])
])

root.append(vNode2.render())
```

vNode1 与 vNode2 比较，DOM diff，然后只修改数据 / 虚拟 DOM 中的某一部分进行打补丁。

```js
const patchElement(parent, newVNode, oldVNode, index = 0) {
  if (!oldVNode) {
    parent.appendChild(newVNode.render())
  } else if (!newVNode) {
    parent.removeChild(parent.childNodes[index])
  } else if (newVNode.tag !== oldVNode.tag || newVNode.text !== oldVNode.text) {
    parent.replaceChild(newVNode.render(), parent.childNodes[index])
  } else {
    for (let i = 0; i < newVNode.children.length || i < oldVNode.children.length; i++) {
      patchElement(parent.childNodes[index], newVNode.children[i], oldVNode.children[i], i)
    }
  }
}
```

