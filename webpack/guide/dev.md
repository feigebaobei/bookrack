# 开发

## 跟踪bug位置

在前面的例子中若出现了bug也不知道是哪个源文件第几行出现的问题。下面就学习如何解决这个问题。下面的代码是基于guide/output里的例子的代码的。

在打包配置文件里设置devtool、mode.

```
// webpack.config.js
...
module.exports= {
	mode: 'development',
	devtool: 'inline-source-map',
	...
}
```

随便写一个bug.

执行打包。

在浏览器是浏览。浏览器报错时会返回哪个源文件的哪行出现的问题。

***

### 备注

devtool只能在开发环境工作。（mode: 'development'）

有关mode的说明请查看config/mode。

## 选择合适的开发工具

现在我们每次打包都需要执行`npm run build` 或`webpack`。希望出现一种自动打包的工具。下面有3个这样的工具。

1. 为webpack设置watch模式
2. webpack-dev-server
3. webpack-dev-middleware

### watch模式

修改package.json

```
// package.json
...
"scripts": {
	"watch": "webpack --watch",
	...
}
```

执行监听模式打包

```
npm run watch // 如同执行 webpack --watch
```

这样一旦开始打包后就不会自动结束（需要手动结束）。每次webpack监听到有文件变动（就是修改文件后保存）时就会重新打包。在浏览器中刷新页面就能看到重新打包后的内容。

***

#### 备注

也可以不修改package.json，直接在命令行中执行`webpack --watch`.

### webpack-dev-server

安装依赖

```
npm i -D webpack-dev-server
```

修改配置文件

```
// webpack.config.js
...
module.exports = {
	...
	devServer: {
		contentBase: './dist' // 以./dist目录做为服务目录。
	}
}
```

修改package.json

```
// package.json
"scripts": {
	"start": "webpack-dev-server --open"
}
```

#### 备注

webpack-dev-server会启动一个服务去提供打包后的内容。把打包后的内容放在内存里。contentBase是指定服务的根目录 。

publicPach是指定打包文件所在的目录。

服务默认启动在localhost:8080.

当有文件修改后会自动重新打包。

有关webpack-dev-server的说明请查看guide/DevServe

### webpack-dev-middlewares

安装依赖

```
npm i -D express webpack-dev-middleware
```

修改配置

```
// webpack.config.js
...
module.exports = {
	...
	output: {
		...
		publicPath: '/'
	}
}
```

新建服务入口文件

```
// server.js
const express = require('express');
const webpack = require('webpack');
const webpackDevMiddleware = require('webpack-dev-middleware');
const app = express();
const config = require('./webpack.config.js');
const compiler = webpack(config);
// Tell express to use the webpack-dev-middleware and use the webpack.config.js
// configuration file as a base.
app.use(webpackDevMiddleware(compiler, {
  publicPath: config.output.publicPath
}));
// Serve the files on port 3000.
app.listen(3000, function () {
  console.log('Example app listening on port 3000!\n');
});
```

做为npm的脚本使用

```
// package.json
...
"scripts": {
	...
	"server": "node server.js"
}
```

执行打包脚本

```
npm run server
```

***

#### 备注

webpack-dev-middleware内部使用webpack-dev-server。这个例子使用它和express完成的。关于它细致的东西我也不会。

***

### 备注

官网上说，一般使用webpack-dev-server.