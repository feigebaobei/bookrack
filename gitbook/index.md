# index

gitbook.md

# decription

可以快速生成方案的工具。

# install

```
npm i gitbook-cli -g
// 查看是否安装成功
gitbook -v
// 我执行后，没看到输出。在执行 gitbook init 时成功初始化项目。
```

官方给的编辑器是gitbook editor。但是我找不到下载的地方。在国内的网站（`http://www.pc6.com/mac/238113.html`）上下载了一个。
听说它还有一些插件。
我没使用gitbook editor. 使用的是typora.

# usage

```
// 生成基本的项目结构
gitbook init [path/to/dir]
// 启动本地的服务。可以在 http://localhost:4000 想看结果。
gitbook serve
// 构建
gitbooke build
// 使用预发布版
gitbook fetch bata
// debug
gitbook build ./ --log=debug --debug
// help
gitbook help
```

# description

执行完gitbook init后在项目根目录会生成README.md、SUMMERY.md文件。
分别是项目的介绍文件和项目的结构文件。
SUMMERY.md
打开SUMMERY.md 显示-源代码模式
()里的是文件的路径
[]里的是文件在目录的名称
当编辑完目录，即完成项目结构后
在项目的根目录执行`gitbook init`会按照目录生成项目结构。

执行`gitbook serve`，会在根目录下生成`_book`目录。再在浏览器中打开`http://localhost:4000`会看到项目的渲染结果。这时项目是热更新的。

执行`gitbook build`，会在根目录下生成`_book`目录。这里是创建后的内容。

