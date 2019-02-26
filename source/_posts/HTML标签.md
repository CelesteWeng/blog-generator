---
title: HTML标签
date: 2019-02-25 19:35:23
tags: HTML
---

## iframe 标签

嵌套页面

`<iframe src="https://www.baidu.com" name="xxx"></iframe>`

1. `name` 属性可以和 a 标签配合使用
2. 使用 `frameborder="0"` 去掉元素边框
3. 相当新开一个窗口，用起来卡，过时
4. `src` 可以写相对路径

<!-- more -->

## a 标签

跳转页面（HTTP GET 请求）

更多属性见 >> <a href="https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/a" target="_blank">MDN</a>

1. target 属性（结合 iframe 理解）
   - `_self`：在当前页面打开（默认）
   - `_blank`：在空页面打开
   - `_partent`：父框架集或父窗口打开
   - `_top`：祖宗窗口中打开
   - framename：在指定的 `iframe` 标签中打开
  
2. href 可以输入的内容
   - 使用 http 协议或 https 协议：`http://qq.com`  `https://qq.com`
   - 使用和当前页面相同的协议，无协议绝对地址: `//qq.com` (直接预览为 file 协议，使用 http-server: `npm i -g http-server`, `http-server` or `hs` 可直接执行，`http-server -c-1` 不要缓存)
   - 相对路径 (跳转到 /xxx.html)：`xxx.html`
   - 锚点（不会发送请求，页面内跳转，别的都要发起请求）：`#xxx`
   - 查询参数：`?name=qqqq`
   - 伪协议：`javascript:alert(1);` （执行 js 代码）；`javascript:;`(需要写a标签，但是点击之后不需要跳转。直接写 `href="#"` 会跳到页面顶部，`href=""` 回刷新当前页，不写 href，a 标签变 span)

   错误的：
   - `qq.com`：为相对位置，当文件打开

3. download 属性：将链接变成可下载的。
   > 另一方法：HTTP 响应头中设置 `Content-Type: application/octet-stream`，浏览器默认处理字节流的方式是下载

4. title 属性：规定额外信息，通常鼠标移到元素上显示这些信息。

## form 标签

跳转页面（HTTP POST 请求）

更多属性见 >> <a href="https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/form" target="_blank">MDN</a>

1. 和 a 标签的区别是发起请求的类型不同。action 类似于 href，target 和 a标签的一样
2. GET 请求：表单信息变成查询参数，会出现在 url 里，请求头中无法有第四部分（Form Data）；
   POST 请求：表单信息作为 HTTP 请求的第四部分提交到服务器，如果要写查询参数，可直接写在 action 属性中， `action="users?age=33"`
3. method="post"，默认为 get，自己改。
4. 请求头中 `Content-Type: application/x-www-form-urlencoded (key=value&key=value`，form 中 input 的 name 为 key)
5. 提交的内容在 Form Data 中，HTTP 不靠谱，密码为明文。英文之外的都会转义，utf-8 变长编码，英文1个字节，中文3个字节，每个字节前有 `%`
6. 如果 form 表单里没有提交按钮（input 标签，type="submit"），则无法提交
7. 如果1个 `<form>` 里面只有一个按钮(`<button>`)，会自动升级为提交按钮，但如果 button 标签写了 `type="button"`，表单则无法提交

## input

- input / button 区别：是否为「空标签」

<a href="https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/Input" target="_blank">input 的更多属性>></a>
<a href="https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/button" target="_blank">button 的更多属性>></a>

| type | 说明 |
|---------------|------|
| button | 定义可点击按钮（多数情况下，用于通过 JavaScript 启动脚本）。<br>和 button 标签在 form 表单中有区别，无法自动升级为提交按钮，看起来和 type="submit" 的 input 一样 |
| checkbox | 定义复选框。<br>（1） id="xxx" `<label for="xxx">你是白痴吗</label>`<br>（2） 用label 包 input ，这样就不用 id 关联，老司机推荐<br>`<label for="xxx">你是白痴吗<input type="checkbox" name="user"></label>`<br>（3） 在 form 中，checked: inputName=on, !cheked: 无内容<br>（4）多选，name 相同，value 不同，勾选的会被提交。如：<br>name="animal", value=["panda", "dog", "fish"] // 选了前两个，Form Data: animal=panda&animal=fish |
| file | 定义输入字段和 "浏览"按钮，供文件上传。 |
| hidden | 定义隐藏的输入字段。https://blog.csdn.net/kuangruike/article/details/52127450 |
| image | 定义图像形式的提交按钮。 |
| password | 定义密码字段。该字段中的字符被掩码。只是在 html 看不见，实际上还是明文的 |
| radio | 定义单选按钮。similar to checkbox |
| reset | 定义重置按钮。重置按钮会清除表单中的所有数据。 |
| submit | 定义提交按钮。提交按钮会把表单数据发送到服务器。 |
| text | 定义单行的输入字段，用户可在其中输入文本。默认宽度为 20 个字符。 |
| HTML5 | 新类型，不支持的浏览器显示为常规的文本域 |
| email | 要有 "@"，并且 "@" 前后都要有字符，且不能为中文（别的没试） |
| url | 粗糙测试，要有 http:// or https:// |
| number | 能够设定对所接受的数字的限定 min="1" max="10" |
| range | 滑动条，min max 设定范围，value 默认值，step 间隔 |
| Date pickers (date, month, week, time, datetime, datetime-local) | date - 选取日、月、年<br>month - 选取月、年<br>week - 选取周和年<br>time - 选取时间（小时和分钟）<br>datetime - 选取时间、日、月、年（UTC 时间）<br>datetime-local - 选取时间、日、月、年（本地时间） |
| search | 右边多个叉叉，点了清空内容|
| color | 只有 Opera 支持，应该没有卵用 |

## select 标签

下拉选择

```
<select name="group" multiple> // multiple 可多选
    <option value="">-</option>
    <option value="1">item1</option>
    <option value="2">item2</option>
    <option value="3" disabled>item3</option> // disabled 不可选
    <option value="4" selected>item4</option> // selected 默认选中
</select>

// 在 form 中提交格式为 selectName=selectedOptionValue
```

## textarea 标签

输入多行内容

| 需求 | 操作 |
|---------------|------|
| 大小不可改变 | CSS  resize: none; |
| 设置大小 | (CSS  width/height) or (col/rows  不准，一般不用) |

## table 标签示例
用于展示数据

属性见：https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/table

1. HTML 规定 `<table>` 中只可以有3个元素：`<thead> <tbody> <tfoot>`
2. tbody 标签不写，浏览器会自动补上
3. 表格边框的间隙默认有，不要则设置 table { border-collapse: collapse; }

```
<!-- table row 行 tr -->
<!-- table data 数据 td -->
<!-- table header 表头的标题 -->

<table border=1>
        <!-- colgroup 里面有元素就是有标签的元素 -->
    <colgroup>
        <!-- col 指定列的宽度 -->
        <!-- col 有 bgcolor 属性，设置改列的背景色 -->
        <col width=100>
        <col bgcolor=red width=200>
        <col width=100>
        <col width=70>
    </colgroup>
    <thead>
            <tr>
                <th>项目</th><th>姓名</th><th>性别</th><th>年龄</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <th></th><td>如花</td><td>1</td><td>90</td>
            </tr>
            <tr>
                <th></th><td>狗蛋</td><td>2</td><td>92</td>
            </tr>
            <tr>
                <th>平均分</th><td></td><td></td><td></td>
            </tr>
        </tbody>
        <tfoot>
            <tr>
                <th>总分</th><td></td><td></td><td></td>
            </tr>
        </tfoot>
</table>
```

<a href="http://www.w3school.com.cn/html5/html5_reference.asp" title="HTML 5 参考手册">HTML5 标签</a>