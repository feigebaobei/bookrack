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

请求头，响应头都有这个属性。它指明缓存策略。设置多个值时使用逗号分隔。

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

由于缓存只有有限的空间用于存储资源副本，所以缓存会定期将一些副本删除。这个过程叫缓存驱逐。

上图描述了缓存新鲜度的事情。第一次请求是没缓存的，代理向服务器请求数据，再给客户端。第二次以后请求会判断当前资源是否已过期（已经不新鲜）（根据max-age判断）。若未过期，则返回缓存。若已经过期，则代理向服务器发送一个请求头包含`if-None-Match`的请求。由服务器判断是否新鲜。若新鲜，则返回304.(没有实例信息，这样会减小带宽。)若不新鲜，则返回指定资源。

计算失效时间的公式：

`expirationTime = responseTime + freshnessLifetime - currentAge`

responseTime: 浏览器得到资源的时间。

### 加速资源

![img](https://mdn.mozillademos.org/files/13779/HTTPRevved.png)

任何防止缓存碰撞的标记如hash/时间戳。

## 缓存验证

当用户点击刷新按钮时开始缓存验证，若缓存的响应头里`Cache-control:must-revalidate`则进行验证，用户在浏览器里设置验证缓存时进行缓存验证。

若缓存文档过期后，需要进行缓存验证或重新获取资源。只有在服务器返回强验证或弱验证时才会进行验证。

响应头里的ETage是强验证。

响应头里的Last-Modified是弱验证（只能精确到一秒）。

| 第一次请求                                          | 后序请求                                    | 验证结果                                                     |
| --------------------------------------------------- | ------------------------------------------- | ------------------------------------------------------------ |
| 响应头里的`ETage`是强验证。                         | 在请求头里使用`If-None-Match`验证缓存。     | 200，返回正常结果。304，不返回body，浏览器使用本地缓存文件。 |
| 响应头里的Last-Modified是弱验证（只能精确到一秒）。 | 在请求头里使用`If-Modified-Since`验证缓存。 | 200，返回正常结果。304，不返回body，浏览器使用本地缓存文件。 |

## 带Vary头的响应

![The Vary header leads cache to use more HTTP headers as key for the cache.](https://mdn.mozillademos.org/files/13769/HTTPVary.png)

使用`Vary`验证是否使用缓存需要对比三个地方。当前请求的请求头里的`Vary`，设置缓存时的请求头里的`Vary`，缓存的响应头里的`Vary`三者都相等时使用缓存。一般使用`User-Agent`做为`Vary`值进行验证。可以方便按浏览器返回数据。如移动端数据与pc端数据分别对待。