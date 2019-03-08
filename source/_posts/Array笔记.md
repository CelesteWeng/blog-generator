---
title: Array笔记
date: 2019-03-07 15:40:58
tags: array
---

-  基本类型构造函数加 new ，才是对象
  
```
Number() // 基本类型
new Number() // 对象
```
<!-- more -->

- 创建函数
  
```
var f = new Function('a', 'b', 'return a+b')
```

数组也可像对象一样赋值（数组是你觉得它是数组）
![array-1.png](http://pntmc1hcw.bkt.clouddn.com/array-1.png)
但是 length 依旧为3
![array-2.png](http://pntmc1hcw.bkt.clouddn.com/array-2.png)
不同遍历方法
![array-2-2.png](http://pntmc1hcw.bkt.clouddn.com/array-2-2.png)

- 伪数组：实例的 `[[Prototype]]` 内置属性（浏览器中表现为 `__proto__`）中，没有 `Array.prototype`。
  - e.g, `arguments`.

```
function xxx() { console.log('hi') }

// 执行
xxx() 
xxx.call()

// 放到内存中，不执行
xxx
```

-  arr.forEach(function (`currentValue`, `index`, `array`) {}, `thisArg`);
  通过 `this` 获取 array
  ![array-3.png](http://pntmc1hcw.bkt.clouddn.com/array-3.png)

- arr.sort(function (a, b) { return a - b }) 
  - a - b 从小到大；b - a 从大到小。
  - 只有 sort 会改变原值

- arr + '' === arr.join() // 实际上调用了 toString() 方法
  
- arr.map() 有返回值，map: 映射。

- arr.filter() 返回布尔值，只留下符合条件的
  - filter 和 map 一起用，对过滤的值操作。`arr.filter((item, index) => item === index).map(t => t + 1)`

  ![array-4.png](http://pntmc1hcw.bkt.clouddn.com/array-4.png)
  ![array-6.png](http://pntmc1hcw.bkt.clouddn.com/array-6.png)
   
- arr.reduce()，reduce(减少，归纳为 => 求和)
  `a.reduce((prevSum, n) => prevSum + n, 0)` prevSum 上一次运算的结果，n 当前值
  
  范围打劫
  ![array-5-2.png](http://pntmc1hcw.bkt.clouddn.com/array-5-2.png)
  ![array-5.png](http://pntmc1hcw.bkt.clouddn.com/array-5.png)