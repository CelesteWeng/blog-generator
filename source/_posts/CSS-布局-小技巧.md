---
title: CSS_布局&小技巧
date: 2019-02-28 10:10:51
tags: CSS
---

## 左中右布局

<a href="https://celesteweng.com/CSS_layout/horizontal-layout.html" target="_blank">查看在线 demo （调整浏览器宽度见效果）</a>

- 5种布局：float、绝对定位、flex、表格、网格
- 网格布局兼容性差，测试发现chome、FireFox新版本兼容，360浏览器grid挂了。

1. 浮动-优点：兼容性高；缺点：浮动要清除，关系要处理好
2. 绝对定位-优点：快捷；缺点：脱离文档流了，之后的元素也要脱离文档流，可使用性差
3. flex-css3出现，移动端，比较完美，不兼容IE8
4. 表格-优点：兼容性好，缺点：三栏，其中一个单元格高度超出，别的单元格也会调整
5. 网格-新技术-优点：代码量少，缺点：只兼容ie11+

<!-- more -->

## 垂直布局

<a href="https://celesteweng.com/CSS_layout/vertical-layout.html" target="_blank">查看在线 demo （调整浏览器高度见效果）</a>

- 4种布局：绝对定位、flex、表格、网格
- 网格布局兼容性差，测试发现chome、FireFox新版本兼容，360浏览器grid挂了。 垂直方向与水平方向在编写是略有不同，需要设
```
html, body, ...{ //省略号代表包含items的标签
    height:100%;
    width:... //宽度自设
}
```

## 左右布局 (资料来自博客，抄抄大法好)

- 分类：2列定宽（不说了）、1列定宽、2列都自适应

### 左列定宽，右列自适应

#### margin + float

```
<div class="parent">
    <div class="left"><p>left</p></div>
    <div class="right-fix">
        <div class="right">
            <p>right</p><p>right</p>
        </div>
    </div>
</div>
```

```
 .left{
    float: left;     //向左浮动
    width: 100px;    //固定宽度
    position: relative;//由于.left与.right-fix重合，且.right-fix在DOM树上的位置比.left要后，因此.right-fix会遮挡住.left，设置.left为relative可以让其冒出来。
}
.right-fix{
    float: right;     //向右浮动
    width: 100%;    //为了自适应设为100%
    margin-left: -100px;//由于宽度设为100%，.right-fix遭到浏览器换行处理；因此通过设置负的margin值，在左侧制造出100px的空白，使.right-fix与.left重合（即处于同一行）
}
.right{
    margin-left: 120px;    //由于.left和.right-fix重合了，因此给.right设置一个margin-left，避免内容区（.right）与.left重合。另外，120px - 100px = 多出来的20px实际上就相当于.left和.right之间的间隔了。
}
```

这个方法其实已经是兼顾到低版本IE的完善版本了，缺点是代码冗长，而且有两个关键知识点比较难理解：

  1. 给.left加上position:relative;怎么就能让.left冒出来而不受.right-fix的遮挡了呢？
  2. .right-fix设置负的margin-left，怎么就能使.left与.right-fix重合了呢？

再者，这个方案由于.right-fix的margin-left和.left的width高度耦合，因此无法实现自适应，只能应用在左列（当然右列也成）固定宽度的场景。


#### absolute

```
<div class="parent">
    <div class="left">
        <p>left</p>
    </div>
    <div class="right">
        <p>right</p>
        <p>right</p>
    </div>
</div>
```

```
.parent{
    position: relative;
}
.left{
    position: absolute;
    left: 0;
    width: 100px;
}
.right{
    position: absolute;
    left: 120px;    //比.left的left多出20px，相当于间隔
    right: 0;
}
```

这种方法是通过absolute配合left/right进行布局：
1. 设置display: absolute后，通过top/right/bottom/left可以实现对元素的位置进行像素级的任意控制。因此，使用left属性即可控制各元素的起始位置，避免重叠。
2. 自适应的关键在于left和right属性，在对元素同时设置这两个属性后，元素的宽度便会遭到拉伸，实现自适应。
3. 需要注意的是父级元素需要设置display: relative。

这种方案很容易理解，但缺点就是不能做到“**不定宽**”，因为.left和.right的left属性的值高度相关。

### 左列不定宽，右列自适应

#### float + BFC

```
<div class="parent">
    <div class="left">
        <p>left</p>
    </div>
    <div class="right">
        <p>right</p>
        <p>right</p>
    </div>
</div>
```

```
.left{
    float: left;
    width: 100px;
    margin-right: 20px;    //形成20px的间隔
}
.right{
    overflow: hidden; //通过设置overflow: hidden来触发BFC特性
}
```

这个方法主要是应用到BFC的一个特性：

1. 浮动元素的块状兄弟元素会无视浮动元素的位置，尽量占满一整行，这样该兄弟元素就会被浮动元素覆盖。
2. 若浮动元素的块状兄弟元素为BFC，则不会占满一整行，而是根据浮动元素的宽度，占据该行剩下的宽度，避免与浮动元素重叠。
3. 浮动元素与其块状BFC兄弟元素之间的margin可以生效，这将继续减少兄弟元素的宽度。

并不是一定要在.right上用overflow: hidden;，只要能触发BFC就好了，另外在IE6上也可以触发haslayout特性（推荐用*zoom: 1;）。

由于.right的宽度是自动计算的，不需要设置任何与.left宽度相关的css，因此.left的宽度可以不固定（由内容盒子决定）。

#### table布局

```
<div class="parent">
    <div class="left">
        <p>left</p>
    </div>
    <div class="right">
        <p>right</p>
        <p>right</p>
    </div>
</div>
```

```
.parent{
    display: table; width: 100%;
    table-layout: fixed;
}
.left,.right{
    display: table-cell;
}
.left{
    width: 100px;
    padding-right: 20px;
}
```

这个方法是表格布局的典型运用。说真的，我也很迷惘要不要使用表格布局，毕竟已经是上个时代的产物了，虽然已经不再用`<table>`的HTML结构了，但用上相应的CSS其实思路跟以前是变化不大的。

这个方法主要是利用了表格(table)的宽度必然等于其所有单元格(table-cell)加起来的总宽度，那么只要表格的宽度一定，其中一个（或几个）单元格的宽度也一定，那么另外一个未设置宽度的单元格则会默认占满剩下的宽度，即实现自适应。

#### flex

```
<div class="parent">
    <div class="left">
        <p>left</p>
    </div>
    <div class="right">
        <p>right</p>
        <p>right</p>
    </div>
</div>
```

```
.parent{
    display: flex;
}
.left{
    margin-right: 20px;
}
.right{
    flex: 1;
}
.left p{width: 200px;
}
```

flex布局的自适应我就不多说了，本来就是设计来自适应的，只需要用上`flex: 1;`，就能让.right分到.parent的宽度减去.left的宽度。

#### 推荐使用

比较推荐用float + `BFC`方案，浏览器兼容性很好，代码量也少，另外也很好理解；移动端上也可以考虑用上flex方案，不过还是那一句，注意用旧版的flex，兼容性会好一点。

#### BFC 是什么？我失忆了，翻了下笔记

<a href="https://celesteweng.github.io/2019/02/28/CSS-盒模型/" target="_blank">详情见 >> CSS 盒模型</a>

## 水平居中/垂直居中

https://css-tricks.com/centering-css-complete-guide/

文盲如何看懂英文，chrome 浏览器 翻译

---
## 其他小技巧

1. 行内元素设置了 position：fixed/absolute 或者 float 属性，即`脱离文档流`，display 隐性更改为 inline-block
2. 加了 `display:inline-block;`，元素底部多出间隙，加 `vertical-align:top;`
3. 行内元素不能设置宽高，上下边距
4. 所有的非空标签都有伪类（::before ::after）
5. height 和width高度不要定死，容易出bug
6. ionic 中用 `position：fixed` 容易变态
7. <a href="https://segmentfault.com/a/1190000005863953" target="_blank">【译】22个必备的CSS小技巧
8. 搞来抄一抄 https://codepen.io/pens/
9. Google