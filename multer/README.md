# Introduction

multer 是node.js的上传文件的中间件。且只有处理`multipart/form-data`格式的上传。

## install

```
npm i multer
```

## usage

multer会有req对象里添加file/files/body对象。body对象里包含form的文本字段。file/files里包含form上传的文件。
基本用法：（不要忘了`enctype="multipart/form-data"`）

```
<form action="/profile" method="post" enctype="multipart/form-data">
    <input type="file" name="avatar">
    <input type="text" name="name">
</form>

var express = require('express')
var multer = require('multer')
var upload = multer({dest: 'uploads/'})
var app = express()
app.post('/profile', upload.single('avatar'), (req, res, next) => {
})
app.post('/profile', upload.array('photos', 12), (req, res, next) => {
    // req.files photo组成的数组。
    // req.body 文本字段
})
app.post('/profile', upload.fields([{name: 'avator', maxCount: 1}, {name: 'gallery', maxCount: 8}]), (req, res, next) => {
    // req.files['avator'] File
    // req.files['gallery'] Files
    // req.body 文本字段
})
```

若希望只传文本字段（不上传文件），则使用.none()方法。

```
var express = require('express')
var app = express()
var multer = require('multer')
var upload = multer()
app.post('/profile', upload.none(), (req, res, next) => {
    // req.body
})
```

## api
### file information

每一个File都包含下列信息

| key          | description                                        | note |
| ------------ | -------------------------------------------------- | ---- |
| fieldname    | 在form里field的name                                |      |
| originalname | 在用户的电脑上时文件的name                         |      |
| encoding     | file的编码类型                                     |      |
| mimetype     | mime type of the file                              |      |
| size         |                                                    |      |
| destination  | 把file保存在哪个目录                               |      |
| filename     | 在destination目录里时file的name.即保存为什么名称。 |      |
| path         | 上传的文件的全路径。                               |      |
| buffer       | 整个文件的buffer                                   |      |

### multer(options)

默认multer会重命名file.为了不发生命名冲突。也可以使用自定义的方法。

| key            | description                                                  |
| -------------- | ------------------------------------------------------------ |
| dest / storage | 这是最基本的一个选项。它说明把上传的文件保存在哪里。若省略则不会保存文件。 |
| fileFilter     | 控制接受哪种文件的方法                                       |
| limits         | 上传文件的限制                                               |
| preservePath   | 保存包含文件名的完整文件路径                                 |

一般只需要设置dest，即：`var upload = multer({dest: 'uploads/'})`

#### storage
##### DiskStorage

diskStoage引擎提供了把文件保存到硬盘的方法。

```
var storage = multer.diskStorage({
    destination: (req, file, cb) => { // 设置保存在哪个目录。可以是fu也可以string.若不设置则使用系统默认的临时目录。
        cb(null, '/tmp/my-uploads')
    },
    filename: (req, file, cb) => { // 设置保存文件时的文件名。
        cb(null, 'file.fieldname' + '-' + Date.now())
    }
})
var upload = multer({storage: storage})
```

##### MemoryStorage

内存存储引擎在内存里保存为buffer对象。这个方法没有任何options.

```
var storage = multer.memoryStorage()
var upload = multer({storage: storage})
```

#### fileFilter

设置保存哪些文件，跳过哪些文件。

```
function fileFilter (req, file, cb) {
    // 跳过
    cb(null, false)
    // 保存
    cb(null, true)
    // 抛出错误
    cb(new Error('string'))
}
```

#### limits

| key           | description                                          | default |
| ------------- | ---------------------------------------------------- | ------- |
| fieldNameSize | 字段名字的最大长度                                   |         |
| fieldSize     | 单个field的最大值                                    |         |
| fields        | 非文件field的最大数量                                |         |
| fileSize      | 上传多个文件时，文件的最大值（b）                    |         |
| files         | 上传多个文件时，文件的最大数量                       |         |
| parts         | 上传多个文件时，part传输的最大数量（fields + files） |         |
| headerPairs   | 在上传多个文件时，key-value的最大组数                |         |

### .single(fieldname)

使用fieldname接收一个文件。文件保存在`req.file`

### .array(fieldname[, maxCount])

使用fieldname接收一个文件组成的数组。文件的数量最大由maxCount控制。文件保存在`req.files`

### .fields(fields)

按照指定的form的field组成的file.它是一个由数组组成的对象，并保存在`req.files`。
field应该包括name、maxCount.

```
[
    {name: 'avator', maxCount: 1},
    {name: 'gallery', maxCount: 8}
]
```

### .none()

只接收文本字段。若上传文件，则报`LIMIT_UNEXPECTED_FILE`错误。

### .any()

可以接收任何文件的文件。并保存在`req.files`。

提示：应该限制用户上传的文件。不要在全局使用multer，防止恶意用户上传到不可预期的路由上。只在预期的路由上使用multer.

## Error handling

若遇到error会抛出错误到express。
若要自定义处理错误的方法。可以使用`multer.MulterError`捕捉错误。

```
app.post('/profile', (req, res) {
    upload(req, res, (err) => {
        if (err instanceof multer.MulterError) {
            ...
            // 这是上传文件时出现的错误。
        } else {
            ...
            // 这不是上传文件时出现的错误。
        }
    })
})
```

## Custom storage engine