# install

## npm & Babel

首先，我的需要安装编译器来揭掉flow types.我们可以在`Babel`和`flow-remove-types`中选择一个。  
`Babel`是一个支持flow的`javascript`编译器。它可以揭掉代码中的注释。  
第二，使用`npm`或`Yarn`安装`babel-cli`和`babel-preset-flow`。  
`npm i babel-cli babel-preset-flow -S`  
第三，我们需要在项目的根目录创建一个`.babelrc`文件（若存在，则不创建）。设置`presets`属性值中包括`flow`。  
```
{
    "presets": ["flow"]
}
```
如果我们的原文件在`src`目录下，我们又想把它编译到其它的目录下。我可以使用下面的命令。   
`./node_modules/.bin/babel src/ -d lib/`  
为了方便，可以使用另一个方法。在`package.json`文件里设置`scripts`.  
```
{
    name: "project",
    "main": "lib/index.js",
    "scripts": {
        "build": "babel src/ -d lib/",
        "prepublish": "npm run build"
    }
}
```
> 提示：当我们在npm上注册我们的代码前，会执行`prepublish`脚本。一般这个脚本使用编译文件。  

## setup flow

flow最好的工作方式是在每个项目中安装明确版本的flow,其次是使用全局方式安装。  
若你有已经熟练`npm`或`yarn`，那你一定可以熟练的处理。在`devDependency`里安装`flow-bin`。  
`npm i flow-bin -S`  
在`package.json`里的`script`里添加`flow`  
```
{
    "name": "flow",
    "version": "1.0.0",
    "devDependencies": {
        "flow-bin": "^0.99.0"
    },
    "scripts": {
        "flow": "flow"
    }
}
```
执行下面的命令。  
第一次执行时：`npm run flow init`  
非第一次执行时：`npm run flow`  

## npm & flow-remove-types

## Yarn & Babel

## Yarn & flow-remove-types

