---
title: HTTP
date: 2019-02-24 13:36:47
tags:
 - 请求
 - 响应
---

## HTTP 请求

### 请求示例

命令 

`curl -s -v -H "aaa: bbb" -- "https://www.baidu.com"`

请求的内容为

```
GET / HTTP/1.1
Host: www.baidu.com
User-Agent: curl/7.54.0
Accept: */*
aaa: bbb
```

命令

`curl -X POST -d "1234567890" -s -v -H "Celeste: xxx" -- "https://www.baidu.com"`

请求的内容为

```
POST / HTTP/1.1
Host: www.baidu.com
User-Agent: curl/7.54.0
Accept: */*
Celeste: xxx
Content-Length: 10
Content-Type: application/x-www-form-urlencoded

1234567890
```

### 请求的格式

```
1 动词 路径 协议/版本
2 Key1: value1
2 Key2: value2
2 Key3: value3
2 Content-Type: application/x-www-form-urlencoded
2 Host: www.baidu.com
2 User-Agent: curl/7.54.0
3 
4 要上传的数据
```

1. 请求最多包含四部分，最少包含三部分。（也就是说第四部分可以为空）
2. 第三部分永远都是一个回车（\n）
3. 动词有 GET POST PUT PATCH DELETE HEAD OPTIONS 等
4. 这里的路径包括「查询参数」，但不包括「锚点」
5. 如果你没有写路径，那么路径默认为 /
6. 第 2 部分中的 Content-Type 标注了第 4 部分的格式

### 用 Chrome 发请求

1. 打开 Network
2. 地址栏输入网址
3. 在 Network 点击，查看 request，点击「view source」
4. 如果有请求的第四部分，那么在 FormData 或 Payload 里面可以看到

## HTTP 响应

### 响应示例

```
HTTP/1.1 200 OK
Accept-Ranges: bytes
Cache-Control: private, no-cache, no-store, proxy-revalidate, no-transform
Connection: Keep-Alive
Content-Length: 2443
Content-Type: text/html
Date: Sun, 24 Feb 2019 05:55:32 GMT
Etag: "58860421-98b"
Last-Modified: Mon, 23 Jan 2017 13:24:49 GMT
Pragma: no-cache
Server: bfe/1.0.8.18
Set-Cookie: BDORZ=27315; max-age=86400; domain=.baidu.com; path=/

<!DOCTYPE html>
<!--STATUS OK--><html> <head> 省略
```

- GET 请求和 POST 请求对应的响应可以一样，也可以不一样
- 响应的第四部分可以很长很长很长

### 响应的格式

```
1 协议/版本号 状态码 状态解释
2 Key1: value1
2 Key2: value2
2 Content-Length: 17931
2 Content-Type: text/html
3
4 要下载的内容
```

- 状态码要背，是服务器对浏览器说的话
    - 1xx 不常用
    - 2xx 表示成功
    - 3xx 表示滚吧
    - 4xx 表示客户端出错
    - 5xx 表示服务器出错
- 状态解释没什么用
- 第 2 部分中的 Content-Type 标注了第 4 部分的格式
- 第 2 部分中的 Content-Type 遵循 MIME 规范

### 用 Chrome 查看响应

1. 打开 Network
2. 输入网址
3. 选中第一个响应
4. 查看 Response Headers，点击「view source」
5. 你会看到响应的前两部分
6. 查看 Response 或者 Preview，你会看到响应的第 4 部分