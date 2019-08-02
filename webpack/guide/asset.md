# 资源管理

## 加载css

创建style.css

```
// src/style.css
.hello {
	color: red;
}
```

修改配置文件的loader.

```
output: {
	filename: 'bundle.js',
	path: path.resolve(__dirname, 'dist')
},
module: {
	rules: [
		{
			test: /\.css$/,
			use: [
				'style-loader',
				'css-loader'
			]
		}
	]
}
```

安装依赖（这里用的依赖是loader）

```
npm i -D style-loader css-loader
```

然后打包

```
npm run build
```

在浏览器中打开html文件后发现hello webpack变成的红色。

***

### 备注

要加载非js文件就需要相应文件的loader.

这里加载css后以`style`标签的形式插入head。就需要先使用`css-loader`再使用`style-loader`。在配置文件里按倒序排列。

加载资源时使用正则表达式匹配后使用相应加载器加载。

## 加载图片

在项目内添加图片

```
// src/first.png
```

在js文件中引入并使用图片。把图片做为img使用。

```
// src/index.js
import _ from 'lodash'
import './style.css'
import Icon from './first.jpg'
console.log(Icon, 1234)
function component () {
  const element = document.createElement('div')
  element.innerHTML = _.join(['Hello', 'webpack'], ' ')
  element.classList.add('hello')
  const myIcon = new Image()
  myIcon.src = Icon
  element.appendChild(myIcon)
  return element
}
document.body.appendChild(component())
```

修改css文件。把图片做为背景图片使用。

```
.hello {
  color: red;
  background: url('./first.jpg');
}
.hello > img {
  width: 250px;
}
```

安装依赖

```
npm i -D file-loader
```

打包

```
npm run build
```

***

### 备注

使用图片就2种情况：1.在html中作为图片使用。2.在css中作为背景图片使用（border的背景也属于背景图片类）。

在这2种情况下都会得到表示图片最终url的变量。

打包不会压缩图片。

## 加载字体

在src里添加字体

```
// src/FZQINGSTJW.TTF
```

在css中使用该字体

```
@font-face {
  font-family: 'MyFont';
  src: url('./FZQINGSTJW.TTF');
  font-weight: 400;
  font-style: normal;
}
.hello {
  color: red;
  font-family: 'MyFont';
  background: url('./first.jpg');
}
...
```

***

### 备注

我在做这里时遇到了浏览器对@font-family @font-face的兼容性。有关它们的详细说明主看confusion/font-family

## 加载数据

下面的例子加载了xml和json数据。

创建xml/json文件

```
// // src/data.xml
<?xml version="1.0" encoding="UTF-8"?>
<note>
  <to>Mary</to>
  <from>John</from>
  <heading>Reminder</heading>
  <body>Call Cindy on Tuesday</body>
</note>
```

```
// src/students.json
[
  {
    "name": "first",
    "age": 25
  },
  {
    "name": "second",
    "age": 22
  },
  {
    "name": "third",
    "age": 22
  },
  {
    "name": "fourth",
    "age": 26
  },
  {
    "name": "fifth",
    "age": 23
  },
  {
    "name": "sixth",
    "age": 24
  }
]
```

引入并使用数据

```
// src/index.js
import _ from 'lodash'
import './style.css'
import Icon from './first.jpg'
import students from './students.json'
import Data from './data.xml'
console.log(Data)
function component () {
  const element = document.createElement('div')
  element.innerHTML = _.join(['Hello', 'webpack'], ' ')
  element.classList.add('hello')
  const myIcon = new Image()
  myIcon.src = Icon
  element.appendChild(myIcon)
  generateStudent(element)
  return element
}
function generateStudent (parent) {
  let ul = document.createElement('ul')
  parent.appendChild(students.reduce((res, cur) => {
    let li = document.createElement('li')
    li.innerHTML = `${cur.name} ${cur.age}`
    res.appendChild(li)
    return res
  }, ul))
}
document.body.appendChild(component())
```

修改配置文件

```
// webpack.config.js
...
module: {
	rules: [
		...
		{
			test: /\.xml$/,
			use: [
				'xml-loader'
			]
		}
	]
}
```

安装依赖

```
npm i -D xml-loader
```

执行打包。

***

### 备注

因在webpack >= 2时，引入json可以正常工作。在引入xml文件时需要使用xml-loader。

## 全局资源

当项目需要使用的资源太多时，就需要把资源整合在一起。

```
// 目录结构
project
|- src
		|- ...
|- dist
|- assets
		|- data.xml
		|- students.json
		|- font.ttf
		|- ...
```

