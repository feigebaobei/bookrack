# 入门

webpack可以打包js模块。

现在来一个最简单的demo.

## demo

```
mkdir webpack
cd webpack
mkdir wpGuide
cd wpGuide
npm init -y
npm i webpack -D
npm i webpack-cli -D
npm i lodash
```

创建如下文档结构

```
  wpGuide
  |- package.json
  |- /node_modules
+ |- dist
+ 	|- index.html
+ |- src
+   |- ndex.js
```

```
// dist/index.js
import _ from 'lodash'
function component() {
	const element= document.createElement('div')
	element.innnerHTML = _.json(['Hello', 'webpack'], ' ')
	return element
}
document.body.appendChild(component())
```

```
// index.html
<!doctype html>
<html>
  <head>
  	<title>Getting Started</title>
  </head>
  <body>
  	<script src="main.js"></script>
  </body>
</html>
```

```
// package.json
{
  "name": "webpack-demo",
  "version": "1.0.0",
  "description": "",
+ "private": true,
- "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^4.20.2",
    "webpack-cli": "^3.1.2"
  },
  "dependencies": {}
}
```

在命令行工具中执行

```
npx webpack
```

在浏览器中打开index.html会看到hello webpack.

## 工作方式

1. 先创建了一个入口文件（index.js）.在package.json里设置入口文件。
2. 在入口文件中引用别的文件（lodash）。这个文件是使用`npm i lodash`安装的。
3. 执行`npx webpack`去打包项目。结果会在`/dist`目录下生成`main.js`。
4. 在index.html文件里引入打包后的文件。

## 使用配置文件

在上面的例子中使用`npx webpack`打包的。因为webpack 4不需要任何配置。在实际项目中会用到各种各样的配置项。下面使用配置文件打包。

在根目录下创建`webpack.config.js`。项目结构如下：

```
	wpGuide
+ |- webpack.config.js
	|- ....
```

```
// webpack.config.js
const path = require('path')
module.exports = {
	entry: './src/index.js',
	output: {
		filename: 'main.js',
		path: path.resolve(__dirname, 'dist')
	}
}
```

使用配置文件打包。

```
npx webpack --config webpack.config.js
```

## 为npm添加脚本

```
// package.json
  {
    "name": "webpack-demo",
    "version": "1.0.0",
    "description": "",
    "scripts": {
-      "test": "echo \"Error: no test specified\" && exit 1"
+      "test": "echo \"Error: no test specified\" && exit 1",
+      "build": "webpack"
    },
    "keywords": [],
    "author": "",
    "license": "ISC",
    "devDependencies": {
      "webpack": "^4.20.2",
      "webpack-cli": "^3.1.2"
    },
    "dependencies": {
      "lodash": "^4.17.5"
    }
  }
```

使用npm脚本打包。

```
npm run build
```

***

## 备注

npx webpack // 执行webpack

在confusion/npx.md有详细说明。