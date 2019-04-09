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