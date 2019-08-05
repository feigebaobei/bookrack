# username&passwrod

现在大部分是使用username/ password验证登录的。为满足这个功能通常需要`passport-local`包。下面是一个例子。

## demo

安装依赖

```
npm i passport-local
```

配置

```
var passport = require('passport')
var LocalStrategy = require('passport-local').Stragety
passport.use(new LocalStrategy(
	(username, password, done) => {
    User.findOne({username: username}, (err, user) => {
			if (err) {return done(err)}
			if (!user) {
				return done(null, false, {message: 'string'})
			}
			if (!user.validPassword(password)) { // 
				return done(null, false, {message: 'string'})
			}
			return done(null, user)
    })
	}
))
```

在路由是使用验证

```
app.post('/login', passport.authenticate('local'))
```

前端的form

```
<form action="/login" method="post">
    <div>
        <label>Username:</label>
        <input type="text" name="username"/>
    </div>
    <div>
        <label>Password:</label>
        <input type="password" name="password"/>
    </div>
    <div>
        <input type="submit" value="Log In"/>
    </div>
</form>
```

验证的参数

```
passport.use(new LocalStrategy({
    usernameField: 'email',
    passwordField: 'passwd'
  },
  function(username, password, done) {
    // ...
  }
));
```

