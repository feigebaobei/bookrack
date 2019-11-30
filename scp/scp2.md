# scp2

使用纯js写的基于ssh2的可以运行在任意系统的安全复制程序。

## install

```
npm i scp2 -g // 全局安装
scp2 -h // 使用命令行工具查看帮助
npm i scp2@ninja // 安装开发版本
```



## High level api

先执行`var client = require('scp2')`再执行复制操作。

```
// 下面是一起demo
client.scp('file.txt', 'admin:password@example.com:/home/admin/', function (err) {...})
client.scp('file.txt', 'admin:password@example.com:port:/home/admin/', function (err) {...})
client.scp('file.txt', {
	host: 'example.com',
	username: 'admin',
	password: 'password',
	path: '/home/admin'
}, funciton (err) {...})
client.scp('data/', 'admin:password@example.com:/home/admin/data/', function (err) {...})
client.scp('data/*.js', 'admin:password@example.com:/home/admin/data/', function (err) {...})
client.scp('admin:password@example.com:/home/admin/file.txt', './', function (err) {...})
client.scp({
	host: 'example.com',
	username: 'admin',
	password: 'password',
	path: '/home/admin/file.txt'
}, './', function (err) {...})
```



## Low level api

先执行`var Client = require('scps').Client`。它是高级client的Client实例。包括了高级api的scp.

```
// 下面是一些文件的demo
client.defaults({
	post: '22',
	host: 'example.com',
	username: 'admin',
	privateKey: '...',
	// password: '[password', (accepts password alse)
})
var client = new Client({port: 22})
// 下面是一些ssh的方法
sftp function (cb) -> cb(err, sftp)
close function ()
mkdir function (dir, [attr], cb) -> cb(err)
write function (options, cb) -> cb(err)
client.write({
	destination: '/home/admin/data/file.txt',
	content: 'hello world'
}, cb)
upload function (src, dest, cb) -> cb(err)
download function (src, dest, cb) -> cb(err)
```



## event

- connect
- ready
- Error(err)
- end
- close
- mkdir
- write(dir)
- Read(src)
- Transfer(buffer, uploaded, total)