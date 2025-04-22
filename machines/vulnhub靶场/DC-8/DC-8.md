![img](./assets/wps1039.jpg)
![img](./assets/wps1040.jpg)
![img](./assets/wps1041.jpg)
![img](./assets/wps1042.jpg)
![img](./assets/wps1043.jpg)
![img](./assets/wps1044.jpg)
![img](./assets/wps1045.jpg)
![img](./assets/wps1046.jpg)
![img](./assets/wps1047.jpg)
![img](./assets/wps1048.jpg)
![img](./assets/wps1049.jpg)
![img](./assets/wps1050.jpg)
![img](./assets/wps1051.jpg)
![img](./assets/wps1052.jpg)
![img](./assets/wps1053.jpg)
![img](./assets/wps1054.jpg)

![img](./assets/wps1055.jpg) 

![img](./assets/wps1056.jpg) 

![img](./assets/wps1057.jpg) 

将php代码改为反弹shell脚本

本地监听，触发后成功反弹shell

 

（环境又崩了，重新搭建）

 

![img](./assets/wps1058.jpg) 

 

 

python转换终端

![img](./assets/wps1059.jpg) 

尝试传入linpeas脚本

 

本地开启http服务

![img](./assets/wps1060.jpg) 

![img](./assets/wps1061.jpg) 

切换到tmp目录

![img](./assets/wps1062.jpg) 

重新wget，成功

![img](./assets/wps1063.jpg) 

![img](./assets/wps1064.jpg) 

但是扫描后无关键内容

 

尝试sudo -l

但是需要提交www-data的密码，而我们没有，尝试suid提权

![img](./assets/wps1065.jpg) 

查找suid位

![img](./assets/wps1066.jpg) 

搜索相关漏洞

最后在exim中找到可用漏洞

![img](./assets/wps1067.jpg) 

查看exim4版本信息

![img](./assets/wps1068.jpg) 

 

找到两个可用来提权的漏洞

 

![img](./assets/wps1069.jpg) 

 

查看46996.sh

![img](./assets/wps1070.jpg) 

 

提供了两种提权方式

![img](./assets/wps1071.jpg) 

保存脚本

![img](./assets/wps1072.jpg) 

 

 

本地开启http服务

![img](./assets/wps1073.jpg) 

 

![img](./assets/wps1074.jpg) 

 

![img](./assets/wps1075.jpg) 

尝试第二种提权方式

![img](./assets/wps1076.jpg) 

成功

 

读取flag

 

![img](./assets/wps1077.jpg) 

 

 