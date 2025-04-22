# 例题

![image-20250402201531211](./assets/image-20250402201531211.png)

分析源码：

get传参url

preg_match过滤

Bash

Nc

Wget

Ping

Ls

Cat

More

Less

Phpinfo

Base64

Echo

Php

Python

Mv

Cp

La

\-

\>

<

%

$

![image-20250402201538049](./assets/image-20250402201538049.png)

可以执行命令，但是无回显

可以利用tee配合管道符绕过

没有过滤 ' 

可以利用其绕过ls cat 

构造payload

```
?url=l''s / |tee 1.txt
```

接着查看1.txt

![image-20250402201614429](./assets/image-20250402201614429.png)



读取flllllaaaaaaggggggg

构造payload:

```
?url=tac /flllll''aaaaaaggggggg|tee 5.txt
```



查看5.txt

![image-20250402201628117](./assets/image-20250402201628117.png)