# npm-package.json

这里有package.json文件里的所有的必要的说明。

## name

它就是package的名字。若你要发布该package，则name/version是必须的配置项。它们将确定惟一的package。若不发布，则无所谓。

有一些规则：

- 字数必须小于214个。
- 不能使用`.`、`_`开头。
- 不能使用大写字母。
- 不能使用url非安全字符。

有一些提示：

- 不要使名字与核心模块的名字一样。
- 不要使用`js`、`node`.
- 不要太长。
- 起一个别人没用过的。

## version

最好语义化。

## description

该package的说明。有便别人了解。string类型。

## keywords

由string组成的array。有助于别人发现该package。

## homepage

该包的url.如`"homepage":"https://github.com/owner/project#readme"`

## bugs

若该包有bug，使网友可以通过email联系上作者。

```
{"url":"...",
"email": "project@hostname.com"}
```

## license

许可证

## people fields: author, contributors

author 是一个作者。

contributors 是一群作者组成的array.

## files
## main
## browser
## bin
## man
## directories
### directories.lib
### directories.bin
### directories.man
### directories.doc
### directories.example
### directories.test
## repository
## scripts
## config
## dependencies
## URLs as Dependencies
## Git URLs as Dependencies
## GitHub URLs
## Local Paths
## devDependencies
## peerDependencies
## bundledDependencies
## optionalDependencies
## engines
## engineStrict
## os
## cpu
## preferGlobal
## private
## publishConfig
## DEFAULT VALUES
## SEE ALSE

- semver
- npm-init
- npm-version
- npm-config
- npm-help
- npm-install
- npm-publish
- npm-uninstall