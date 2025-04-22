![img](./assets/wps398.jpg)

 

![img](./assets/wps399.jpg) 

 

分析源码

首先：

![img](./assets/wps400.jpg) 

get方式传参num

判断num的长度要小于3，且num又要大于999999999

科学计数法

num=9e9

 

接着

![img](./assets/wps401.jpg) 

get方式传参str

将str中的NSSCTF替换为空

且判断str仍为NSSCTF

双写绕过

str=NSSNSSCTFCTF

 

 

![img](./assets/wps402.jpg) 

post方式传入md5_1和md5_2

如果md5_1和md5_1不相等且它们两个的md5值相同

md5_1=QNKCDZO

md5_2=240610708

 

得到flag

![img](./assets/wps403.jpg) 

 

 