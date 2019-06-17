# Router

[TOC]

router对象是一个隔离的中间件。你可以认为它是一个迷你中间件，只能执行路由功能。express有内置的app router.

你可以把为router作为app.use()的参数。也可以在路由上使用另一个路由。使用router.use()。

router是express对象的一个顶级方法。使用这个方法可以新建一个路由对象。

一旦你使用了路由对象，你就可以使用http方法。下面是例子。

```
// invoked for any requests passed to this router
router.use(function(req, res, next) {
  // .. some logic here .. like any other middleware
  next();
});
// will handle any request that ends in /events
// depends on where the router is "use()'d"
router.get('/events', function(req, res, next) {
  // ..
});
```

你可以为应用使用路由分成多个部分路由。

```
app.use('/calendar', router)
```

## Methods

### router.all(path, [callback, ...] callback)

这个方法就像router.METHOD()。除了 可以匹配所有http方法。

这个方法对于全局逻辑、部分路由、武断匹配非常有用。如下面这个路由，它被定义在其它路由之前。这样就会匹配所有路由，然后执行验证、加载用户工作。在最后一个方法后使得next()可以执行其他匹配路由的中间件方法。

```
router.all('*', requireAuthentication, loadUser)
<=>
router.all('*', requireAuthentication)
router.all('*', loadUser)
```

下面的例子是全局白名单功能。这严格匹配路径前缀'/api'后执行。

`router.all('/api/*', requireAuthentication)`

### router.METHOD(path, [callback, ...] callback)

这个方法在express里提供路由方法，method是任一种http请求方法。（GET/PUT/POST/DELETE...）但是router.METHOD使用小写。即：router.get()/router.post()/router.put()

你可以使用多个中间件方法。每一个中间件文件使用next('route')调用下一个中间件方法。

```
// 
router.get('/', function(req, res){
  res.send('hello world');
});
// 使用正则表达式
router.get(/^\/commits\/(\w+)(?:\.\.(\w+))?$/, function(req, res){
  var from = req.params[0];
  var to = req.params[1] || 'HEAD';
  res.send('commit range ' + from + '..' + to);
});
```

### router.param(name, callback)
> router.param()不接受数组形式的路由参数。

当路由有name时执行callback.虽然name中一个技术选项，但v4.11.0后不反对使用。

回调函数有5个参数，分别如下。

- req
- res
- next
- value // name的值
- name // name的名称

正在是一个例子，预计路由在若有:user则自动执行回调方法。

```
router.param('user', function (req, res, next, value, name) {
	User.find(value, function (err, user) {
		if (err) {
			next(err)
		} else {
			if (user) {
				req.user = user
				next()
			} else {
				next(new Error('failed to load user'))
			}
		}
	})
})
```

这个方法只作用于当前router。不会作用于其后代router.只作用于request-response cycle中。下面是一个例子。

```
router.param('id', (req, res, next, value, name) => {
	console.log('called only once')
	next()
})
router.get('/user/:id', (req, res, next) => {
	console.log('one')
	next()
})
router.get('/user/:id', (req, res, next) => {
	console.log('two')
	res.end()
})
// 若执行 /user/42
// called only once
// one
// two
```

> 下列内容在v4.11.0+支持。

这个方法第一个参数是可以被捕获的路由参数。第二个参数可以是任意js对象，但必需返回一个中间件执行。

下面是一个例子。router.param(name, callback)被修改为router.param(name, accessId)。callback可以是name/number。

```
var express = require('express');
var app = express();
var router = express.Router();
// customizing the behavior of router.param()
router.param(function(param, option) {
  return function (req, res, next, val) {
    if (val == option) {
      next();
    }
    else {
      res.sendStatus(403);
    }
  }
});
// using the customized router.param()
router.param('id', 1337);

// route to trigger the capture
router.get('/user/:id', function (req, res) {
  res.send('OK');
});
app.use(router);
app.listen(3000, function () {
  console.log('Ready');
});
```

下面的例子保持router.param(name, callback)不变，但是替换了回调函数。使用自定义方法验证用户。

```
router.param(function(param, validator) {
  return function (req, res, next, val) {
    if (validator(val)) {
      next();
    }
    else {
      res.sendStatus(403);
    }
  }
});
router.param('id', function (candidate) {
  return !isNaN(parseFloat(candidate)) && isFinite(candidate);
});
```

### router.route(path)

可以为同一个路径使用多种不同的http请求方式。

```
var router = express.Router();
router.param('user_id', function(req, res, next, id) {
  // sample user, would actually fetch from DB, etc...
  req.user = {
    id: id,
    name: 'TJ'
  };
  next();
});
router.route('/users/:user_id')
.all(function(req, res, next) {
  // runs for all HTTP verbs first
  // think of it as route specific middleware!
  next();
})
.get(function(req, res, next) {
  res.json(req.user);
})
.put(function(req, res, next) {
  // just an example of maybe updating the user
  req.user.name = req.params.name;
  // save user ... etc
  res.json(req.user);
})
.post(function(req, res, next) {
  next(new Error('not implemented'));
})
.delete(function(req, res, next) {
  next(new Error('not implemented'));
});
```

> 当你使用router.route()时中间件的执行顺序依据路由被创建时的顺序。（不是被添加的顺序）因此你就需要考虑路由创建的顺序。

### router.use([path], [function, ...] function)

为path绑定一个或多个中间件方法。path默认为'/'。有点类似app.use().

```
var express = require('express');
var app = express();
var router = express.Router();
// simple logger for this router's requests
// all requests to this router will first hit this middleware
router.use(function(req, res, next) {
  console.log('%s %s %s', req.method, req.url, req.path);
  next();
});
// this will only be invoked if the path starts with /bar from the mount point
router.use('/bar', function(req, res, next) {
  // ... maybe some additional /bar logging ...
  next();
});
// always invoked
router.use(function(req, res, next) {
  res.send('Hello World');
});
app.use('/foo', router); // /foo/bar
app.listen(3000);
```

定义router.use()和中间件的顺序非常重要。他们的执行顺序就是定义时的顺序。因此先定义中间件。下面是二个例子。  
使用router.use()方法为同一个路径使用多次时，其多次绑定的中间件会根据定义时的顺序执行。

```
// 这样写不会记录请求静态文件。
var logger = require('morgan');
router.use(logger());
router.use(express.static(__dirname + '/public'));
router.use(function(req, res){
  res.send('Hello');
});
// 会记录表示静态文件。
router.use(express.static(__dirname + '/public'))
router.use(logger())
router.use((req, res) => {
	res.send('string')
})
```

下面的例子定义了多个静态文件目录。会优先使用'./public'

```
app.use(express.static(`__dirname${/public}`))
app.use(express.static(`__dirname${/files}`))
app.use(express.static(`__dirname${/uploads}`))
```

