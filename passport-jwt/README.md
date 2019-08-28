# Introduction

## passport-jwt

使用jsonwebtoken验证用户。

### 配置策略

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

options是一个包含了如何从req中取出token/验证token的各个设置的对象.

| property                | type     | describe                                                     | default        |
| ----------------------- | -------- | ------------------------------------------------------------ | -------------- |
|                         |          |                                                              |                |
|                         |          |                                                              |                |
| jwtFromRequest          | function | 接收req返回jwt / null。必选项。                              |                |
| issuer                  |          | 若设置了issuer(iss)则验证。                                  |                |
| audience                |          |                                                              |                |
| algorithms              | Array    | 算法名称的列表。                                             |                |
| ignoreExpiration        | Boolean  | true: 不验证token里的expireIn(exp)。                         |                |
| passReqToCallback       | Boolean  | true: 把req做为验证回调函数的参数之一。如：`verify(req, jwt_payload, done_callback)` |                |
| jsonWebTokenOptions     | String   | 指定passport-jwt的验证者。如：`verify(jwt_payload, done)`    | 'jsonwebtoken' |
| jwt_payload             | Object   | 包含解码后jwt payload                                        |                |
| done                    | Function | 当passport出错时执行。第一个参数是error.如：`done(error, user, info)` |                |
| tokenBodyField          | String   |                                                              |                |
| auth_token              | String   |                                                              |                |
| tokenQueryParameterName | String   |                                                              |                |

### 从req中取出jwt

在options里的jwtFromRequest指定的方法就是从req中取出jwt的方法。它接收req为参数返回解码后的jwt或null.

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

