# HTTP 访问控制（CORS）

[TOC]

在网页上的请求与该网页的协议、域、端口有一个不同，则该请求就是跨域请求。

## 什么情况下需要cors

- 前文提到的由 [`XMLHttpRequest`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest) 或 [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) 发起的跨域 HTTP 请求。
- Web 字体 (CSS 中通过` @font-face `使用跨域字体资源), [因此，网站就可以发布 TrueType 字体资源，并只允许已授权网站进行跨站调用](http://www.webfonts.info/wiki/index.php?title=%40font-face_support_in_Firefox)。
- [WebGL 贴图](https://developer.mozilla.org/zh-CN/docs/Web/API/WebGL_API/Tutorial/Using_textures_in_WebGL)
- 使用 `drawImage` 将 Images/video 画面绘制到 canvas
- 样式表（使用 [CSSOM](https://developer.mozilla.org/en-US/docs/Web/CSS/CSSOM_View)）

(这是从文档里复制来的。我不知道为什么。)

## 功能概述

若执行有副作用的http请求（如：非get方法，有特殊MIME类型的post.）时，浏览器先使用`options`方法请求预检结果。服务器接到options请求后根据cookie/http论证相关数据验证是否通过预检。

## 若干访问控制场景

### 简单请求

简单请求过程很简单。就是发送请求，得到回馈数据。

简单请求的范围。

1. GET/HEAD/POST
2. 头部里只能有`Accpt / Accept-Language / Content-Language / Content-Type / DPR / Downlink / Save-Date / Viewport-Width / Width`
3. Content-Type的值只有是以下三者之一。`text/plain  multipart/form-data  application/x-www-form-urlencoded`
4. hxr上没有注册任何事件监听器。
5. 请求中没有使用`ReadableStream`对象。

### 预检请求

预检请求使用`OPTIONS`方法。由浏览器发起。编程者不用处理。用来获服务器是否允许该请求，避免跨域请求对服务器造成未预期的影响。不是简单请求的请求都会先使用预检请求，再使用相应请求。

### 附带身份凭证的请求

若为xhr对象设置`withCredentials=true`。则可以把cookie传给服务器。

若服务器返回信息的头部`Access-Control-Allow-Credentials: true`，则浏览器不会拦截得到的数据。否则浏览器会拦截得到的数据。

当xhr对象使用附带身份凭证的请求时，若服务器设置`Access-Control-Allow-Origin: *`，则请求失败。若服务器设置`Access-Control-Allow-Origin: <http://foo.example>`（相应发起请求的域名），则请求会成功。

响应头部中设置`Set-Cookie`，会尝试设置cookie。若操作失败，则抛出异常。

## http响应头部字段

`Access-Control-Allow-Origin: <origin> | * ` 允许访问该资源的外域URI。

`Access-Control-Expose-Headers: <fieldname>` 允许浏览器访问的头字段的白名单。

`Access-Control-Max-Age: <delta-seconds>` 设置预检请求能缓存多久。（单位为秒）

`Access-Control-Allow-Credentials` 是否允许读取response的内容。

`Access-Control-Allow-Methods: <method>[, <method>]` 用于预检请求的响应，指明 实际请求所允许的http方法。

`Access-Control-Allow-Headers: <field-name>[, <field-name>]` 用于预检请求的响应，指明实际请求中允许携带的头部字段。

## http请求头部字段

`Origin: <origin>` 指明预检请求或实际请求的源站。

`Access-Control-Request-Method: <method>` 用于预检请求，指明实际请求时使用的请求方法。

`Access-Control-Request-Headers: <fieldname>[, <fieldname>]` 用于预检请求，指明实际请求时请求头部会使用哪些字段。

## 规范

## 浏览器兼容性

## 参见

