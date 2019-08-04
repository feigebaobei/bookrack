# 输出管理

## 输出多个js文件

这个例子是在`guide/index.md`里的例子的基础上开发的。我们会打包出多个js文件。

创建新的js文件。用于入口文件。

```
// src/print.js
export default function printMe() {
	console.log('I get called from print.js!')
}
```

引用上个文件

```
// src/index.js
import _ from 'lodash'
import printMe from './print.js'
function component () {
  const element = document.createElement('div')
  const button = document.createElement('button')
  element.innerHTML = _.join(['Hello', 'webpack'], ' ')
  button.innerHTML = 'Click me and check the console!'
  button.onclick = printMe
  element.appendChild(button)
  return element
}
document.body.appendChild(component())
```

修改html

```
// dist/index.html
<!doctype html>
<html>
  <head>
    <title>Output Management</title>
    <script src="./print.bundle.js"></script>
  </head>
  <body>
    <script src="./app.bundle.js"></script>
  </body>
</html>
```

修改配置文件。这里控制的输出后的文件。

```
// webpack.config.js
const path = require('path')
module.exports = {
  entry: {
    app: './src/index.js',
    print: './src/print.js'
  },
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist')
  }
}
```

执行打包。

***

### 备注

我们在html里写死了打包后的文件。这个例子说明了打包成多个有js文件时需要在配置文件时写入多个入口文件。原来多个输出需要多个输入。

若想不写死打包文件的引用链接，请学习下个demo.

## 生成html文件

安装依赖

```
npm i -D html-webpack-plugin
```

在配置文件中使用插件

```
// webpack.config.js
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
  entry: {
    app: './src/index.js',
    print: './src/print.js'
  },
  plugins: [
    new HtmlWebpackPlugin({
      title: 'Output Management'
    })
  ],
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist')
  }
}
```

执行打包

***

### 备注

html-webpack-template可以生成一个html文件，并使用所有打包后的js文件。凭借这一点。我们可以只写入口文件和资源文件，然后打包。生成的html文件就可以正常使用了。

有关html-webpack-template的详细说明，请查看plugin/htmlWebpackTemplate.

## 清理`/dist`目录

有时执行多次打包后会在`/dist`目录生成好多杂乱的内容。（我们做的例子里还没有这样的事发生。若使用hash生成文件名就会造成`/dist`目录杂乱。）下面的插件可以在清空该目录。保证每次打包后的输出目录都是整洁的。

安装依赖

```
npm i -D clean-webpack-plugin
```

在配置文件中使用这个插件

```
// webpack.config.js
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const { CleanWebpackPlugin } = require('clean-webpack-plugin')
module.exports = {
  entry: {
    app: './src/index.js',
    print: './src/print.js'
  },
  plugins: [
    new CleanWebpackPlugin,
    new HtmlWebpackPlugin({
      title: 'Output Management'
    })
  ],
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist')
  }
}
```

***

### 备注

有关html-webpack-template的详细说明，请查看plugin/htmlWebpackTemplate.