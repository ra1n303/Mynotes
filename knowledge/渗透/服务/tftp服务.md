# 介绍

TFTP全称Trivial File Transfer Protocol（简单文本传输协议）

它是一种轻量级的文本传输协议，基于UDP运行

与FTP相比，TFTP功能更简单，没有辅助的认证机制或目录浏览功能，主要用于快速传输小型文件



# 默认端口

69



# 特点

- 简单性：
  - 协议设计简洁，易于实现
  - 不支持用户认证（无用户名/密码验证）
    - 因此任何能连接到服务器的人都可以尝试请求文件
- 基于UDP：
  - TFTP使用UDP，因此速度快，但是不保证可靠性
- 限制：
  - 不支持目录列表，文件删除等高级功能。
    - 由于客户端无法直接列出服务器上的文件列表，必须知道确切的文件名才能下载



# 简单使用

```
tftp <ip>
```

连接tftp服务



```
binary
```

切换为二进制模式



```
put <filename>
```

上传文件



```
get <filename>
```

下载文件



```
quit
```

退出



```
tftp <ip> -c get <filename>
```

非交互模式下载文件

```
tftp <ip> -c put <filename>
```

非交互模式上传文件





# 攻击

## MSF

利用auxiliary/scanner/tftp/tftpbrute模块枚举TFTP文件

需要提供字典然后逐一尝试

默认字典：

```
/usr/share/metasploit-framework/data/wordlists/tftp.txt
```



