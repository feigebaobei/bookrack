# htmlWebpackTemplate

生成html文件的插件。

## 安装

```
npm i -D html-webpack-plugin
```

## 基本用法

```
var HtmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
	...
	plugin: [
		new HtmlWebpackPlugin()
	]
}
```

## 配置项

| name               | type                        | default | description                    |
| ------------------ | --------------------------- | ------- | ------------------------------ |
| title              | string                      |         |                                |
| filename           | string                      |         |                                |
| template           | string                      |         | 指定模板文件                   |
| templateParameters | {boolean\|object\|funciton} |         | 模板文件需要的参数             |
| inject             | {boolean\|string}           |         |                                |
| favicon            | {string}                    |         |                                |
| meta               | {object}                    |         |                                |
| base               | {object\|string\|false}     |         | 设置base标签                   |
| minify             | {boolean\|object}           |         | 是否压缩                       |
| hash               | {boolean}                   |         |                                |
| cache              | {boolean}                   |         | 当文件改变时打包。             |
| showErrors         | {boolean}                   |         | 当出现错误时是否显示在html中。 |
| chunks             | {?}                         |         | 只添加哪些chunk                |
| chunksSortMode     | {string\|function}          |         |                                |
| excludeChunks      | {array.<string>}            |         |                                |
| xhtml              | {boolean}                   |         |                                |

