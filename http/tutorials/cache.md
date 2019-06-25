# HTTP 缓存

使用重用资源的方法来提高网站与应用的性能。

## 各种类型的缓存

当web缓存发现请求的资源已经被存储，它会拦截请求，返回该资源的拷贝，而不会去服务器下载。缓存是提高性能的方法。大致会为以下2类。

### （私有）浏览器缓存

缓存在浏览器的内容。浏览过的文档提供向后、向前导航。只能对该浏览器使用。

### （共享）代理缓存

缓存的内容在代理上。可以被多个用户使用。可以减少网络拥堵。

## 缓存操作的目标

浏览器只能缓存get请求。

- 成功响应。200，ok
- 永久重定向301.
- 错误响应404.
- 不完全响应206
- 除get外，如果匹配到作为一个已被定义的cache键名的响应。

## 缓存控制

### Cache-control头

请求头，响应头都有这个属性。它指明缓存策略。

#### 禁止进行缓存

`Cache-Control:no-store`// 不缓存

#### 强制确认缓存

`Cache-Control: no-cache`//

#### 私有缓存和公共缓存

`Cache-Control: private`

`Cache-Control: public`

#### 缓存过期机制

`Cache-Control: max-age=31536000`

#### 缓存验证确认

`Cache-Control: must-revalidate`

###  Prama头

## 新鲜度

![Show how a proxy cache acts when a doc is not cache, in the cache and fresh, in the cache and stale.](https://mdn.mozillademos.org/files/13771/HTTPStaleness.png)

上图描述了缓存新鲜度的事情。第一次请求是没缓存的，代理向服务器请求数据，再给客户端。第二次以后请求会判断当前资源是否已过期（已经不新鲜）（根据max-age判断）。若未过期，则返回缓存。若已经过期，则代理向服务器发送一个请求头包含`if-None-Match`的请求。由服务器判断是否新鲜。若新鲜，则返回304.(没有实例信息，这样会减小带宽。)若不新鲜，则返回指定资源。

计算失效时间的公式：

`expirationTime = responseTime + freshnessLifetime - currentAge`

responseTime: 浏览器得到资源的时间。

### 加速资源

## 缓存验证

### ETags

## 带Vary头的响应

