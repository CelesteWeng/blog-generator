---
title: jQuery体验
date: 2019-03-09 14:55:41
tags: jQuery
---

## 题目

```
window.jQuery = ???
window.$ = jQuery

var $div = $('div')
$div.addClass('red') // 可将所有 div 的 class 添加一个 red
$div.setText('hi') // 可将所有 div 的 textContent 变为 hi
```

<!-- more -->

## 思路

### 第一次、第二次

1. 传入的是字符串，直接用 DOM api 获取节点。
2. addClass：遍历 nodes，获取每个节点的 className，如果没有要添加的类，则添加。
3. setText：遍历 nodes，直接在每个节点写入 textContent。
4. 一开始使用 for 循环遍历，querySelectorAll 获取到的为类数组对象，改成了 forEach 遍历，addClass、setText 函数内将 nodes 换成 this，感觉这样可以实现更松散的耦合。
   
```
window.jQuery = function (nodeOrSelector) {
  let nodes = document.querySelectorAll(nodeOrSelector)
  nodes.addClass = function (classes) {
    this.forEach(t => {
      if (!t.className.split(' ').includes(classes)) {
        t.classList.add(classes)
      }
    })
  }
  nodes.setText = function (text) {
    this.forEach(t => t.textContent = text)
  }
  return nodes
}
```

### 第三次

1. 上述功能没有达到参数 nodeOrSelector 代表的含义，传入节点是不行的。
2. 增加对 nodeOrSelector 的判断，分 string 和 node

### 第四次

1. 改完发现传入多个节点时是错误的，一个节点的部分原型链是这样的，`HTMLDivElement -> HTMLElement -> Element -> Node`，多个节点的原型对象是 `HTMLCollection` 的实例，判断改成 string、Node 的实例或 HTMLCollection 的实例。
2. `let nodes = {}` -> `let nodes = { length: 0 }` 觉得既然是伪数组，那么一开始就要有 length 属性。
3. 让节点的原型链纯净，只将需要的属性值赋给 nodes。
   
```
window.jQuery = function (nodeOrSelector) {
  let nodes = { length: 0 }
  // 传入字符串
  if (typeof nodeOrSelector === 'string') {
    let temp = document.querySelectorAll(nodeOrSelector)
    nodes.length = temp.length
    for (let i = 0; i < temp.length; i++) {
      nodes[i] = temp[i]
    }
  }
  // 传入1个节点
  else if (nodeOrSelector instanceof Node) {
    nodes = { 0: nodeOrSelector, length: 1 }
  } 
  // 传入多个节点
  else if (nodeOrSelector instanceof HTMLCollection) {
    let temp = nodeOrSelector
    nodes.length = temp.length
    for (let i = 0; i < temp.length; i++) {
      nodes[i] = temp[i]
    }
  }
  ...
}
```

### 第五次

1. 遍历代码重复了，累赘，封装成函数

```
window.jQuery = function (nodeOrSelector) {
  let nodes = { length: 0 }
  // 传入1个节点
  if (nodeOrSelector instanceof Node) {
    nodes = { 0: nodeOrSelector, length: 1 }
  } else {
    function cloneObject(nodes, temp) {
      nodes.length = temp.length
      for (let i = 0; i < temp.length; i++) {
        nodes[i] = temp[i]
      }
    }

    // 传入字符串
    if (typeof nodeOrSelector === 'string') {
      let temp = document.querySelectorAll(nodeOrSelector)
      cloneObject(nodes, temp)
    }
    // 传入多个节点
    else if (nodeOrSelector instanceof HTMLCollection) {
      let temp = nodeOrSelector
      cloneObject(nodes, temp)
    }
  }

  nodes.addClass = function (classes) {
    for (let i = 0; i < this.length; i++) {
      if (!this[i].className.split(' ').includes(classes)) {
        this[i].classList.add(classes)
      }
    }
  }
  nodes.setText = function (text) {
    for (let i = 0; i < this.length; i++) {
      this[i].textContent = text
    }
  }
  return nodes
}
```

**done**.