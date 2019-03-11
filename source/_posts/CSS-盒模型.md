---
title: CSS_盒模型
date: 2019-02-28 10:45:53
tags: 
- 清除浮动的原理
- BFC
- JS获取宽高
- IE模型
- 标准模型
---

## 一、基本概念

**IE模型**：width/height 只包含content
**标准模型**：width/height 包含content + padding + border

- 内联样式：
  1. 行内的
  2. 写在html的style标签内的。
- 外联样式：link引入的样式表。

<!-- more -->

## 二、JS如何设置盒模型对应的宽和高

- dom.style.width/height: 只能获取内联样式，不够准确。
- dom.currentStyle.width/height：浏览器渲染之后的样式，不管之前样式是怎么获取的，比较准确，但只有IE支持。
- window.getComputedStyle(dom).width/height: 和上一个api原理相似，只不过可以兼容FireFox和chome。
- **dom.getBoundingClientRect().width/height**: 这个api也可以获取准确的属性值，可以获取相对视窗的绝对位置（左上角），有4个属性：
  1. top
  2. left
  3. width
  4. height

## 三、边距重叠问题

1. 取较大值
2. 利用如下实例说明
    ```html
    //父级 height:100px; 
    //子集 height：100px; 有 margin-top:10px;
    //且父级的背景色不显示
    <section id="sec"> 
        <style>
            #sec{
                background: red;
            }
            .child{
                height:100px;
                margin-top:10px;
                background:yellow;
            }
        </style>
        <article class="child"></article>
    </section>
    ```

    ```html
    //父级高度变为 110px；
    // BFC(边距重叠解决方案)
    <section id="sec">
        <style>
            #sec{
                background: red;
                overflow: hidden; //增加该属性
            }
            .child{
                height:100px;
                margin-top:10px;
                background:yellow;
            }
        </style>
        <article class="child"></article>
    </section>
    ```

- BFC的**概念**：块级元素格式化上下文(相应的-IFC-内联元素格式化上下文)
- BFC的**原理**（渲染规则）
  1. 在BFC这个元素的垂直方向的边距会产生重叠
  2. BFC的区域不会与浮动元素的box重叠（用来清除浮动）
  3. BFC在页面上是一个独立的容器，外面的元素不会影响它里面的元素，反之亦然
  4. 计算BFC高度时，浮动元素也会参与计算
- 如何**创建**BFC
  1.  float值不为none；
  2. position的值不是static或relative；
  3. display值为table、table-cell等和table有关的，block, list-item；
  4. overflow值不为visible，即hidden、scroll、auto都可，inherit(从父元素继承 overflow属性的值,所以不确定，因为可能继承到visible)
- BFC的**使用场景**：
  1. 边距重叠：给元素添加一个父级，并给父级创建BFC。（测试发现直接对元素创建BFC不管用，还是需要父级）
  2. 浮动元素重叠：2个浮动的元素，高度固定，当其中一块的内容超出，该元素会与另一浮动元素重叠，++对高度超出的元素创建BFC++，可解决该问题
  3. 清除浮动：BFC子元素即使是float，也会参与高度计算。（例子：子元素浮动，高度为100px，在浏览器F12发现父级高度为0，原因是float脱离文档流，没有参与计算，当给父级创建BFC，BFC计算高度时，浮动元素也会参与计算，此时父级高度变为了100px；若子元素为文字，父级有背景色，在清除浮动后，父级的背景色才可见。这就是之前用overflow：hidden；清除浮动的原理）

---
### 扩展

- float: left/right/none/inherit
- position:
    absolute: 绝对定位, 相对于 static 定位以外的第一个父元素进行定位。
    fixed: 绝对定位, 相对于浏览器窗口进行定位。
    relative: 相对定位, 相对于其正常位置进行定位。
    static: 默认值。没有定位，元素出现在正常的流中。
    inherit: 从父元素继承 position 属性的值。
- overflow:
    visible: 默认值。内容不会被修剪，会呈现在元素框之外。
    hidden: 内容会被修剪，并且其余内容是不可见的。
    scroll: 内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容。
    auto: 如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容。
    inherit: 从父元素继承 overflow 属性的值。
- display:
  
| 值 | 描述 |
| ---------------- | ------------------------ |
| none |此元素不会被显示。 |
| block | 此元素将显示为块级元素，此元素前后会带有换行符。 |
| inline | 默认。此元素会被显示为内联元素，元素前后没有换行符。 |
| inline-block | 行内块元素。（CSS2.1 新增的值）。 |
| list-item | 此元素会作为列表显示。 |
| run-in | 此元素会根据上下文作为块级元素或内联元素显示。 |
| compact | CSS中有值compact，不过由于缺乏广泛支持，已经从CSS2.1中删除。 |
| marker | CSS 中有值 marker，不过由于缺乏广泛支持，已经从 CSS2.1 中删除。|
| table | 此元素会作为块级表格来显示（类似table），表格前后带有换行符。 |
| inline-table | 此元素会作为内联表格来显示（类似table），表格前后没有换行符。 |
| table-row-group | 此元素会作为一个或多个行的分组来显示（类似tbody）。 |
| table-header-group | 此元素会作为一个或多个行的分组来显示（类似thead）。 |
| table-footer-group | 此元素会作为一个或多个行的分组来显示（类似tfoot）。 |
| table-row | 此元素会作为一个表格行显示（类似 tr）。 |
| table-column-group | 此元素会作为一个或多个列的分组来显示（类似colgroup）。 |
| table-column | 此元素会作为一个单元格列显示（类似 col）。 |
| table-cell | 此元素会作为一个表格单元格显示（类似 td 和 th）。 |
| table-caption | 此元素会作为一个表格标题显示（类似 caption）。 |
| inherit | 规定应该从父元素继承 display 属性的值。 |