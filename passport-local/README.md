# Introduction

passport-local(pl)是作为passport的一个策略的插件，用来验证username/passport.

## install

```
npm i passport-local
```

## usage

### 把pl配置在passport里。

pl是验证username/passport的策略。它需要一个有验证回调函数。这个回调函数接收username/password/done(这是执行完这个回调函数后的回调函数。done（err, user）)

```
passport.use(new LocalStrategy((username, password, done) => {
	User.findOne({username: username}, (err, user) => {
		if (err) {return done(err)}
		if (!user) {return done(null, false)}
		if (!user.verifyPassword(password)) {return done(null, false)}
		return done(null, user)
	})
}))
```

### 验证请求

```
app.post('/login', passport.authenticate('local', {failureRedirect: '/login'}), (req, res) => {
	res.redirect('/')
})
```

