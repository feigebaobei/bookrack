# Request

[TOC]

```
app.get('/usre/:id', (req, res) => {
	res.send('user ' + req.params.id)
})
```

req对象是node的加强版本结果。

## 属性

### req.app

在中间件中可以使用这个属性得到express应用的实例的属性。

如果你是像下面这样使用输出中间件，再使用require()得到它。你就可以在express实例中通过req.app得到express实例的属性。

```
// index.js
app.get('/viewdirectory', require('./mymiddleware.js'))
// mymiddleware.js
module.exports = function (req, res) {
  res.send('The views directory is ' + req.app.get('views'));
}
```

### req.baseUrl

路由被挂载的路径。它与app.mountpath差不多，区别在于app.mountpath返回一个路径（位）.

```
var greet = express.Router();
greet.get('/jp', function (req, res) {
  console.log(req.baseUrl); // /greet
  res.send('Konichiwa!');
});
app.use('/greet', greet); // load the router on '/greet'
```

若为多个路径绑定中间件，则req.baseUrl只返回当前请求的基本路径。

```
app.use(['/gre+t', '/hel{2}o'], greet); // load the router on '/gre+t' and '/hel{2}o'
```

当请求/greet/jq时，req.baseUrl返回/greet。当请求/hello/jq时，req.baseUrl返回/hello。

### req.body

包含从请求体里的kv形式的数据。荐存在，则默认值为undefined。

下面是一个例子，如何使用body-parsing中间件处理req.body里的数据。

```
var app = require('express')();
var bodyParser = require('body-parser');
var multer = require('multer'); // v1.0.5
var upload = multer(); // for parsing multipart/form-data
app.use(bodyParser.json()); // for parsing application/json
app.use(bodyParser.urlencoded({ extended: true })); // for parsing application/x-www-form-urlencoded
app.post('/profile', upload.array(), function (req, res, next) {
  console.log(req.body);
  res.json(req.body);
});
```

### req.cookies

当你使用[cookie-parser](<https://github.com/expressjs/cookie-parser>)中间件时，request会包含一个cookies对象，其默认值为{}。其包含cookies值。

```
// cookie name= top
req.cookies.name // top
```

若你使用了标记cookie，请使用req.signedCookies。

更多信息请查看[cookie-parser](<https://github.com/expressjs/cookie-parser>)

### req.fresh

表明请求是否新鲜，它与req.stale相反。

若请求头里cache-control没有no-cache指令，或以下任一情况，则为true.

- if-modified-since 被指定。并请求标头早于、等于修改的响应标头。
- if-none-match 是*
- 解析为if-none-match与etag不同。

`req.fresh // true`[fresh](<https://github.com/jshttp/fresh>)

### req.hostname

得到 http 头部的hostname.

### req.ip

得到请求地址的远程ip.

`req.ip // 127.0.0.1`

### req.ips

当可信息代理设置不为false时，其属性是一个包含明确ip地址属性的，在请求头里的x-forwarded-for里，否则为空数组。

若x-forwared-for为`client, proxy1, proxy2, proxy3`则会得到`["client", "proxy1", "proxy2", "proxy3"]`

### req.method
得到请求的方法。如GET,POST,PUT。

### req.originalUrl

> req.url已经不能使用了。

它会返回req.baseUrl/req.path/查询字符串。

```
app.use('/admin', function(req, res, next) {  // GET 'http://www.example.com/admin/new'
  console.log(req.originalUrl); // '/admin/new'
  console.log(req.baseUrl); // '/admin'
  console.log(req.path); // '/new'
  next();
});
```

### req.params

得到动态路由里的参数。使用对象包裹这些数据。key是动态路由的变量，value是对应的值。该对象的默认值是{}。

```
// GET /user/tj // /user/:name
console.dir(req.params.name)
// => 'tj'
```

若使用正则表达式，其会捕获成为数组，使用req.params[n]得到相应的值。n是第n个被捕获的路由参数。如：

```
// GET /file/javascripts/jquery.js
console.dir(req.params[0])
// => 'javascripts/jquery.js'
```

若你需要改变req.params需要使用app.param控制器。

> express在req.params里使用了decodeURIComponent.所以会自动decode.

### req.path

得到请求的路径。（没有hostname、没有query string）

### req.protocol

得到请求协议。现在只有http 和 https。

当信任协议不是false时，若header里有x-forwarded-proto属性，则取其值。

### req.query

使用对象包裹请求字符串。其默认值是{}.

```
// GET /search?q=tobi+ferret
console.dir(req.query.q)
// => 'tobi ferret'
// GET /shoes?order=desc&shoe[color]=blue&shoe[type]=converse
console.dir(req.query.order)
// => 'desc'
console.dir(req.query.shoe.color)
// => 'blue'
console.dir(req.query.shoe.type)
// => 'converse'
// GET /shoes?color[]=blue&color[]=black&color[]=red
console.dir(req.query.color)
// => ['blue', 'black', 'red']
```

### req.route

得到当前匹配的路由。

```
app.get('/user/:id?', function userIdHandler (req, res) {
  console.log(req.route)
  res.send('GET')
})
// 得到如下数据
{ path: '/user/:id?',
  stack:
   [ { handle: [Function: userIdHandler],
       name: 'userIdHandler',
       params: undefined,
       path: undefined,
       keys: [],
       regexp: /^\/?$/i,
       method: 'get' } ],
  methods: { get: true } }
```

### req.secure

得到是否是TLS connection.与`req.protocol === 'https'` 相等。

### req.signedCookies

标记cookie会在另一个对象里，与未标记cookie分开对待。标记cookie不会被加密和隐藏。因为它是私密的。其默认值是{}。

```
// Cookie: user=tobi.CP7AWaXDfAKIRfH49dQzKJx7sKzzSoPq7/AcBBRVwlI3
console.dir(req.signedCookies.user)
// => 'tobi'
```

更多信息请查看[cookie-parser](<https://github.com/expressjs/cookie-parser>)

### req.stale

得到请求是否是陈旧的。与req.fresh相反。

### req.subdomains

以数组形式得到子域名。

```
// Host: "tobi.ferrets.example.com"
console.dir(req.subdomains)
// => ['ferrets', 'tobi']
```

### req.xhr
返回请求的x-request-with是否为xmlhttprequest。

## 方法

### req.accepts()

检测内容类型是否在请求头的Accept字段值里。若不存在则返回false。此时会返回406，not Acceptable.

其值可以是多种类型。可以是string/array.方法会返回一个最好的匹配结果。

```
// Accept: text/html
req.accepts('html')
// => "html"
// Accept: text/*, application/json
req.accepts('html')
// => "html"
req.accepts('text/html')
// => "text/html"
req.accepts(['json', 'text'])
// => "json"
req.accepts('application/json')
// => "application/json"
// Accept: text/*, application/json
req.accepts('image/png')
req.accepts('png')
// => undefined
// Accept: text/*;q=.5, application/json
req.accepts(['html', 'json'])
// => "json"
```

更多信息请查看[accepts](<https://github.com/expressjs/accepts>)

### req.acceptsCharsets(charset[, ...])

基于请求头的Accept-Charset字段。返回是否接受指定字符集。

### req.acceptsEncodings(encoding[, ...])

基于请求头的Accept-Charset字段。返回是否接受指定编码。

### req.acceptsLanguage(lang[, ...])

基于请求头的Accept-Charset字段。返回是否接受指定语言。

### req.get(field)

得到请求头的特定字段。

```
req.get('Content-Type')
// => "text/plain"
req.get('content-type')
// => "text/plain"
req.get('Something')
// => undefined
```

别名是：req.header(field)

### req.is(type)

返回请求头里的Content-Type字段值是否与指定的类型一样。若没有请求体则返回null.其它情况返回false.

```
// With Content-Type: text/html; charset=utf-8
req.is('html')
// => 'html'
req.is('text/html')
// => 'text/html'
req.is('text/*')
// => 'text/*'
// When Content-Type is application/json
req.is('json')
// => 'json'
req.is('application/json')
// => 'application/json'
req.is('application/*')
// => 'application/*'
req.is('html')
// => false
```

更多信息主要查看[type-is](<https://github.com/jshttp/accepts>)

### req.param(name[, defaultValue])

> 可以使用在req.params / req.body / req.query

```
// ?name=tobi
req.param('name')
// => "tobi"
// POST name=tobi
req.param('name')
// => "tobi"
// /user/tobi for /user/:name
req.param('name')
// => "tobi"
```

它们的执行顺序是req.params / req.body / req.query

当没有任何请求对象时，你可以使用你测试的默认值。

### req.range(size[, options])

size是请求资源的最大值。

options是一个对象，可以包含以下属性。

| 属性    | 类型    | 说明                                                         |
| ------- | ------- | ------------------------------------------------------------ |
| combine | Boolean | 若邻近的并重复的range会成功combine。默认为false.当为true时， |

