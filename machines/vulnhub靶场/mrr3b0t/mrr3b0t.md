![img](./assets/wps1132.jpg)

arp-scan主机探活

![img](./assets/wps1133.jpg) 

确定靶机ip，192.168.56.125

 

 

利用CTFEnum扫描

![img](./assets/wps1134.jpg) 

 

 

确定靶机开放端口

![img](./assets/wps1135.jpg) 

 

 

关键22 和 80

 

 

访问192.168.56.125

![img](./assets/wps1136.jpg) 

 

web站点

whatweb识别指纹信息

![img](./assets/wps1137.jpg) 

 

 

dirsearch扫描目录

![img](./assets/wps1138.jpg) 

 

![img](./assets/wps1139.jpg) 

 

后台页面

![img](./assets/wps1140.jpg) 

其源码中存在提示

![img](./assets/wps1141.jpg) 

 

提示找一下目录

![img](./assets/wps1142.jpg) 

根据扫描的结果逐个查看

最终在images文件夹下发现flag.txt.txt

![img](./assets/wps1143.jpg) 

 

提示

![img](./assets/wps1144.jpg) 

 

![img](./assets/wps1145.jpg) 

找到hidden.png

保存该png图片

随波逐流

![img](./assets/wps1146.jpg) 

 

 

利用zsteg扫描

![img](./assets/wps1147.jpg) 

 

 

或者利用stegsolve

![img](./assets/wps1148.jpg) 

提示使用mrrobot作为user然后爆破密码

 

 

抓包爆破

![img](./assets/wps1149.jpg) 

 

Top3000字典

![img](./assets/wps1150.jpg) 

爆破出密码为secret

 

上传php脚本反弹shell脚本

上传失败，提示只能上传图片

![img](./assets/wps1151.jpg) 

 

![img](./assets/wps1152.jpg) 

尝试绕过，发现，它只会检测文件名中是否含义jpg,jpeg,gif等

修改文件名为1.png.php

![img](./assets/wps1153.jpg) 

成功上传，但是没有回显上传路径

抓包同样没发现路径

![img](./assets/wps1154.jpg) 

尝试upload  uploads

![img](./assets/wps1155.jpg) 

![img](./assets/wps1156.jpg) 

不存在

在当前目录发现上传的文件

![img](./assets/wps1157.jpg) 

 

本地开启监听，重新访问

![img](./assets/wps1158.jpg) 

成功反弹shell

 

利用python转换终端，提示没有python

![img](./assets/wps1159.jpg) 

 

利用 /usr/bin/script -qc /bin/bash /dev/null转换

![img](./assets/wps1160.jpg) 

 

执行whoami

![img](./assets/wps1161.jpg) 

 

www-data用户

 

 

查看/etc/passwd

![img](./assets/wps1162.jpg) 

 

存在exploiter用户

进入其家目录

发现有个隐藏文件 .sudo_as_admin_successful，但是无内容

![img](./assets/wps1163.jpg) 

 

上传linpeas脚本

 

本地开启http服务

 

![img](./assets/wps1164.jpg) 

 

靶机中使用wget下载

![img](./assets/wps1165.jpg) 

 

拒绝写入，切换到tmp文件夹

 

![img](./assets/wps1166.jpg) 

 

赋予执行权限

![img](./assets/wps1167.jpg) 

 

运行

 

 

扫描后无结果

 

查找suid位

![img](./assets/wps1168.jpg) 

 

搜索可用漏洞

 

无结果

 

进入网站目录

查看/var/www

 

存在bf文件夹

![img](./assets/wps1169.jpg) 

存在buffer文件

![img](./assets/wps1170.jpg) 

执行

要求输入一个密码

随便输入内容提示错误

![img](./assets/wps1171.jpg) 

 

 

strings查看

 

![img](./assets/wps1172.jpg) 

 

![img](./assets/wps1173.jpg) 

得到Password@123

或者利用nc将文件传回本地

![img](./assets/wps1174.jpg) 

 

利用ida查看

![img](./assets/wps1175.jpg) 

 

即如果传入MrRbt121，则回显Password@123

 

 

 

或者利用ltrace

![img](./assets/wps1176.jpg) 

 

随便传入参数后会有MrRb0t121关键字

 

 

 

得到Password@123

尝试切换为exploiter用户

![img](./assets/wps1177.jpg) 

 

成功切换

执行id

 

![img](./assets/wps1178.jpg) 

![img](./assets/wps1179.jpg)
![img](./assets/wps1180.jpg)

然后开启本地http服务，将tar.gz文件上传到靶机

![img](./assets/wps1181.jpg) 

 

 

![img](./assets/wps1182.jpg) 

将该文件添加进LXD

lxc image import ./alpine-v3.13-x86_64-20210218_0139.tar.gz --alias myimage

![img](./assets/wps1183.jpg) 

 

查看镜像列表

![img](./assets/wps1184.jpg) 

 

初始化lxc（选项默认）

 

lxc init

![img](./assets/wps1185.jpg) 

 

lxc init myimage ignite -c security.privileged=true

![img](./assets/wps1186.jpg) 

lxc config device add ignite mydevice disk source=/ path=/mnt/root recursive=true

![img](./assets/wps1187.jpg) 

 lxc start ignite

![img](./assets/wps1188.jpg) 

lxc exec ignite /bin/sh

![img](./assets/wps1189.jpg) 

执行id

![img](./assets/wps1190.jpg) 

 

提权成功

读取/mnt/root/root目录下的flag.txt.txt

![img](./assets/wps1191.jpg) 

 

![img](./assets/wps1192.jpg) 

关于LXD提权参考

[看我如何利用LXD实现权限提升 - FreeBuf网络安全行业门户](https://www.freebuf.com/articles/network/216803.html)

 