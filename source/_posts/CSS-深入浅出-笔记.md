---
title: CSS_深入浅出_笔记
date: 2019-03-04 15:29:03
tags: CSS
---

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

## 宽度与高度

1. 元素的高度是行高决定的(单行)。

    默认行高是字体设计师确定的，在字体文件中有。在自定义很小的行高（比如 1px）时有可能会失效。

2. `&nbsp;`：no break space.

3. `text-align: justify;`： 多行文字两边对齐。

4. 如何让单行文字两边对齐？

    ![text-align.png](http://pntmc1hcw.bkt.clouddn.com/text-align.png)