得到png图片

010打开无异常

exiftool无关键信息

扫描无异常

放进stegsolve中查看

发现有张二维码

![img](./assets/wps52.jpg) 

 

扫描后得到十六进制数

![img](./assets/wps53.jpg) 

 

 

03 F3 0D 0A

查了一下是pyc文件格式

010导入十六进制数

![img](./assets/wps54.jpg) 

 

![img](./assets/wps55.jpg) 

保存为pyc文件格式

放入kali中利用uncompyle6反编译得到py文件

![img](./assets/wps56.jpg) 

运行报错

![img](./assets/wps57.jpg) 

修改代码后得到flag

![img](./assets/wps58.jpg) 

 

 