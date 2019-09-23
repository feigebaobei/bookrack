# Introduction

## passport-jwt

使用jsonwebtoken验证用户。

可以使用jsonwebtoken验证终端，这样可以不使用session.

## install

```
npm i passport-jwt
```

## usage

### configure strategy

```
new JwtStrategy(options, verify)
```

options是一个包含了如何从req中取出token/验证token的各个设置的对象.

| property            | type            | required                                        | describe                                                     | default        | example            |
| ------------------- | --------------- | ----------------------------------------------- | ------------------------------------------------------------ | -------------- | ------------------ |
| secretOrKey         | string / buffer | secretOrKey/secretOrKeyProvider必须有其中一个。 | 一个包含secret的string/buffer。                              |                |                    |
| secretOrKeyProvider | function        | secretOrKey/secretOrKeyProvider必须有其中一个。 | 它是一个回调函数。`secretOrKeyProvider(request, rawJwtToken, done)` |                |                    |
| jwtFromRequest      | function        | true                                            | 接收req作为参数，返回jwt(string) / null。                    |                |                    |
| issuer              |                 | -                                               | 若设置了issuer(iss)则验证。                                  |                |                    |
| audience            |                 | -                                               | 若设置了audience(aud)则验证。                                |                |                    |
| algorithms          | Array           |                                                 | 算法名称的列表。                                             |                | ['HS256', 'HS384'] |
| ignoreExpiration    | Boolean         |                                                 | true: 不验证token里的expireIn(exp)。即：是否验证过期。       |                |                    |
| passReqToCallback   | Boolean         |                                                 | true: 把req做为验证回调函数的参数之一。如：`verify(req, jwt_payload, done_callback)` |                |                    |
| jsonWebTokenOptions | String          |                                                 | 指定passport-jwt的验证者。如：`verify(jwt_payload, done)`    | 'jsonwebtoken' |                    |

verify `verify(jwt_payload, done)`

jwt_payload 是一个包含解码的JWT的payload.

done 回调函数。`done(error, user, info)`

    let jwtStrategy = require('passport-jwt').Strategy,
      ExtractJwt = require('passport-jwt').ExtractJwt
    let options = {
      jwtFromRequest = ExtractJwt.fromAuthHeaderAsBearerToken()
      secretOrKey = 'secret',
      issuer = 'accounts.examplesoft.com',
      audience = 'yoursite.net'
    }
    passport.use(new jwtStrategy(options, (jwt_payload, done) => {
      User.findOne({id: jwt_payload.sub}, (err, user) => {
        if (err) {return done(err, false)}
        if (user) {
          return done(null, user)
        } else {
          return done(null, false)
        }
      })
    }))

### 从req中取出jwt

有几个方法可以从request里取出jwt。在options里的`jwtFromRequest`指定的方法就是从req中取出jwt的方法。它接收req为参数返回解码后的jwt或null.

passport-jwt.ExtractJwt有6个提取jwt的方法。它们都返回一个使用指定的参数配置的提取者。

| function                                       | params | describe                                          |      |
| ---------------------------------------------- | ------ | ------------------------------------------------- | ---- |
| fromHeader(header_name)                        |        | 返回一个从haeder里取jwt的提交者。                 |      |
| fromBodyField(field_name)                      |        | 。。。。为使用这个方法你必须配置body解析器。      |      |
| fromUrlQueryParameter(param_name)              |        |                                                   |      |
| formAuthHeaderWithSchema(auth_schema)          |        | 从验证头中提取。接收一个auth_schema作为匹配模型。 |      |
| formAuthHeadersAsBearerToken()                 |        | 从验证头中提取，提取模型默认是`'bearer'`.         |      |
| fromExtractors([Array of extractor functions]) |        | 依次尝试返回提取者，直到正确返回。                |      |

### 编写自定义的提取者方法

```
var cookieExtractor = (req) => {
  var token = null
  if (req && req.cookies) {
    token = req.cookies['jwt']
  }
  return token
}
```

### authenticate requests

```
app.post('/profile', passport.authenticate('jwt', {session: false}), (req, res) => {
	res.send(req.user.profile)
})
```

### 在req中包含jwt
