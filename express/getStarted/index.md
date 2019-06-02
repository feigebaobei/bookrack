# Getting Started

假定我们已经安装了node.js。创建一个应用目录。

```
$ mkdir myapp
$ cd myapp
```

在我们的根目录使用`npm init`创建`package.json`文件。若要了解更多关于package.json的信息可以查看[明确package.json的工作内容](https://docs.npmjs.com/files/package.json)

```
$ npm init
```

这行命令会做几件事。比如设置应用的name/version.我们也可以使用默认数据。像下面这样：

```
entry point: (index.js)
```

进行`app.js`或者你想设置为主要文件的文件。建议使用默认的文件名。

现在项目中在dependencies安装exprss。如下：

```
$ npm i express -S
```

临时在dependencies安装express。(临时安装不会保存在dependencies)

```
$ npm i express --no-save
```

