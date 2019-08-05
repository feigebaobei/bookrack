# 官网

## configure

三个步骤完成passport验证配置。

1. 验证策略 （authentication strategies）
2. 应用中间件 （application middleware）
3. 会话（可选项）

### strategies

passport需要使用策略去验证请求。策略包括验证username/password，也使用委托验证（使用[OAuth](http://oauth.net/)），也可以使用联合验证（使用[OpenID](https://openid.net/)）

在使用passport验证请求前必须为passport配置策略。

策略与其配置需要使用`user()`方法提供（即使用user()配置使用哪种验证策略）。下面是的例子使用`LocalStrategy`(它是一种策略)去验证username/password。

```
var passport = require('passport')
  , LocalStrategy = require('passport-local').Strategy;
passport.use(new LocalStrategy(
  function(username, password, done) {
    User.findOne({ username: username }, function (err, user) {
      if (err) { return done(err); }
      if (!user) {
        return done(null, false, { message: 'Incorrect username.' });
      }
      if (!user.validPassword(password)) {
        return done(null, false, { message: 'Incorrect password.' });
      }
      return done(null, user);
    });
  }
));
```

new LocalStrategy()里的内容是passport-local的知识。

### verify callback

上面的例子提到一个重要概念——验证回调。策略需要一个验证回调函数处理验证是否通过。（上面的例子在验证回调函数时做了查找是用户的username/password）

当请求一个验证时，这个回调函数需要解析在请求中的这凭证。即把凭证做为回调函数的参数。此例中使用`username/password`，若通过验证则调用`done(null, user)`.user是通过验证的user.若没有通过验证，则调用`done(null, false, {message: ***})`,false表示验证失败。若出现意外则调用`done(err)`。

### middleware

在express/connect框架下使用passport。需要使用`passport.initialize()`初始化passport。若你还使用session维持登录，则需要必须使用`passport.session()`中间件。

````
var session = require('express-sessoin')
var bodyParser = require('body-parser')
app.use(express.static('public'))
app.use(session({secret: 'cats'}))
app.use(bodyParser.urlencoded({extended: false}))
app.use(passport.initialize())
app.use(passport.session())
````

先使用session()再使用passport.session()。这样可以保存登录session.

### sessions

在请求登录时验证凭证。若验证成功，则session被建立，并且把cookie设置为session id.

后续的每个请求不包含凭证，但保证是惟一cookie鉴定session.为了支持登录session.passport必须把serssion中的user实例serialize / deserialize (序列化、反序列化)。

```
passport.serializeUser(function(user, done) {
  done(null, user.id);
});

passport.deserializeUser(function(id, done) {
  User.findById(id, function(err, user) {
    done(err, user);
  });
});
```

