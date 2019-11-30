# index

## introduce

scp = secure copy. 用来本地与远程之间拷贝文件。使用ssh认证方式。需要使用密码或口令。

## usage

`scp [-1245BCpqrv] [-c cipher] [F ssh_config] [-I identity_file] [-l limit] [-o ssh_option] [-P port] [-S program] [[user@]host1:] file1 […] [[suer@]host2:]file2`

把host1上的file1复制到host2上并成为file2.

## option

```
SCP 命令选项

-1 强制scp 用协议1

-2 强制scp 用协议2

-4 强制scp用IPV4的网址

-6 强制scp用IPV6的网址

-B 选择批处理模式（防止输入密码）

-C 允许压缩。 标注-C到ssh(1)来允许压缩

-c cipher选择cipher来加密数据传输。这个选项直接传递到ssh(1)

-F ssh_config设定一个可变动的用户配置给ssh.这个选项直接会被传递到ssh(1)

-i identity_file选择被RSA认证读取私有密码的文件。这个选项可以直接被传递到ssh(1)

-l limit限制传输带宽，也就是速度 用KByte/s的速度

-o ssh_option 可以把ssh_config中的配置格式传到ssh中。这种模式对于说明没有独立的scp文件中断符的scp很有帮助。关于选项的如下。而他们的值请参看ssh_config(5)

-P port 指定连接远程连接端口。注意这个选项需要写成大写的模式。因为-p已经早保留了次数和模式

-S program  指定一个加密程序。这个程序必须可读所有ssh(1)的选项。

-p 指定修改次数，连接次数，还有对于原文件的模式

-q 把进度参数关掉

-r 递归的复制整个文件夹

-S program  指定一个加密程序。这个程序必须可读所有ssh(1)的选项。

-V   冗余模式。 让 scp 和 ssh(1) 打印他们的排错信息， 这个在排错连接，认证，和配置中非常有用。
```

