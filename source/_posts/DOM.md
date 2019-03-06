---
title: JavaScript 教程 - DOM api 归档
date: 2019-03-06 10:46:00
tags: 
- 心痛
---

知道有这么个功能就行，详情见 Google or <a href="https://wangdoc.com/javascript/dom/index.html" target="_blank">JavaScript 教程 / DOM</a>

<!-- more -->

# 概述

## DOM

DOM 是 JavaScript 操作网页的接口，全称为“文档对象模型”（Document Object Model）。

## 节点

DOM 的最小组成单位叫做节点（node）。

节点的类型有七种。

  - Document：整个文档树的顶层节点
  - DocumentType：doctype标签（比如`<!DOCTYPE html>`）
  - Element：网页的各种HTML标签（比如`<body>`、`<a>`等）
  - Attribute：网页元素的属性（比如class="right"）
  - Text：标签之间或标签包含的文本
  - Comment：注释
  - DocumentFragment：文档的片段
  
浏览器提供一个原生的节点对象Node，七种节点都继承了Node，因此具有一些共同的属性和方法。

## 节点树

DOM 树：节点按所在层级，抽象成树状结构。
根节点：`<html>`。
除根节点，其他节点有三种层关系。

- 父节点关系（parentNode）：直接的那个上级节点
- 子节点关系（childNodes）：直接的下级节点
- 同级节点关系（sibling）：拥有同一个父节点的节点

# Node 接口

  均为 Node.prototype.xxx，不重复写了。

  - 属性
    - **nodeType**：整数，节点类型。(文档-9，元素-1，属性-2，文本-3，文档片断-11，文档类型-10，注释-8），有对应常量(e.g.,Node.DOCUMENT_NODE)。
    - **nodeName**：节点名，元素大写（e.g.,DIV），#xxx 等。
    - **nodeValue**：返回字符串，节点文本值，可读写，text、comment、attr 有值，别的 null。div要读儿子的文本。
    - **textContent**：返回自己和子孙的文本，忽略 HTML 标签。写入时对 HTML 标签转义。有节点该属性为 null。
    - **baseURI**：字符串，网页绝对路径。只读。`<base>` > `window.location`。
    - **ownerDocument**：找出我的祖宗。顶层文档对象，即document对象。`document.ownerDocument === null`。
    - **nextSibling**：我后面的兄弟。包括文本节点和注释节点。空格也是兄弟。可用来遍历。
    - **previousSibling**：我前面的兄弟。之后同上。
    - **parentNode**：找爸爸。有人没爸爸 null。生出来没上户口的 null。`node.parentNode.removeChild(node);`自己不想活了。
    - **parentElement**：只要是 element 的爸爸，document 和 documentfragment 滚蛋。
    - **firstChild**，**lastChild**：第一个儿子（element or text or comment），没有就 null。
    - **childNodes**：类数组对象，把我的儿子们（text 和 comment 弱鸡也是我的儿子）打包。没儿子给个空包。动态集合。
    - **isConnected**：布尔值，看看我上户口了没。
  - 方法
    - **appendChild**()：喜当爹，送你个娃。返回值是这个娃。例外：娃 是 DocumentFragment 时，自己查。
    - **hasChildNodes**()：布尔值，我有没有娃（还有2种方法）。
    - **cloneNode**()：造一个同卵双胞胎。有唯一属性要改、监听\事件\回调 不能克隆。
    - **insertBefore**()：插入父节点内指定元素前。
    - **removeChild**()：把这个儿子赶出家门。(把不是儿子的人赶出家门会报错)。
    - **replaceChild**()：辣鸡，我要换个人做儿子。
    - **contains**()：布尔值，看看我和子孙们上族谱了没。
    - **compareDocumentPosition**()：返回一个六个比特位的二进制值，表示参数节点与当前节点的关系。两人可能关系复杂，返回数值为总和，需与掩码 与运算，具体判断。
    - **isEqualNode**()，**isSameNode**()：布尔值，是否相等。类型相同、属性相同、子节点相同。
    - **normalize**()：去除空文本节点，毗邻的文本节点合并。Text.splitText的逆方法。
    - **getRootNode**()：作用等于 ownerDocument，but，`document.getRootNode() // document`。

# NodeList 接口，HTMLCollection 接口

  节点集合：NodeList(contain the various types node) 和 HTMLCollection（HTML element node only）。

  - NodeList 接口（NodeList.prototype.xxxxx）
    - 概述：
      - NodeList实例是类数组对象，成员为节点对象。
      - 创建NodeList实例：（1）Node.childNodes；（2）document.querySelectorAll()等节点搜索方法。
      - 转为真正的数组：`var nodeArr = Array.prototype.slice.call(document.body.childNodes);`。
      - Node.childNodes 动态集合，别的为静态集合。
    - length：返回节点数。
    - forEach()：用法同数组 forEach()。
    - item()：返回该位置成员。
    - keys()，values()，entries()：类似 Object.xxx。

  - HTMLCollection 接口（HTMLCollection.prototype.xxx）
    - 概述：
      - 只有元素节点，类数组对象，只能 for 循环遍历。
      - HTMLCollection 实例都为动态集合。
    - length：成员数量。
    - item()：返回该位置成员。
    - namedItem()：返回 id 或 name 为指定字符串的元素节点，没有就 null。

mdwoyaokantule

# ParentNode 接口，ChildNode 接口

  - ParentNode 接口（ParentNode.xxx）
    - children：all child element nodes.
    - firstElementChild： nothing special.
    - lastElementChild：nothing special..
    - childElementCount：nothing special...
    - append()，prepend()：添加 元素 or 文字子节点。往后加 / 往前加。
  - ChildNode 接口（有爸爸就能用）
    - ChildNode.remove()：让自己从世界消失。
    - ChildNode.before()，ChildNode.after()：insert sibling before or after myself.
    - ChildNode.replaceWith()：替换节点。

# Document 节点

  - 概述：document 节点 -> 整个文档。document 对象有不同办法获取，详情见Google。
  - 属性（document.xxx）
    - 快捷方式属性：
      - defaultView：返回 document 对象所属的 window 对象，无则 null。
      - doctype：指向`<DOCTYPE>`节点。
      - documentElement：返回当前文档的 根元素节点，一般为 `<html>` 节点。
      - body，head：指向相应节点，代码里不写，浏览器给你加。可写。
      - scrollingElement：当前文档的滚动元素。
      - activeElement：获得焦点的 DOM 元素。e.g.,`<input>`、`<textarea>`、`<select>`等表单元素。无返回  `<body>` 或 `null`。
      - fullscreenElement：全屏展示的 DOM 元素。比如查看 `<video>` 元素是否为全屏状态。

    - 节点集合属性：
      - links：所有设定了 href 属性的 `<a>` 及 `<area>` 节点。
      - forms：所有 `<form>` 节点。可用 id 和 name 来引用。
      - images：所有图片节点。可以用 imgList[i].src === 'xxx' 来查找某张图。
      - embeds，plugins：所有 `<embed>` 节点。
      - scripts：所有。。。
      - styleSheets：文档内嵌或引入的样式表集合。
      - 小结：除了document.styleSheets，别的都返回 HTMLCollection 实例。

我挂了，这篇博客先太监吧，IDE 你要学会自己写代码呀

    - 文档静态信息属性：
    - 文档状态属性：
    - cookie：
    - designMode：
    - implementation：
  - 方法（document.xxx）
    - open()，close()：
    - write()，writeln()：
    - querySelector()，querySelectorAll()：
    - getElementsByTagName()：
    - getElementsByClassName()：
    - getElementsByName()：
    - getElementById()：
    - elementFromPoint()，elementsFromPoint()：
    - caretPositionFromPoint()：
    - createElement()：
    - createTextNode()：
    - createAttribute()：
    - createComment()：
    - createDocumentFragment()：
    - createEvent()：
    - addEventListener()，removeEventListener()，dispatchEvent()：
    - hasFocus()：
    - adoptNode()，importNode()：
    - createNodeIterator()：
    - createTreeWalker()：
    - execCommand()，queryCommandSupported()，queryCommandEnabled()：
    - getSelection()：

# Element 节点

- 实例属性
元素特性的相关属性
元素状态的相关属性
Element.attributes
Element.className，Element.classList
Element.dataset
Element.innerHTML
Element.outerHTML
Element.clientHeight，Element.clientWidth
Element.clientLeft，Element.clientTop
Element.scrollHeight，Element.scrollWidth
Element.scrollLeft，Element.scrollTop
Element.offsetParent
Element.offsetHeight，Element.offsetWidth
Element.offsetLeft，Element.offsetTop
Element.style
Element.children，Element.childElementCount
Element.firstElementChild，Element.lastElementChild
Element.nextElementSibling，Element.previousElementSibling
- 实例方法
属性相关方法
Element.querySelector()
Element.querySelectorAll()
Element.getElementsByClassName()
Element.getElementsByTagName()
Element.closest()
Element.matches()
事件相关方法
Element.scrollIntoView()
Element.getBoundingClientRect()
Element.getClientRects()
Element.insertAdjacentElement()
Element.insertAdjacentHTML()，Element.insertAdjacentText()
Element.remove()
Element.focus()，Element.blur()
Element.click()
- 参考链接

# 属性的操作

- Element.attributes 属性
- 元素的标准属性
- 属性操作的标准方法
概述
Element.getAttribute()
Element.getAttributeNames()
Element.setAttribute()
Element.hasAttribute()
Element.hasAttributes()
Element.removeAttribute()
- dataset 属性

# Text 节点和 DocumentFragment 节点

  - Text 节点的概念
  - Text 节点的属性
    - data
    - wholeText
    - length
    - nextElementSibling，previousElementSibling
  - Text 节点的方法
    - appendData()，deleteData()，insertData()，replaceData()，subStringData()
    - remove()
    - splitText()
  - DocumentFragment 节点

# CSS 操作

  - HTML 元素的 style 属性
  - CSSStyleDeclaration 接口
    - 简介
    - CSSStyleDeclaration 实例属性
    - CSSStyleDeclaration 实例方法
  - CSS 模块的侦测
  - CSS 对象
    - CSS.escape()
    - CSS.supports()
  - window.getComputedStyle()
  - CSS 伪元素
  - StyleSheet 接口
    - 概述
    - 实例属性
    - 实例方法
  - 实例：添加样式表
  - CSSRuleList 接口
  - CSSRule 接口
    - 概述
    - CSSRule 实例的属性
    - CSSStyleRule 接口
    - CSSMediaRule 接口
  - window.matchMedia()
    - 基本用法
    - MediaQueryList 接口的实例属性
    - MediaQueryList 接口的实例方法

# Mutation Observer API

  - 概述
  - MutationObserver 构造函数
  - MutationObserver 的实例方法
    - observe()
    - disconnect()，takeRecords（）
  - MutationRecord 对象
  - 应用示例
    - 子元素的变动
    - 属性的变动
    - 取代 DOMContentLoaded 事件
  - 参考链接
