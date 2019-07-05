# Commande Line

[TOC]

### 安装

`npm i jade -g`

### 使用

`$ jade [options] [dir|file...]`

| key of options        | 说明                                                         |      |
| --------------------- | ------------------------------------------------------------ | ---- |
| -h, --help            | 输出使用方法                                                 |      |
| -V, --version         | 输出版本号                                                   |      |
| -O, --obj <path\|str> |                                                              |      |
| -o, --out <dir>       | 设置编译后文件的位置                                         |      |
| -p, --path <path>     | 指定编译的文件名                                             |      |
| -P, --pretty          | 编译成有缩进的html文件                                       |      |
| -c, --client          | 在客户端运行时编译                                           |      |
| -n, --name <str>      | 编译模板的名字。（使用-c时必需。）                           |      |
| -D, --no-debug        | 不使用debug编译。编译后的结果会小。                          |      |
| -w, --watch           | 监听文件有变化时，自动重渲染。                               |      |
| --name-after-file     |                                                              |      |
| --doctype <str>       | 在命令行里指定文件的doctype。（当模板文件里没有指定doctype时） |      |

|                                 | 说明                                   |
| ------------------------------- | -------------------------------------- |
| `$ jade templates`              | 编译模板                               |
| `$ jade {foo, bar, third}.jade` | 编译foo.jade bar.jade third.jade       |
| `$ jade <my.jade> my.html`      | 编译my.jade后覆盖my.html               |
| `$ echo "h1 Jade!" | jade`      |                                        |
| `$ jade foo bar --out /tmp`     | 把foo bar 目录下的jade编译结果放在/tmp |
|                                 |                                        |
|                                 |                                        |

