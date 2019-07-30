# HTTP 访问控制（CORS）

## 若干访问控制场景

### 简单请求

条件：

1. get/post/head
2. 不能在这些头部字段以外的字段。（Accpt, Accept-Language, Content-Language, Content-Type）
3. Content-Type的值只能是此三者之一（text/plain, multipart/form-data application/x-www-form-rulencoded）
4. 请求中的任意xhr对象没有注册任何事件监听器。（若要xxx可以使用XMLHttpRequest.upload属性访问）
5. 请求中没有使用ReadableStream对象。

### 预检请求

