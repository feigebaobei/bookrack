# HTTP Cookie

[TOC]

服务端给浏览器后,存在浏览器的数据.在以后的请求中把cookie放在请求头里，传给后端。这样就可以表明。连接状态。把无状态的http协议变的有状态。  
主要用于三个方面：  

- 会话状态管理。（用户登录状态、购物车，游戏分数……）  
- 个性化设置。（用户自定义设置，主题……）  
- 浏览器行为跟踪（分析用户行为）  

## 创建cookie
服务器在响应头里设置`Set-Cookie`字段。其值为`key=value`格式。

`Set-Cookie: yummy_cookie=choco; tasty_cookie=strawberry`

### 会话期cookie

若不设置`Expires``Max-Age`就是会话期cookie。一般在关闭网页后被删除。特殊时：因浏览器有会话恢复功能，所以会话期cookie被保留下来，像没有关闭网页一样。

### 持久性cookie

设置`Expires``Max-Age`后cookie一直保留到指定时间。

### Cookie的Secure和HttpOnly标记

`Secure`：安全。只有使用https协议时才会在请求头里设置Secure。

`HttpOnly`:限制。服务器给客户端的响应头里的`Set-Cookie`的值里使用这个属性（这个属性不需要值），则客户端无法操作该cookie。

### Cookie的作用域

`Domain``Path`属性控制cookie的作用域。

`Domain`控制在哪个主域名下使用。默认为当前主域名。`Path`表示在哪个路径下使用。对其子路径也有效。

### SameSite

是否允许跨站发送cookie。可以防止csrf攻击。不是所有浏览器支持。

### js通过document.cookies访问cookie

`document.cookie`可以得到非HttpOnly标记的cookie。

## 安全

### 会话劫持和xss
设置HttpOnly可以缓解此类攻击。

### 跨站请求伪造

1. 对用户输入进行过滤来阻止xss.
2. 任何敏感操作都需要确认。
3. 用于敏感信息的cookie只能拥有较短生命周期。

## 追踪和隐私
### 第三方cookie

第一方cookie是与当前页面域相同的cookie。否则是第三方cookie。

现在技术上没方法禁止追踪cookie。只能人工禁止。

### 禁止追踪Do-Not-Track
### 欧盟Cookie指令
### 僵尸cookie和删不掉的cookie

## 一些方法

- Set-Cookie
- Cookie
- document.cookie
- navigator.cookieEnabled // 得到当前页面是否启用cookie.

