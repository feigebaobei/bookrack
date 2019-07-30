# Introduction

[TOC]

passport是为node.js设计的与express框架兼容的验证中间件。

它是目的就是验证请求，它可以使用策略扩展。它不能挂载在路由上或数据模式（database schema）上。最大的灵活性，允许应用级设计。它的api很简单：你使用passport验证请求，passport会使用钩子函数在需要验证的地方，控制是否成功。

## install

`npm i passport`

## 用法
### 策略

passport使用策略的概念验证。策略可以验证username/password。也可以使用三方验证。（如facebook/twitter）或使用要OpenId同盟验证。

在验证前你需要配置相应的策略。

```
passport.use(new LocalStrategy(
	function (username, password, done) {
		User.findOne({username: username}, (err, user) => {
			if (err) {return done(err)}
			if (!user) {return done(null, false)}
			if (!user.verifyPassword(password)) {return done(null, false)}
			return done(null, user)
		})
	}
))
```

在[passportjs.org](<http://www.passportjs.org/>)有300+种策略。

### sessions

passport会维持login sessions。为了维持login sessions，验证开始时会序列化session。传给后续的请求时再反序列化。

passport不使用任何限制，你如何记录、保存user呢。你需要提供必要的serialization / deserialization 逻辑。在典型的应用中，你应该序列化user ID.在反序列化是使用该id找到user。

```
passport.serializaUser((user, done) => {
	done(null, user.id)
})
passport.deserializaUser((id, done) => {
	User.findById(id, (err, user) => {done(err, user)})
})
```

### middleware

在express框架里使用passport时使用`passport.initialize()`方法配置中间件。若你保留session逻辑需要使用passport.session()。

```
var app = express();
app.use(require('serve-static')(__dirname + '/../../public'));
app.use(require('cookie-parser')());
app.use(require('body-parser').urlencoded({ extended: true }));
app.use(require('express-session')({ secret: 'keyboard cat', resave: true, saveUninitialized: true }));
app.use(passport.initialize());
app.use(passport.session());
```



### authenticate requests

为路由添加验证时使用`passport.authenticate()`.

```
app.post('/login', passport.authenticate('local', {failureRedirect: '/login'}), (req, res) => {res.redirect('/')})
```



## 策略

passport的官网展示了300+策略。

## 搜索所有的策略

| 策略                   | 协议                      | 开发者       |
| ---------------------- | ------------------------- | ------------ |
| Local                  | HTML form                 | Jared Hanson |
| OpenID                 | OpenID                    | Jared Hanson |
| BrowserID              | BrowserID                 | Jared Hanson |
| Facebook               | OAuth 2.0                 | Jared Hanson |
| Google                 | OpenID                    | Jared Hanson |
| Google                 | OAuth / OAuth 2.0         | Jared Hanson |
| Twitter                | OAuth                     | Jared Hanson |
| Azure Active Directory | OAuth 2.0 / OpenID / SAML | Jared Hanson |



## 例子
## 测试
## 信用
## 支持者