![img](./assets/wps512.jpg)

 

进入靶场

发现只有报表中心可以正常访问

![img](./assets/wps513.jpg) 

且有一个传参 id

burp爆破

payload设置为id值

 

![img](./assets/wps514.jpg) 

数值类型

1--1000

![img](./assets/wps515.jpg) 

 

发现一处长度和别的不一样

 

![img](./assets/wps516.jpg) 

 

访问得到flag

![img](./assets/wps517.jpg) 

 

 