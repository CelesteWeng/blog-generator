---
title: 无分类笔记（解构赋值、立即执行函数、闭包、MVC、Promise）
date: 2019-03-16 13:10:02
tags: 
- 解构赋值
- 使用立即执行函数
- 使用闭包
- MVC
- Promise
- 封装 jQuery.ajax
---

#### ES6 解构赋值

```js
var fn = function(options) {
  let { url, method, body } = options
}
```

<!-- more -->

从作为函数实参的对象中提取数据

```js
// 直接将第一个参数解构并声明，相当于用 let 声明
var fn = function({url, method, body}){
  ...
}
```

交换变量的值

```js
var a = 'a'
var b = 'b'

[a, b] = [b, a]
```

其余功能：加`()`免声明（要在语句前加`;`）、修改属性名、忽略某个值（用`,`分割，空出下标对应项）、`...rest`剩余项赋值给一个变量、设置默认值（等号左边用`=`，等号右边用`:`）

[解构赋值--MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

#### Promise 的意义

1. 不用取名字。不用去记参数名，直接在 `.then()`中写，第一个参数为成功的回调，第二个参数为失败的回调。
2. `.then`可以对一个状态进行多次处理
   
`jQuery.ajax`中 responseText ("Content-Type": "text/json")，会自动转化为对象。

封装 jQuery.ajax

```js
window.jQuery = function(nodesOrSelector) {
  let nodes = {}
  nodes.addClass = function() {}
  node.textContent = function() {}
  return nodes
}
window.$ = window.jQuery
```

```js
window.jQuery.ajax = function({url, method, headers, body, success, fail}){
  let request = new XMLHttpRequest()
  request.open(method, url)
  for(let key in headers) {
    let value = headers[key]
    request.setRequestHeader(key, value)
  }
  request.onreadystatechange = () => {
    if (request.readyState === 4) {
      if (request.status >= 200 && request.status < 300) {
        success.call(undefined, request.responseText)
      } else if (request.status >= 400) {
        fail.call(undefined, request)
      }
    }
  }
  request.send()
}
```

```js
function doSthIfSuccess(responseText) {
  // ...
}

myButton.addEventListener('click', (e) => {
  $.ajax({
    url: '/xxx',
    method: 'get',
    headers: {
      'Content-ype': 'application/x-www-form-urlencoded',
      'Name': 'Celeste'
    },
    success: (responseText) => {
      doSthIfSuccess.call(undefined, responseText)
    },
    fail: (response) => {
      console.log(response, response.status, response.responseText)
    }
  })
})
```

---

```js
// 自己封装 jQuery.ajax 满足 Promise 规则
window.jQuery.ajax = function({ url, method, body, headers }) {
    return new Promise(function(resolve, reject) {
        let request = new XMLHttpRequest()
        request.open(method, url)
        for (let key in headers) {
            let value = headers[key]
            request.setRequestHeader(key, value)
        }
        request.onreadystatechange = () => {
            if (request.readyState === 4) {
                if (request.status >= 200 && request.status < 300) {
                    resolve.call(undefined, request.responseText)
                } else if (request.status >= 400) {
                    reject.call(undefined, request)
                }
            }
            request.send()
        }
    })
}
```

```js
myButton.addEventListener('click', (e) => {
  let promise = jQuery.ajax({
    url: '/xxx',
    method: 'get',
    headers: {
      'Content-Type': 'application/x-www-form-urlencoded',
      'Name': 'Alice'
    }
  })

  promise.then(
    (text) => { console.log(text) },
    (request) => { console.log(request) }
  )
})

// 上述代码相当于
myButton.addEventListener('click', (e) => {
  jQuery.ajax({
    url: '/xxx',
    method: 'get',
    headers: {
      'Content-Type': 'application/x-www-form-urlencoded',
      'Name': 'Alice'
    }
  }).then(
    (text) => { console.log(text) },
    (request) => { console.log(request) }
  )
})
```

#### 如何使用立即执行函数

1. 我们不想要全局变量
2. 我们要使用局部变量
3. ES 5 里面，只有函数有局部变量
4. 于是我们声明一个 function xxx，然后 xxx.call()
5. 这个时候 xxx 是全局变量（全局函数）
6. 所以我们不能给这个函数名字
7. function(){}.call()
8. 但是 Chrome 报错，语法错误
9. 试出来一种方法可以不报错:
   i. !function(){}.call() (我们不在乎这个匿名函数的返回值，所以加个 ! 取反没关系)
   ii. (function(){}).call() 方方不推荐
      xxx
      (function(){}).call() 报错
   iii. frank192837192463981273912873098127912378.call() 不推荐

#### 如何使用闭包

```js
!function(){
  var person = {
    name: 'xiaoming',
    age: 3
  }
  window.personGrowUp = function(){
    person.age += 1
    return person.age
  }
}.call()
```

```js
var accessor = function() { // accessor 为 返回匿名函数 的 匿名函数
  var person = {
    name: 'xiaoming',
    age: 3
  }
  return function(){
    person.age += 1
    return person.age
  }
}

var growUp = accessor.call() // growUp 为函数

growUp.call()
```

1. 立即执行函数使得 person 无法被外部访问
2. 闭包使得匿名函数可以操作 person
3. window.frankGrowUp 保存了匿名函数的地址
4. 任何地方都可以使用 window.frankGrowUp
   => 任何地方都可以使用 window.frankGrowUp 操作 person，但是不能直接访问 person

```js
// MVC的M和V
!function(){
  var view = document.querySelector('#topNavBar')
  var controller = function(view){
    window.addEventListener('scroll', function(x) {
      // ...  
    })
  }
  controller.call(undefined, view)
}
```

```js
// 将 controller 变成对象
!function(){
  var view = document.querySelector('#topNavBar')

  var controller = {
    view: null,
    init: function(view) {
      this.view = view
      this.bindEvents()
      // this.bindEvents.call(this)
    },
    bindEvents: function() {
      var view = this.view
      window.addEventListener('scroll', function(e) {
        if (window.scrollY > 0) {
          view.classList.add('sticky')
        } else {
          view.classList.remove('sticky')
        }
      })
    }
  }

  controller.init(view)
  // controller.init.call(controller, view)
}.call()
```

```js
// 将对 view 的操作全部放在 controller 的属性内，使代码的结构变得清晰
!function(){
  var view = document.querySelector('#topNavBar')

  var controller = {
    view: null,
    init: function(view) {
      this.view = view
      this.bindEvents()
      // this.bindEvents.call(this)
    },
    bindEvents: function() {
      var view = this.view
      window.addEventListener('scroll', (e) => {
        // 1. 函数内 this 为触发事件的元素，使用 function(){} 需要绑定 this
        // function(){}.bind(this)
        // 2. 使用箭头函数。箭头函数没有 this，没有箭头函数的内外 this 是一样的
        if (window.scrollY > 0) {
          this.active()
        } else {
          this.deactive()
        }
      })
    }，
    active: function() {
      this.view.classList.add('sticky')
    },
    deactive: function() {
      this.view.classList.remove('sticky')
    }

  }

  controller.init(view)
  // controller.init.call(controller, view)
}.call()
```

#### MVC

MVC 是一种组织代码的思想，使代码结构更清晰，方便后续的修改的扩展。

![MVC.png](http://pntmc1hcw.bkt.clouddn.com//blog/img/MVC.png)

MVC 将代码分成三块：

   1. 控制器（Controller）- 负责转发请求，对请求进行处理（负责其他的所有事情）。
   2. 视图（View） - 界面设计人员进行图形界面设计（你的代码长什么样子或者你的代码在哪一块）。
   3. 模型（Model） - 程序员编写程序应有的功能（实现算法等等）、数据库专家进行数据管理和数据库设计(可以实现具体的功能)（一个操作数据的对象。初始化、获取和保存等）。

Model 和服务器交互，Model 将得到的数据交给 Controller，Controller 把数据填入 View，并监听 View
用户操作 View，如点击按钮，Controller 就会接受到点击事件，Controller 这时会去调用 Model，Model 会与服务器交互，得到数据后返回给 Controller，Controller 得到数据就去更新 View。

MVC模式是软件工程中一种软件架构模式，一般把软件模式分为三部分，模型(Model)+视图(View)+控制器(Controller);
  
  模型：模型用于封装与应用程序的业务逻辑相关的数据以及对数据处理的方法。模型有对数据直接访问的权利。模型不依赖 “视图” 和 “控制器”, 也就是说 模型它不关心页面如何显示及如何被操作.
  
  视图：视图层最主要的是监听模型层上的数据改变，并且实时的更新html页面。当然也包括一些事件的注册或者ajax请求操作(发布事件),都是放在视图层来完成。
  
  控制器：控制器接收用户的操作，最主要是订阅视图层的事件，然后调用模型或视图去完成用户的操作;比如：当页面上触发一个事件，控制器不输出任何东西及对页面做任何处理; 它只是接收请求并决定调用模型中的那个方法去处理请求, 然后再确定调用那个视图中的方法来显示返回的数据。


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