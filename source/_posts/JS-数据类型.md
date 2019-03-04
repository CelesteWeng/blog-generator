---
title: JS_数据类型
date: 2019-03-04 12:36:28
tags: 
- JS的历史
- JS数据类型
---

## 一、 JS 的历史

- 1991年，李爵士 发明了 www 万维网
- 1992年，同事发明了  CSS
- 1993年，李爵士 创办了 W3C
- 1995年，网景 Netscape（公司） -> Navigator（浏览器） 支持脚本，之前只支持 HTML + CSS
<!-- more -->
- JS 之父：Brendan Eich，JS 最初叫 Mocha -> liveScript -> JavaScript，10天完成了设计
- 网景和 Sun 公司（发明 java）达成协议，JS 和 Java 一起发布
- 此时 JS 还缺少很多模块，编码有问题，Unicode 在之后才发布了 UTF-8
- 1996，MS IE -> JScript
- 网景 被打败，开源 -> Firefox
- IE 5.5，MS 推出 JS 发请求功能
- 2004，Gmail 利用 JS 发请求功能，做了网页上的程序。JS 变成正式的编程语言
- 2010左右，中国 前端 Front-end
- 网景为了打败 MS，向 ECMA 申报标准 （ECMAScript）
- JS 不行：全局变量（没有模块化）、标准库（内置代码少）
- ECMAScript 4 死了，ECMAScript 5 做了小升级，步子太小
- ES6：Rails 社区 Ruby —— CoffeeScript，JS 改良版。类，箭头函数
- ES5 不兼容 IE7，ES 6 不兼容 IE8

## 二、7种数据类型

  - ECMAScript 的变量是松散类型的，所谓松散类型就是可以用来保存任何类型的数据。换句话说， 每个变量仅仅是一个用于保存值的占位符而已。
  - Safari 5 及之前版本、Chrome 7 及之 前版本在对正则表达式调用 typeof 操作符时会返回"function"，而其他浏览器在这种情况下会返回 "object"。

### 1. Number 数字
   - `.1` 可表示 0.1；
   - 0b 二进制，011 八进制，0x 十六进制
  
#### i. 浮点数值
 - 带小数点，但实际上为整型的变量会保存为整型。
 - ECMASctipt 会将那些小数点后面带有 6 个零以上的浮点数值转换为以 e 表示法 表示的数值(例如，0.0000003 会被转换成 3e-7)。
 - 浮点数值的最高精度是 17 位小数，但在进行算术计算时其精确度远远不如整数。例如，0.1 加 0.2 的结果不是 0.3，而是 0.30000000000000004。这个小小的舍入误差会导致无法测试特定的浮点数值。
 - 我们测试的是两个数的和是不是等于 0.3。如果这两个数是 0.05 和 0.25，或者是 0.15 和 0.15 都不会有问题。而如前所述，如果这两个数是 0.1 和 0.2，那么测试将无法通过。因此，永远不 要测试某个特定的浮点数值。
 - 关于浮点数值计算会产生舍入误差的问题，有一点需要明确:这是使用基于 IEEE754 数值的浮点计算的通病，ECMAScript 并非独此一家;其他使用相同数值格 式的语言也存在这个问题。

#### ii. 数值范围
 - ECMAScript 能够表示的最小数值保 存在 Number.MIN_VALUE 中——在大多数浏览器中，这个值是 5e-324。
 - 能够表示的最大数值保存在 Number.MAX_VALUE 中——在大多数浏览器中，这个值是 1.7976931348623157e+308。
 - 要想确定一个数值是不是有穷的(换句话说，是不是位于最 小和最大的数值之间)，可以使用 isFinite()函数。返 回 Boolean 值。
 - 访问 Number.NEGATIVE_INFINITY 和 Number.POSITIVE_INFINITY 也可以 得到负和正 Infinity 的值。可以想见，这两个属性中分别保存着-Infinity 和 Infinity。

#### iii. NaN
  在 ECMAScript 中，任何数值除以 0 会返回 NaN 
  （但实际上只有 0 除以 0 才会返回 NaN，正数除以 0 返回 Infinity，负数除以 0 返回-Infinity。）
  NaN，即非数值(Not a Number)是一个特殊的数值 
  NaN 与任何值都不相等，包括 NaN 本身 
  isNaN()函数 ：判断是否为 NaN
  parseInt() 函数
  var num1 = parseInt("10", 2);
  var num2 = parseInt("10", 8);
  var num3 = parseInt("10", 10);
  var num4 = parseInt("10", 16);
  //2 (按二进制解析) //8 (按八进制解析) //10(按十进制解析) //16(按十六进制解析) 
  
### 2. String 字符串
   - 空字符串 长度为0，空格字符串 长度为1；
   - 转义：`\` + `xxx`，`\n` 回车，`\t` Tab 制表符，长度为1
   - 多行字符串（不是字符串里有回车）：
  ```
    // 坑
    var s = '1234 \
            5678'

    // 推荐
    var s2 = '1234' +
            '5678'

    // 很多空格 会报错，因为没有闭合
    var s3 = '1234 \______ 
              5678'

    // ES6 特性 模板字符串包含回车 length: 9
    var s4 = `1234
    6789`
    ```

### 3. Boolean 布尔
 - Boolean，数学家，下雨去上课没带伞，肺病死了。。。
 - `if ("222") { console.log('代码执行到此处') }` 实际上打印的代码不会执行，<a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness#Loose_equality_using" target="_blank">MDN 对于宽松相等有表格总结</a>，"222" -> 222，true -> 1，222 != 1；<br>建议：<br>（1）== 两端有 `true` 和 `false`，千万不要使用 == <br>（2）== 两端有 `[]`、`""`，或者 `0`，尽量不要使用 ==
  
### 4. Symbol(符号)
  Symbol 生成一个全局唯一的值。Symbol 的值和名字没有关系。
  ```
  var a1 = Symbol('a')
  var a2 = Symbol('a')
  a1 !== a2 // true
  ```

### 5. Null(对象)
 - 要保存对象的变量还没真正保存对象，空对象指针
 - 调用 typeof null 会返回"object"
  
### 6. Undefined(声明变量后没赋值 / 没有声明该变量)
 - 未初始化的变量会自动被赋予 undefined 值，显式地初始化变化更为明智。如此在 typeof 操作符返回 'undefined' 值时，可知道被检测变量还未被声明。
  
### 7. Object 对象

 - key 不加引号，要遵守标识符的规则: <br>（1）不能数字开头；（2）不能有空格；（3）合法字符
 - key 符合标识符的情况下可用 `obj.key`，其余 `obj['key']`
 - `delete obj['key']` 删除 key，`obj['key'] = undefined` 只是将值置为 undefined

## typeof 操作符  
"undefined"——如果这个值未定义;
"boolean"——如果这个值是布尔值;
"number"——如果这个值是数值;
"object"——如果这个值是对象或 `null`;
"function"——如果这个值是函数。 // function 不在7种类型中

调用 typeof null 会返回"object"，因为特殊值 null 被认为是一个空的对象引用。 

Safari 5 及之前版本、Chrome 7 及之 前版本在对正则表达式调用 typeof 操作符时会返回"function"，而其他浏览器在这种情况下会返回 "object"。

