---
title: 数据_Cookie_Session_LocalStorage_Cache-Control
date: 2019-04-05 10:44:39
tags: 
- Cookie
---

# Cookie

## 特点

1. 服务器通过 Set-Cookie 响应头设置 Cookie
2. 浏览器得到 Cookie 之后，每次请求都要带上 Cookie
3. 服务器读取 Cookie 就知道登陆用户的信息

## 问题

1. 在 Chrome 上登陆得到了 Cookie，用 Safari 访问，Safari 会带上 Cookie 吗？

    答：不会，Cookie 是跟着浏览器的。

2. Cookie 存在哪？

    答：Windows 存在 C 盘的一个文件里，其他系统存在硬盘的某个文件中，你找不到，系统不让你改。

3. Cookie 可以作假吗？

    答：可以。Chrome 调试工具里就可以改。设置 HttpOnly 后，将不能用 JS 修改该 Cookie。

4. Cookie 的有效期是多久？

    答：20分钟左右，由浏览器决定。

## LocalStorage

session 是服务器上的 hash 表。
session 占内存，cookie 不占内存。
localStorage 是浏览器上的 hash 表。

localStorage.setItem(‘object’, { name: ‘obj’ }) 
在存储时会转化为字符串，而对象 toString 都会变成 “[object Object]”

正确：
localStorage.setItem(‘jsonString’, JSON.stringify({ name: ‘obj’ })) 
localStorage.getItem
localStorage.clear()

localStorage 持久化存储，存在 C 盘文件中（windows系统）

问题：

1. Cookie 和 Session 有什么关系？

    答：一般来说，Session 是基于 Cookie 来实现的，因为 SessionId（随机数）通过 Cookie 发给客户端。

2. Cookie 和 LocalStorage 最大的区别是什么？

    答：Cookie（4K） 会通过浏览器被带到服务器上去，HTTP 不会带上 LocalStorage（5Mb） 的值。

3. LocalStorage 和 SessionStorage 的区别？

    答：SessionStorage 在用户关闭页面（会话结束）后就失效。

4. Cookie 和 SessionStorage 的区别？

    答：都是在用户关闭页面后就失效，但是后台代码可以任意设置 Cookie 的过期时间。

## 不基于 Cookie 的 Session

- 通过查询参数和 LocalStorage 传递 sessionId

## 前端永远不要 读/写 Cookie，用 LocalStorage