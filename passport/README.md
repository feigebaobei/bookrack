# Introduction

[TOC]

这是兼容express框架的只做验证功能的中间件。它需要strategy（策略）扩展。它不挂载在路由上，不篡改数据库schema.让开发者最大化扩展主程序。它的api很简单：提供一个请求去验证，passport提供一个验证成功或失败的钩子函数。

## install

`npm i passport`

## usage

### strategies

passport使用strategy验证请求。strategy可以验证username/password也可以使用OAuth也可以使用OpenID(开放验证).

在验证前一定要配置。

```
passport.use(new LocalStrategy(
  function(username, password, done) {
    User.findOne({ username: username }, function (err, user) {
      if (err) { return done(err); }
      if (!user) { return done(null, false); }
      if (!user.verifyPassword(password)) { return done(null, false); }
      return done(null, user);
    });
  }
));
```

现在已经有300+个strategy.

### sessions

passport将维持持续登录session。为了让持续登录session正常工作,所以验证用户时必须序列化这个session，并且在后续请求中反序列化。

passport不会影响你保存用户、记录数据。你需要执行必要的序列化方法和反序列化方法。下面是一种常见情况，它很简单的序列化user id，并在反序列化是找到这个id.

```
passport.serializeUser((user, done) => {done(null, user.id)})
passport.deserializeUser((id, done) => {User.findById(id, (err, user) => {done(err, user)})})
```

### middleware

在express/connect框架的应用中使用passport需要先配置（`passport.initialize()`）。若你的应用使用持续登录session，则推荐但不必要使用`passport.session()`

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

passport提供了一个在路由里验证请求的方法`authenticate()`

```
app.post('/login', passport.authenticate('local', {failureRedirect: '/login'}), (req, res) => {res.redirect('/')})
```

## strategies

passport有好多策略。下面是一些常用的策略。

| 策略                   | 协议                      | 开发者 |
| ---------------------- | ------------------------- | ------ |
| Local                  | HTML form                 |        |
| OpenID                 | OpenID                    |        |
| BrowserID              | BrowserID                 |        |
| Facebook               | OAuth 2.0                 |        |
| Google                 | OpenID                    |        |
| Google                 | OAuth/OAuth 2.0           |        |
| Twitter                | OAuth                     |        |
| Azure Active Directory | OAuth 2.0 / OpenID / SAML |        |

## examples

## related modules
