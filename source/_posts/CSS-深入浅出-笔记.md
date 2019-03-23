---
title: CSS_深入浅出_笔记
date: 2019-03-04 15:29:03
tags: CSS
---

## 1. CSS 学习思路

1. 监测是否支持 touch 事件：
  (1)'ontouchstart' in documentbody
  (2)document.body.ontouchstart === undefined (不支持。 === null 支持)
2. p标签 内，img float，文字则环绕 <!-- more -->
3. github markdown css
4. border 和 margin，display: table.flex, 阻断 margin 的合并；overflow: hidden;
5. li 小圆点，display: list-item; 的属性，换了 display，就没有小圆点
6. 内联元素display: inline/inline-block，position: absolute; 后，-> display: block;
7. 一元素 position: fixed; 在 css 中插入内容，父级使用 transform: scale(); 此时，该元素变成相对于父级定位
8. ![css-1-1.png](http://pntmc1hcw.bkt.clouddn.com/css-1-1.png) '你好'没有在浮动元素下，相当于文字环绕浮动图片
9. <a href="https://www.google.com/search?q=CSS+3+generator" target="_blank">CSS 3 generator</a>
10. <a href="//cndevdocs.com" target="_blank">cndevdocs.com</a>

<!-- more -->

## 2. 宽度与高度

1. 元素的高度是行高决定的(单行)。

    默认行高是字体设计师确定的，在字体文件中有。在自定义很小的行高（比如 1px）时有可能会失效。

2. `&nbsp;`：no break space.

3. `text-align: justify;`： 多行文字两边对齐。

4. 如何让单行文字两边对齐？

    ![text-align.png](http://pntmc1hcw.bkt.clouddn.com/text-align.png)

5. 换行：单词加上 `-` 才会换行。
6. 一行文字超出**省略号**
```css
.inlineText {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
```
7. 多行省略号
   ```css
   {
     display: -webkit-box;
     -webkit-line-clamp: 2;
     -webkit-box-orient: vertical;
     overflow: hidden;
   }
   ```
8. div 的宽度不是文字决定的。
9. 浏览器份额查询关键系：百度统计 浏览器。
10. **margin 合并**：父级的四周没有东西挡起来（e.g., border/padding/overflow: hidden;/在父级内加文字可以），父级和子级的 margin 会合并。测试时可以给父级加 `outline: 1px solid red`。
    
### 总结

#### div 的高度是由什么决定的

div 高度是由它内部文档流中的元素高度总和**决定**的，并不是相等。
div 宽度默认自适应。

**正方形**：

```css
{
  border: 1px solid;

  /* 100% 表示和宽度一样 */
  padding-top: 100%;
}
```

#### span 的高度、宽度是由什么决定的

高度：行高决定。
宽度：内容 + 左右margin + 左右padding + 左右border宽度

#### 文档流

文档流就是内联元素从左到右，块级元素从上到下。

##### 脱离文档流

算高度的时候别算上我

- 方法
  - float
  - position: absolute;
  - position: fixed;

#### 绝对居中

使用场景：例如父级为全屏显示的元素时。

##### 子元素要定宽定高

```css
.son {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  margin: auto;
}
```

##### flex

IE 不兼容

```css
.dad {
  display: flex;
  justify-content: center;
  align-items: center;
  /* height: 100vh; */
}
```

## 3. 堆叠上下文

资料：[深入理解CSS中的层叠上下文和层叠顺序](https://www.zhangxinxu.com/wordpress/2016/01/understand-css-stacking-context-order-z-index/)

### 什么是堆叠顺序(The stacking order)

1. background
2. border
3. 块级
4. 浮动
5. 内联
6. z-index: 0
7. z-index: +
   
如果是兄弟元素重叠，那么后面的盖在前面的身上。

![stacking-order.png](http://pntmc1hcw.bkt.clouddn.com//blog/img/stacking-order.png)


### 堆叠上下文(The stacking context)

https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Understanding_z_index/The_stacking_context

可以理解为堆叠作用域。跟 BFC 一样，我们只知道一些属性会触发堆叠上下文，但并不知道堆叠上下文是什么。

1.根元素 (HTML),
1.z-index 值不为 "auto"的 绝对/相对定位，
1.一个 z-index 值不为 "auto"的 flex 项目 (flex item)，即：父元素 display: flex|inline-flex，
1.opacity 属性值小于 1 的元素（参考 the specification for opacity），
1.transform 属性值不为 "none"的元素，
1.mix-blend-mode 属性值不为 "normal"的元素，
1.filter值不为“none”的元素，
1.perspective值不为“none”的元素，
1.isolation 属性被设置为 "isolate"的元素，
1.position: fixed
1.在 will-change 中指定了任意 CSS 属性，即便你没有直接指定这些属性的值（参考 这篇文章）
1.-webkit-overflow-scrolling 属性被设置 "touch"的元素