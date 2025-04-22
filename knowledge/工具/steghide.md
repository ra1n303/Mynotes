### 介绍

可以在jpeg，bmp，wmv，au文件中隐写信息的软件

（不支持png）





### 基本用法

```
steghide extract -sf <filename> [-p passwd]
```

将隐藏信息从载体中分离出来

extract：提取

sf：待提取的文件名

-p：密码

可以不加密码参数，运行后会询问密码

若没有密码，询问密码时直接enter跳过



```
steghide info <filename>
```

查询载体有没有隐藏信息