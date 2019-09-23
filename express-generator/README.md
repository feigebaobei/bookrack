# Introduction

用来生成express项目。

## installation

```
npm i -g express-generator
```

## quick start

快速生成express应用。

```
// 创建app
express --view=hbs /tmp/foo && cd /tmp/foo
// 安装依赖
npm i
// 启动express应用
npm start
```

## command line options

若为一个属性设置多个值，则后面的会覆盖前面的。

```
    --version        output the version number 输出版本号
-H, --hogan          add hogan.js engine support
-v, --view <engine>  add view <engine> support (dust|ejs|hbs|hjs|jade|pug|twig|vash) (defaults to jade)
    --no-view        use static html instead of view engine
-c, --css <engine>   add stylesheet <engine> support (less|stylus|compass|sass) (defaults to plain css)
    --git            add .gitignore
-f, --force          force on non-empty directory 强行在非空目录中创建项目
-h, --help           output usage information
```

