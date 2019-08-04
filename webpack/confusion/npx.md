# npx

我查的说明是：临时执行命令。

有时某个命令行工具没有安装，但是又想使用，就可以使用使用npx.

## 概要

```
npx [options] <command>[@version] [command-arg]...
npx [options] [-p|--package <pkg>]... <command> [command-arg]...
npx [options] -c '<command-string>'
npx --shell-auto-fallback [shell]
```

## install

```
npm i -g npx
```

一般不用手动安装。因为node内置了npx。所以安装了node就安装了npx。可以手动更新。

## 说明

执行`<command>`时会在本地的`node_modules/.bin`(这是包的二进制文件)或缓存中心查找相应的包。若找到则执行命令。若设置了`$PATH`则再查找。若以上都没找到则临时安装并执行。执行完后删除该包。

若设置了`-p, --package`，则使用临时安装的包。否使用使用`node_models/.bin`里的包。所有的命令都可以使用npx。包括git。

若使用完整说明符或使用`--package`则问题使用临时安装的包。

| options               | 说明                                                         | 类型    | demo                 |
| --------------------- | ------------------------------------------------------------ | ------- | -------------------- |
| -p, --package         | 指定需要安装的包。因npx默认认为包的名称和命令的拼写是一样的，但是实际情况不全是这样。所以此时需要指定包名和命令。可以指定多个需要安装的包。 | package | `-p first -p second` |
| --no-install          | 在执行npx时，只是执行命令，或已经安装，或在当前路径，或在`$prefix/node_modules/.bin`，则不使用安装。 |         |                      |
| --cache               | 设置npm的缓存位置。                                          | path    |                      |
| --userconfig          | 为npm指定自定义的配置文件。                                  | path    |                      |
| -c                    | 看不懂                                                       | string  |                      |
| --shell               | 调用shell的命令                                              | string  |                      |
| --shell-auto-fallback | 看不懂                                                       | shell   |                      |
| --ignore-existing     | 忽略已经存在的包，直接安装。                                 |         |                      |
| -q, --quiet           | 不输出。会输出子命令的输出。                                 |         |                      |
| -n, --node-arg        | 扩展node的参数。                                             |         |                      |
| -v, --version         | 返回npx的版本号                                              |         |                      |

