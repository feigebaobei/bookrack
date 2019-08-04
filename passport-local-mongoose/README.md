# Introduction

这个插件mongoose的插件用来与passport结合创建用户的username/password.

## 教程

npm给了一个[教程链接](<https://mherman.org/blog/user-authentication-with-passport-dot-js/>)

## install

```
npm i passport-local-mongoose
```

passport-local-mongoose 不依赖于 passport / passport-local。但是需要用到它们才能正常工作，所以开发者需要安装三个包。

```
npm i passport passport-local passport-local-mongoose
```

## uasge

### 插件passport-local-mongoose

引入plm(passport-local-mongoose)，再为user(它是用户的schema)添加plm插件。

```
// module/user.js
const mongoose = require('mongoose')
const Schema = mongoose.Schema
const passportLocalMongoose = require('passport-local-mongoose')
const User = new Schema()
User.plugin(passportLocalMongoose)
module.exports = mongoose.model('User', User)
```

plm会user添加username、hash、salt域，分别保存username/经过hash的password和盐值。还会为user添加一些静态方法。

## 配置passport/passport-local

## 简单地配置passport/passport-local

## async/await

## options

| name                | type    | default     | describe                                            |
| ------------------- | ------- | ----------- | --------------------------------------------------- |
| salten              | number  | 32          | 盐值的长度                                          |
| iterations          | number  | 25000       | 使用pbkdf2进行哈希算法的次数。                      |
| keylen              | number  | 512         | 生成key（hash）的长度。                             |
| digestAlgorithm     |         | sha256      | pbkdf2的消化算法。                                  |
| interval            | number  | 100         | 当登录失败时最长经过多长时间（ms）再执行一次。      |
| maxInterval         | number  | 30000       | 最多尝试多少次。                                    |
| usernameField       | string  | username    | 用户名。                                            |
| usernameUnique      | boolean | true        | username字段是否是惟一键。                          |
| saltField           | string  | 'salt'      | 盐值的key.                                          |
| hashField           |         | 'hash'      |                                                     |
| attempsField        |         | 'attemps'   | 从最近一次登录成功开始算登录失败的次数的key.        |
| lastLoginField      |         | 'last'      | 最后一次尝试登录的时间戳的key。                     |
| selectFields        |         | 'undefined' | 从mongodb中选中的字段。默认全部选中。               |
| usernameLowerCase   |         | 'false'     | 是否把username域的值转化为小写。                    |
| populateFields      |         | undefined   | 指定一个域作用于findByUsername.                     |
| encoding            |         | hex         | 指定使用什么方式生成salt / hash.                    |
| limitAttempts       |         | false       | 是否有登录失败或尝试登录被限制。                    |
| maxAttemps          |         | infinity    | 最大的登录失败次数。                                |
| passwordValidator   |         |             | 验证用户密码的函数（function (password, cb) {...}） |
| usernameQueryFields |         |             | 使用哪个字段确定一个用户。                          |
| findByUsername      |         |             |                                                     |

## 为schema添加的静态方法

| 方法                         | 功能                                               |
| ---------------------------- | -------------------------------------------------- |
| authenticate()               | 生成在passportr的LocalStrategy里使用的函数。       |
| serializeUser()              | 生成用于把用户信息序列化到session里的函数。        |
| deserializeUser()            | 反序列                                             |
| register(user, password, cb) | 使用一个user实例，密码，回调函数去注册一个新用户。 |
| findByUsername()             | 使用惟一的username找到user实例。                   |
| createStrategy()             | 生成一个匹配passport-local实例的本地策略。         |

## api

| 实例方法                           | 说明                                                         |
| ---------------------------------- | ------------------------------------------------------------ |
| setPassword(pw, [cb])              | 设置用户密码。不保存用户对象。若没有cb,则返回Promise对象。   |
| changePassword(oldpw, newpw, [cb]) | 改变用户密码并保存用户对象。若没有cb则返回Promise对象。若旧密码不匹配则返回 incorrectPasswordError. |
| authenticate(pw, [cb])             | 验证用户对象。若没有cd，则返回promise对象。                  |
| resetAttempts([cb])                | 重置用户密码错误的次数。                                     |

### 回调函数的参数

- err 当哈希算法抛出错误时有错误，否则为null
- thisModel 若验证成功则返回user.否则返回false.
- passwordErr 若验证不通过则返回一个AuthenticationError的实例，说明密码错误。否则返回 undefind.

## 错误信息

| 错误类型               | content                                                |
| ---------------------- | ------------------------------------------------------ |
| MissingPasswordError   | 'No password was given'                                |
| AttemptTooSoonError    | 'Account is currently locked.Try again later'          |
| TooManyAttemptsError   | 'Account locked due to too many failed login attempts' |
| NoSaltValueStoredError | 'Authentication not possible. No salt value stored'    |
| IncorrectPasswordError | 'Password or username are incorrect'                   |
| IncorrectUsernameError | 'Password or username are incorrect'                   |
| MissingUsernameError   | 'No username was given'                                |
| UserExistsError        | 'A user with the given username is already registered' |

## 哈希算法

plm使用pbkdf2库作为hash算法库。

## demo

保存在数据库里是这样的。

```
{
    "_id":"5d462eb9ac1ad310c472911f",
    "admin":false,
    "username":"third",
    "salt":"772f3e95f089e156d083eb16a320ef84b814f8cd61a26f0af588d46ee80149aa",
    "hash":"e2751500529490238eaa4c8844294590bb98975a1a173f83db977508fe19f59f6a25f8dc4bdd9451b7f1cfb66145423064071fb21c357a7e9e27d1d10fe839acb06184a17fd0b662d85c1d36c3a74a279d52958018ceb1a882dea54cbc9b7dfbcdc720cbee3c345b8851e5cf1f8edac38051c52e0232bb636ea6a19dbea3b00e7f34f5de6fc75b3a0030ec75d681f564baf41fee88cb041628412a47e2d48628f5fc918d182eea21cf39a6d10a1335a7dc8c2e73d82d8ca10e0a16da61481f6b45e16d778ee40bdbba3f3ea503ab7e71dbe706ce402f1b3e3be728e5e2af19847a71ad85759492db442371955fe9974db8b22f5b367fb401c23068f7c30a5bf1acec0868bf8fb0cc533d87aad54944d16c0d93b781e3508e8bb023683d509691fc84c78e8bd7e579aaf09f8c8fb4e2d48ccde68eb925390a29772de103876f49aecdd2d95ad648cc4036face409def1ee5c36c3195e19e59a9c0d43a167149ff0997bec6700c301acd9d45ced42d05f4d7b084721ddeda6e6f7366349a84d12b4db9cbfd4bf1e0a4152c6bdbfce19898f4e6e5a888e037be45b8dd55c8e62a115e156ff146692a64d7ac4279f9d34c71699324b2614e6d4e663e3db9377065e9770f49ac7424b75d1efca5233d39f0332d34783cdf3e81b3bcb8760b8254405d52462db807f81a0bd6d60be95e9a54c7ae23c6554e9a5f9a0e27bf8000b04104",
    "__v":0
}
```

| field    | describe                                                     |
| -------- | ------------------------------------------------------------ |
| _id      | mongodb生成的的ObjectId对象。它使用12个字节的存储空间（每2个16位进制的数字占用1个字节，共24个16进制数字）。它是bson类型的字符串。依次表示：unix时间戳（4字节）mongodb的机器（3字节）生成此_id的进程（2字节）随机数（3字节） |
| admin    | 这是用户自己设置的域。                                       |
| username | plm（passport-local-mongoose）设置的用户名域。               |
| salt     | 盐值                                                         |
| hash     | 经过hash的（passwort + salt）的值                            |
| __v      | 版本号                                                       |

