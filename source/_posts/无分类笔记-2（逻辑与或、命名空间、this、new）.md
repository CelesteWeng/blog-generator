---
title: 无分类笔记-2（逻辑与或、命名空间、this、new、异步）
date: 2019-03-18 11:19:36
tags: 
- 逻辑与或
- 命名空间
- this
- new
- 异步、回调
---

# 面相对象编程吧

## 逻辑与或

### &&

有假的，返回第一个 falsy 值，遇到 falsy 值就不往后判断了；全是真的，返回最后一个 truy 值。

falsy值：`0`、`NaN`、`null`、`undefined`、`""`、`false`.

### ||

有真的，返回第一个 truy 值，遇到 truy 值就中断；全是假的，返回最后一个 falsy 值。

## 命名空间

命名空间（Namespace），也称名称空间等，它表示着一个标识符（identifier）的可见范围。 一个标识符可在多个命名空间中定义，它在不同命名空间中的含义是互不相干的。

(e.g., window.jQuery、电脑中的文件夹)

## this

`element.onclick = functionRef;`

在函数内，this 是触发当前事件的元素。

## new

![prototype.png](http://pntmc1hcw.bkt.clouddn.com//blog/img/prototype.png)
![prototype-2.png](http://pntmc1hcw.bkt.clouddn.com//blog/img/prototype-2.png)

## 异步、回调

异步：【不等结果】直接执行下一步。
如何拿到结果 => 回调可以拿到异步的结果

【回调是拿到异步结果的一种方式】
【回调也可以拿到同步结果】