# ftp

## 介绍

FTP（File Transfer Protocol，文件传输协议）是一种用于在网络上计算机之间传输文件的标准协议。它于 1971 年由 Abhay Bhushan 开发，旨在实现文件的上传、下载和管理。



## 默认端口

21



## 配置文件

 vsftpd.conf



## 连接FTP服务器

```
ftp <ip>
```



## 简单命令

```
binary
```

登录成功后，利用binary切换到二进制模式，否则下载的可执行文件可能会损坏

```
prompt
```

执行prompt可以将交互式的提示关闭，这样不用每次下载都需要确认

```
mget *.txt 
```

批量下载

```
get <filename>
```

单个下载

```
put <filename>
```

上传

```
rmdir <dir>
```

删除目录

```
mkdir <dir>
```

创建目录

```
delete <filename>
```

删除文件

```
bye
```

退出



## 文件类型检测

下载文件后，可以执行

```
file filename
```

file命令通过分析文件的魔法数字和内容，检测文件类型，从而进行下一步操作





## 匿名登录（anonymous）

FTP（文件传输协议）默认支持匿名登陆

指定用户名为anonymous，密码为空，可以尝试匿名登录

改 FTP 配置（像）

```
anonymous_enable=NO
```

如果匿名登陆被禁用，可以尝试破解弱密码（hydra）



