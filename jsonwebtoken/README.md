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

exp/nbf/iat必须是数值型的时间。

options.header是自定义的。

若没有设置`noTimestamp`则生成jwt时默认包括`iat`.若iat写在payload里，它将代替真实的时间戳。并重新计算其它属性。（比如option.expiresIn）

## token的有效时间



## error对象

## 支持的加密算法

## 更新jwt

