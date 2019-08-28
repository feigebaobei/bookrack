# Introduction

jsonwebtoken是用来处理（加密、验证、解析）jsonwebtoken字符串。

## usage

官方的文档中介绍了三个方法。

jwt.sign(payload, secretOrPrivateKey, [options, cb]) // 生成加密后jwt string.

jwt.verify(token, secretOrPrivateKey, [options, cb]) // 验证jwt。

jwt.decode(token[, options]) // 解码jwt

### jwt.sign(payload, secretOrPrivateKey, [options, cb])

若使用cb,则它接受的参数是jwt的错误对象和jwt。（请查看本文的error对象部分）cb是异步的。

若不使用cb，则是同步的。返回一个jwt string.

若payload不是buffer 、string 对象，则强制使用JSON.stringify。

payload是buffer / string时不会检查json的有效性。

只有payload是文字对象时才设置`exp`等声明。

secretOrPrivateKey 是一个string / buffer / object 用于HMAC算法或PEM编码。

options的属性及说明如下：

| pramas        | type            | describe | default | demo                                    |
| ------------- | --------------- | -------- | ------- | --------------------------------------- |
| algorithm     |                 | 算法     | HS256   |                                         |
| expiresIn     | string/number   | 时间跨度 | -       | 60, '2 days', '10h', '7d', 120 => 120ms |
| notBefore     | string / number | 时间跨度 |         |                                         |
| audience      |                 |          |         |                                         |
| issuer        |                 |          |         |                                         |
| jwtid         |                 |          |         |                                         |
| subject       |                 |          |         |                                         |
| noTimestamp   |                 |          |         |                                         |
| header        |                 |          |         |                                         |
| keyid         |                 |          |         |                                         |
| mutatePayload |                 |          |         |                                         |

有一些没有默认值的属性（expiresIn, notBefore, audience, subject, issuer）。这些属性也可以写在payload里。若写payload时分别使用exp, nbf, aud, sub, iss对应.但是不能同时在options和payload里出现。

<!-- exp/nbf/iat必须是数值型的时间。 -->
expiresIn / notBefore 若是Number时，则被理解为秒。若是String，则需要使用时间单位（`days`,`hours`），否则使用ms为单位。

options.header是自定义的。

若没有设置`noTimestamp`则生成jwt时默认包括`iat`.若iat写在payload里，它将代替真实的时间戳。并重新计算其它属性。（比如option.expiresIn）

## token的有效时间

exp字段应该是一个从纪元时间开始的一个数值。

下面是一个例子：
    jwt.sign({
        exp: Math.floor(Date.now() / 1000) + (60 * 60), // 1h
        data: 'foobar'
    }, 'secret')
    
    jwt.sign({
        data: 'foobar'
        }, 'secret', {expiresIn: 60 * 60}) // 1h
    
    jwt.sign({data: 'foobar'}, 'secret', {expiresIn: '1h'}) // 1h
### jwt.verify(token, secretOrPublicKey, [options, cb])
若有cb则异步回调函数。该函数的参数是err/token。
若没有cd则同步返回jwt。
secretOrPublicKey 是解码的公钥。可以是string / buffer / 包含HMAC 算法。
若jwt.verify是异步方法。secretOrPublicKey可以是一个返回公钥的方法。
| 属性             | type            | describe                                       | default |
| ---------------- | --------------- | ---------------------------------------------- | ------- |
| algorithms       | Array           | 由算法的名字组成的数组。                       |         |
| audience         | String / Regexp | 受众                                           |         |
| complete         | Object          | 返回解码后的对象{payload, header, signature}   |         |
| issuer           | string / Array  | iss 发布者                                     |         |
| ignoreExpiration |                 | 是否忽略token里的终止期限。                    |         |
| ignoreNotBefore  |                 |                                                |         |
| subject          |                 | 主题                                           |         |
| clockTolerance   | number          | 检查nbf / exp 时处理一些不范围与服务器的时间差 |         |
| maxAge           | string / Number | 最大的有效时间                                 |         |
| clockTimestamp   |                 | 使用当前时间                                   |         |
| nonce            |                 | 一般用到Open ID                                |         |

### jwt.decode(token[, options])

同步返回解码后的payload.
若不相信token是有效的,需要使用jwt.verify(...)

|          |         |                                                      |      |
| -------- | ------- | ---------------------------------------------------- | ---- |
| json     |         | 若header里没有`"typ": "JWT"`，则强制使用`JSON.parse` |      |
| complete | Boolean | 返回解码后的对象：{payload, header}                  |      |

## error对象

jsonwebtoken定义了三个error对象
TokenExpiredError // 当token超时时抛出。

    err = {
        name: 'TokenExpiredError',
        massage: 'jwt expired',
        expired: [ExpDate]
    }

JsonWebTokenError

    err = {
        name: 'JsonWebTokenError',
        message: 'jwt malformed' // 'jwt malformed', 'jwt signature in required', 'invalid signature', 'jwt audience invalid. expected: [OPTIONS AUDIENCE]', 'jwt issuer invalid. expected: [OPTIONS ISSUER]', 'jwt id invalid. expected:[OPTIONS JWT ID]', 'jwt subject invalid. expected: [OPTIONS SUBJECT]'
    }

NotBeforeError
当当前时间超过nbf的值时抛出该错误。

    err = {
        name: 'NotBeforeError',
        message: 'jwt not active',
        date: 2018-10-04T16:10:44.000Z
    }
## 支持的加密算法

## 更新jwt

