# Language Reference

[TOC]

## doctype

指定文件的doctype

| 简短写法             | 编译结果                                                     |      |      |
| -------------------- | ------------------------------------------------------------ | ---- | ---- |
| doctype html         | <!DOCTYPE html>                                              |      |      |
| doctype xml          | <?xml version="1.0" encoding="utf-8" ?>                      |      |      |
| doctype transitional | <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> |      |      |
| doctype strict       | <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd"> |      |      |
| doctype frameset     | <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Frameset//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-frameset.dtd"> |      |      |
| doctype 1.1          | <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd"> |      |      |
| doctype basic        | <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML Basic 1.1//EN" "http://www.w3.org/TR/xhtml-basic/xhtml-basic11.dtd"> |      |      |
| doctype mobile       | <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML Basic 1.1//EN">     |      |      |

## Comments

`// 行注释`

`<!-- 块注释 -->`

## Attributes

为dom元素设置属性及属性值。

`dom(attribute="value")`

`dom(attribute="value")&attributes({key: "value", key2: 'value'})` // 单双引号都可以。attribuites的参数是对象。

若省略dom，则默认使用div。

为dom设置id/class时有简洁写法。使用#/.

```
doctype html
html(lang="en")
  head
  body
    h1 Jade - node template engine
    a(href="google.com") google
    input(type="text")
    input(type='text')
    div(id="id", class="class")
    div(id="id" class="class")
    div.button
    .content
    div(key="value")
    p test
    div(key="value")&attributes({bar: 'foo'})
```

```
<!DOCTYPE html>
<html lang="en">
  <head></head>
  <body>
    <h1>Jade - node template engine</h1><a href="google.com">google</a>
    <input type="text">
    <input type="text">
    <div id="id" class="class"></div>
    <div id="id" class="class"></div>
    <div class="button"></div>
    <div class="content"></div>
    <div key="value"></div>
    <p>test</p>
    <div key="value" bar="foo"></div>
  </body>
</html>
```

## plain text

- 使用管道符
- 在dom标签后同行输入
- 使用块级标签

```
doctype html
html(lang="en")
  head
  body
    p content
    |other
    |other
    |other
```



```
<!DOCTYPE html>
<html lang="en">
  <head></head>
  <body>
    <p>content</p>other
    other
    other
  </body>
</html>
```



## includes

在一个jade文件里使用另一个jade文件。也可以引入css/md/js/less等。

```
// index.jade
doctype html
html(lang="en")
  // head
  include ./head.jade
  body
    p content
    |other
    |other
    |other
    include ./foot.jade
```

```
// head.jade
head
  title my site
  script(src="../jquery.js")
  script(src="../app.js")
```

```
// foot.jade
#foot
  p Copyright (c) foobar
```

```
// index.html
<!DOCTYPE html>
<html lang="en">
  <!-- head-->
  <head>
    <title>my site</title>
    <script src="../jquery.js"></script>
    <script src="../app.js"></script>
  </head>
  <body>
    <p>content</p>other
    other
    other
    <div id="foot">
      <p>Copyright (c) foobar</p>
    </div>
  </body>
</html>
```

## extends / inheritance

先定义模板文件。在模板文件中设置需要可扩展的地方（这些地方会被继承者插入内容）。在继承的文件里指定母板再定义相应的内容块，若定义母板中未定义的块，则报错。继承与扩展是成对使用，有继承就一定有扩展。

```
// layout.jade
doctype html
html
  head
    block title
      <!-- 这里是默认内容 start -->
      title Default title
      <!-- 这里是默认内容 end -->
  body
    block content
```

```
// index.jade
extends ./layout.jade
block title
  title Article Title
block content
  h1 my article
```

```
<!DOCTYPE html>
<html>
  <head>
    <title>Article Title</title>
  </head>
  <body>
    <h1>my article</h1>
  </body>
</html>
```

## filters

可以jade文件时使用`:coffee-script :babel :uglify-js :less :markdown-it`语言。

做这里时我写成这样，发现jade不支持。`:markdown`

```
:markdown
	# title
	content
```

jade会报错。

```
In order to apply the transform markdown you must install one of "marked","supermarked","markdown-js","markdown"
```

jade让我们去安装marked / supermarked / markdown-js / markdown 中的一个。于是我在npm上查了markdown。发现它可以把md文件转换为html(`md2html /path/to/doc.md > /path/to/doc.html`)。要不是这点我才不安装呢。执行`npm i -g markdown `安装markdown.然后转换jade文件。

```
doctype html
html
  head
  body
    :markdown
      # title
      pararms
```

```
<!DOCTYPE html>
<html>
  <head></head>
  <body><h1>title</h1>

<p>pararms</p>
  </body>
</html>
```

这才只转换了markdown。我测试使用`:coffee-script`时也提示需要安装`jstransformer-coffee-script`.为了转换成html文件，还需要安装这么多文件。真烦。早知道这么不好用，我就不学了。

我在查答案过程发现：jade升级后是pug。

## code

使用js代码。需要`- `开头。

```
doctype html
html
  head
  body
    ul
    - for (var x = 0; x < 3; x++)
      li item #{x}
```

```
<!DOCTYPE html>
<html>
  <head></head>
  <body>
    <ul></ul>
    <li>item 0</li>
    <li>item 1</li>
    <li>item 2</li>
  </body>
</html>
```

tag后面紧跟`=`可以设置内容。html内容会逃逸。若希望不逃逸，使用使用`!=`。

```
doctype html
html
  head
  body
    div='content<p>'
    div!='content<p>'
    div = 'content<p>'
    - for (var x = 0; x < 3; x++)
      p='code is <escap>'
```

```
<!DOCTYPE html>
<html>
  <head></head>
  <body>
    <div>content&lt;p&gt;</div>
    <div>content<p></div>
    <div>= 'content<p>'</div>
    <p>code is &lt;escap&gt;</p>
    <p>code is &lt;escap&gt;</p>
    <p>code is &lt;escap&gt;</p>
  </body>
</html>
```

## conditionals

关于条件只有if else。

```
doctype html
html
  head
  body
    - var authorised = false
    #user
      if authorised
        h2 h2 title
        p.description text p text p
      else
        h2 other title
        p.class
      user use
```

```
<!DOCTYPE html>
<html>
  <head></head>
  <body>
    <div id="user">
      <h2>other title</h2>
      <p class="class"></p>
      <user>use</user>
    </div>
  </body>
</html>
```

## case

比if分类更多有关键字。类似switch。

```
doctype html
html
  head
  body
    - var friend = 10
    case friend
      when 0
        p you have no friend
      when 1
        p you have a friend
      default
        p you have #{friend} friend
```

```
<!DOCTYPE html>
<html>
  <head></head>
  <body>
    <p>you have 10 friend</p>
  </body>
</html>
```

## interpolation

在tag的内容/属性时使用变量时。使用`#{}`包裹变量名。

```
doctype html
html
  head
  body
    - for (var x = 0; x < 3; x++)
      p x is #{x}
```

```
<!DOCTYPE html>
<html>
  <head></head>
  <body>
    <p>x is 0</p>
    <p>x is 1</p>
    <p>x is 2</p>
  </body>
</html>
```

## mixin

定义一块在当前文件可以复用的内容块。

```
doctype html
html
  head
  body
    // 定义
    mixin list(id, ...items)
      ul(id=id)
        each item in items
          li=item
    // 使用
    +list('idName', 1, 2, 3, 4)
    +list('idName', 1, 2, 3, 4)
    hr
    mixin pet(name)
      li.pet=name
    ul
      +pet('cat')
      +pet('cat')
      +pet('cat')
```

```
<!DOCTYPE html>
<html>
  <head></head>
  <body>
    <!-- 定义-->
    <!-- 使用-->
        <ul id="idName">
          <li>1</li>
          <li>2</li>
          <li>3</li>
          <li>4</li>
        </ul>
        <ul id="idName">
          <li>1</li>
          <li>2</li>
          <li>3</li>
          <li>4</li>
        </ul>
    <hr>
    <ul>
          <li class="pet">cat</li>
          <li class="pet">cat</li>
          <li class="pet">cat</li>
    </ul>
    <hr>
  </body>
</html>
```

## iteration

就2个关键字，`each`, `while`

```
doctype html
html
  head
  body
    // each
    ul
      each val in [1, 2, 3, 4]
        li=val
    // while
    - var n = 0
    ul
      while n < 4
        li= n++
```

```
<!DOCTYPE html>
<html>
  <head></head>
  <body>
    <!-- each-->
    <ul>
      <li>1</li>
      <li>2</li>
      <li>3</li>
      <li>4</li>
    </ul>
    <!-- while-->
    <ul>
      <li>0</li>
      <li>1</li>
      <li>2</li>
      <li>3</li>
    </ul>
  </body>
</html>
```

## tag

每一行开头就第一个单词就是html tag.

若省略，则使用div为tag.