---
title: DOM事件类
date: 2017-10-29 20:45:20
tags: DOM
---

## 一、基本概念：DOM事件的级别

- 事件级别（DOM标准定义的级别）
- DOM0 element.onclick = function(){}
- DOM2 element.addEventListener(‘click’,function(){},false)
- 默认false，冒泡；true为捕获。
- DOM3 element.addEventListener(‘keyup’,function(){},false)
- DOM1标准并非不存在，只是没有涉及和事件相关的东西

<!-- more -->

## 二、DOM事件模型

捕获：从上往下
冒泡：从下往上

## 三、DOM事件流

- 浏览器为当前页面与用户做交互（如点击鼠标左键）的过程中，左键传到页面上，然后给出响应。

- 三个阶段
  1. 捕获
  2. 目标阶段：事件通过捕获到达目标元素（如：点击的那个按钮，就为目标阶段）
  3. 冒泡：从目标元素上传到window对象

## 四、描述DOM事件捕获的具体流程

- 接收事件的顺序
    1. window
    2. document
    3. html(扩展：如何获取当前页面的html节点：document.documentElement)
    4. body
    5. …
    6. 最终：目标元素
- 冒泡则反向回去

## 五、Event对象的常见应用

- 简介：拿用户交互的参数，比如想知道按了哪个键，鼠标点了哪个键，基本都是从Event对象拿来的。
- 易混淆的5项：
    1. event.preventDefault() —- 阻止默认事件
    2. event.stopPropagation() —- 阻止冒泡
    3. event.stopImmediatePropagation()
        例：一个按钮，绑定2个事件，响应函数a、响应函数b，通过++优先级++的方式，若a被点击，b不执行，就要用到该语句，在函数a中加入该语句。
    4. event.currentTarget —-指向事件所绑定的元素（即事件委托中的父级）
    5. event.target —-始终指向事件发生时的元素
- 知识前提：
    事件委托：把子元素的事件都委托到父元素上，绑定一次事件即可。做响应时，要判断当前是哪个元素被点击，就要用到target。

## 六、自定义事件

```
var eve = new Event('custome');
ev.addEventListener('custome', function(){
    console.log('custome');
});
ev.ispatchEvent(eve); //触发eve对象
```

- CustomEvent和Event区别：CustomEvent还可以绑定obj参数，在用法上，两者一致。