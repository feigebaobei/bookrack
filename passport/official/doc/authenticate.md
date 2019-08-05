# authenticate

若要验证请求，只需要执行`passport.authenticate()`

passport.authenticate(strategy, options, cb)

stragety是定义好的验证策略。

options

| name            | type           | default | describe                                                     |
| --------------- | -------------- | ------- | ------------------------------------------------------------ |
| successRedirect | string         |         | 验证成功后进入的路由。                                       |
| failureRedirect | string         |         | 验证失败后进入的路由。                                       |
| failureFlash    | boolean/string |         | 验证失败后给前端的提示信息。（可以自定义：{failureFlash: 'string'}） |
| successFlash    | boolean/string |         | 成功后给前端的信息。                                         |
| session         | boolean        | true    | 是否使用session里的数据。                                    |

cb是自定义的验证回调函数。fn(err, user, info){}

