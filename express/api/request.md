# Request

req对象代表了http请求。它有好多属性，如：query strign, parameters, body, HTTP headers 等。一般都使用req.你也可以在中间件里使用你喜欢的变量名。

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
### req.path
### req.protocol
### req.query
### req.route
### req.secure
### req.signedCookies
### req.stale
### req.subdomains
### req.xhr
## 方法

### req.accepts()

### req.acceptsCharsets()

### req.acceptsEncodings()

### req.acceptsLanguage()

### req.get()

### req.is()

### req.param()

### req.range()