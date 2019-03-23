---
title: JS题目总结（原型链、new、JSON、MVC、Promise）
date: 2019-03-19 17:57:58
tags: 
- 原型链
- new
- JSON
- MVC
- Promise
---

<!-- more -->

## 第一题

```js
var object = {}
object.__proto__ === Object.prototype // 为 true

var fn = function() {}
fn.__proto__ === Function.prototype  // 为 true
fn.__proto__.__proto__ === Object.prototype // 为 true

var array = []
array.__proto__ === Array.prototype // 为 true
array.__proto__.__proto__ === Object.prototype // 为 true

Function.__proto__ === Function.prototype // 为 true
Array.__proto__ === Function.prototype // 为 true
Object.__proto__ === Function.prototype // 为 true

true.__proto__ === Boolean.prototype // 为 true

Function.prototype.__proto__ === Object.prototype // 为 true
```

## 第二题

```js
function fn() {
    console.log(this)
}
new fn()
```

new fn() 会执行 fn，并打印出 this，请问这个 this 有哪些属性？这个 this 的原型有哪些属性？

答：this 为构造函数 fn 的一个实例，有内置属性 `[[Prototype]]`，在部分浏览器中表现为 `__proto__` 属性；
`this.__proto__ === fn.prototype // true`，this 的原型的属性有 `constructor`、`__proto__`。
    
```js
// 换成这样做
function fn() {
    console.log(this)
}
var f1 = new fn()
console.log('(f1.__proto__ === fn.prototype) =>', f1.__proto__ === fn.prototype)
console.log(f1)
console.log(f1.__proto__)
```

## 第三题

JSON vs JS => 博客-发送请求.md

## 第四题

1. 前端 MVC 是什么？

  答：MVC模式是软件工程中一种软件架构模式，一般把软件模式分为三部分，模型(Model)+视图(View)+控制器(Controller)
    
  **模型**：模型用于封装与应用程序的业务逻辑相关的数据以及对数据处理的方法。模型有对数据直接访问的权利。模型不依赖 “视图” 和 “控制器”, 也就是说 模型它不关心页面如何显示及如何被操作.
  
  **视图**：视图层最主要的是监听模型层上的数据改变，并且实时的更新html页面。当然也包括一些事件的注册或者ajax请求操作(发布事件),都是放在视图层来完成。
  
  **控制器**：控制器接收用户的操作，最主要是订阅视图层的事件，然后调用模型或视图去完成用户的操作比如：当页面上触发一个事件，控制器不输出任何东西及对页面做任何处理 它只是接收请求并决定调用模型中的那个方法去处理请求, 然后再确定调用那个视图中的方法来显示返回的数据。

2. 请用代码大概说明 MVC 三个对象分别有哪些重要属性和方法。

```js
/*
View('.xxx')
*/
window.View = function (selector) {
  return document.querySelector(selector)
}
```

```js
/*
Model({
  'resourceName': xxx
})
*/
window.Model = function(object) {
    let resourceName = object.resourceName
    return {
        init: function() {},
        fetch: function() {},
        save: function(object) {}
    }
}
```

```js
/*
Controller({
    init: function(view, model) {
        this.view = xxx
        this.model = xxx
        this.xxx()
        this.yyy()
    },
    xxx: function() {},
    yyy: function() {}
})
*/
window.Controller = function(options) {
    let init = options.init
    let object = {
        view: null,
        model: null,
        init: function(view, model) {
            this.view = view
            this.model = model
            this.model.init()
            init.call(this, view, model)
            // this.bindEvents.call(this)
        }
    }
    for (let key in options) {
        if (key !== 'init') {
            object[key] = options[key]
        }
    }
    return object
}
```

## 第五题

如何在 ES5 中如何用函数模拟一个类？（10分）

答：使用原型对象，构造函数，new来模拟类。

将公共属性放到原型对象里，并且将构造函数的prototype属性指向原型对象。
私有属性(自有属性)放到构造函数里去定义。
将实例化的对象的__proto__指向原型对象。
这样当构造函数创建一个实例化的对象的时候，就即拥有自己的私有变量和方法，也有公有的变量和方法了，实例化出来的对象的私有方法和变量修改都不会互相有影响，只有在修改公有的变量和方法的时候是对所有实例生效的。

```js
function Dessert(name) {
    this.name = name
}
Dessert.prototype.calorie = function() {}
let dessert = new Dessert('iceCream')
```
上面代码就是一个最简单的类，Dessert 构造函数创建出来的对象自身有 name 属性，其原型上面有一个 calorie 属性。

## 第六题

用过 Promise 吗？举例说明。
如果要你创建一个返回 Promise 对象的函数，你会怎么写？举例说明。

答：用过，调用后端接口对获取到的数据进行处理、自己模仿 jQuery 封装 Ajax、MVC 中 Model 的 fetch 属性和 save 属性返回 Promise 对象；

```js
/**
* yyy().then(successFn, failFn)
*/

function yyy() {
    return new Promise(function(resolve, reject) {
        if (success) {
           resolve.call(undefined, ...zzz)
        } 
        else if (fail) {
           reject.call(undefined, ...qqq)
        }
    })
}
```
