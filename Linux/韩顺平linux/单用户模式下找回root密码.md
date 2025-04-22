单用户模式下找回root密码

 

![img](./assets/wps877.jpg) 

启动系统，开机界面，在界面中按 "e"进入编辑页面

 

![img](./assets/wps878.jpg) 

 

 

![img](./assets/wps879.jpg) 

在该行末尾输入：init=/bin/sh

 

![img](./assets/wps880.jpg) 

接着快捷键 ctrl+x进入单用户模式

 

![img](./assets/wps881.jpg) 

在光标闪烁处输入：mount -o remount,rw / 

 

![img](./assets/wps882.jpg) 

 

在新的一行输入passwd，回车后输入密码，修改完成后，会显示passwd….

![img](./assets/wps883.jpg) 

 

接着输入 touch /.autorelabel

![img](./assets/wps884.jpg) 

 

最后输入 exec /sbin/init

![img](./assets/wps885.jpg) 

 