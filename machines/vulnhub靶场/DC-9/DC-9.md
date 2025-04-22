![img](./assets/wps1078.jpg)

 

 

 

 

主机探活：

arp-scan -l --interface=eth1

![img](./assets/wps1079.jpg) 

 

 

确定靶机ip：192.168.56.119

 

 

 

nmap扫描端口信息

这里我们使用CTFEnum

![img](./assets/wps1080.jpg) 

 

 

![img](./assets/wps1081.jpg) 

可以看到开放了

21

22

80

但是22端口显示filtered，即过滤状态，无法进行暴力破解

 

访问192.168.56.119

 

![img](./assets/wps1082.jpg) 

 

![img](./assets/wps1083.jpg) 

 

 

关键点在于一个搜索框，一个登录页面

 

 

dirsesarch进行扫描目录

![img](./assets/wps1084.jpg) 

 

 

无关键信息

 

Wappalyzer进行指纹识别，依旧无关键信息

![img](./assets/wps1085.jpg) 

 

 

whatweb也是无信息

![img](./assets/wps1086.jpg) 

 

无法识别该靶机的CMS

 

 

 

尝试从页面入手

 

其中search页面

![img](./assets/wps1087.jpg) 

 

![img](./assets/wps1088.jpg) 

 

尝试sql注入

 

抓包

可以看到为post请求，post到results.php参数为search

![img](./assets/wps1089.jpg) 

 

 

利用sqlmap

![img](./assets/wps1090.jpg) 

 

![img](./assets/wps1091.jpg) 

 

![img](./assets/wps1092.jpg) 

 

 

可以看到search是一个注入点，且数据库为mysql

 

 

或者直接将抓包内容保存到文件中，利用sqlmap读取文件内容自动检测

![img](./assets/wps1093.jpg) 

 

 

![img](./assets/wps1094.jpg) 

 

 

查找数据库

![img](./assets/wps1095.jpg) 

 

 

![img](./assets/wps1096.jpg) 

 

 

三个数据库

Staff

和users

 

查看users数据库下的表名

![img](./assets/wps1097.jpg) 

 

![img](./assets/wps1098.jpg) 

 

 

存在UserDetails表

 

 

查看该表中的所有字段

![img](./assets/wps1099.jpg) 

 

 

 

查看该表中的username和password的值

![img](./assets/wps1100.jpg) 

 

![img](./assets/wps1101.jpg) 

 

 

 

 

查看

![img](./assets/wps1102.jpg) 

 

 

处理数据

将用户名保存到users.txt

将密码保存到password.txt

 

![img](./assets/wps1103.jpg) 

 

 

![img](./assets/wps1104.jpg) 

 

 

 

 

继续利用sqlmap跑Staff数据库

![img](./assets/wps1105.jpg) 

 

![img](./assets/wps1106.jpg) 

 

得到Users表

 

 

查找Users表中的字段

![img](./assets/wps1107.jpg) 

 

![img](./assets/wps1108.jpg) 

 

 

查询username和password的字段

![img](./assets/wps1109.jpg) 

 

 

得到admin的账号和密码

![img](./assets/wps1110.jpg) 

 

 

![img](./assets/wps1111.jpg) 

 

 

 

 

返回登录页面

成功登录

![img](./assets/wps1112.jpg) 

 

 

下面提示File does not exist，文件不存在

判断可能存在文件读取，尝试bp爆破参数及路径

 

尝试直接?file=../../../../../../etc/passwd

![img](./assets/wps1113.jpg) 

 

 

 

成功读取/etc/passwd

 

即参数为file

 

 

尝试读取/etc/shadow

![img](./assets/wps1114.jpg) 

失败

 

 

 

 

结合22ssh端口的filter被过滤

 

查看/proc/sched_debug

这个文件可以读取linux系统中的任务调度情况

 

![img](./assets/wps1115.jpg) 

 

 

 

将内容保存到2.txt文件

![img](./assets/wps1116.jpg) 

 

过滤出所有服务

可以看到存在knockd服务

![img](./assets/wps1117.jpg) 

 

 

读取该服务的配置文件/etc/knockd.conf

![img](./assets/wps1118.jpg) 

 

 

存在三个端口7469，8457，9842

向三个端口发送SYN请求（敲门）

 

可以使用knock发送请求，也可以用nc

 

（我这里复现的时候有问题，重装靶机后可以看到22端口开放）

（ip由192.168.56.119变为192.168.56.120）

![img](./assets/wps1119.jpg) 

 

22端口开放，利用hydra爆破ssh账号密码

 

![img](./assets/wps1120.jpg) 

 

 

![img](./assets/wps1121.jpg) 

 

 

前两个用户登陆后无关键信息

第三个用户登陆后发现新的密码

![img](./assets/wps1122.jpg) 

 

 

将新密码追加到password.txt文件

![img](./assets/wps1123.jpg) 

 

重新爆破ssh服务

![img](./assets/wps1124.jpg) 

 

爆破出新的用户名fredf

登录

 

查看sudo -l

![img](./assets/wps1125.jpg) 

 

提示可以以root身份执行/opt/devstuff/dist/test/test文件

 

尝试执行

![img](./assets/wps1126.jpg) 

 

 

查找test.py

![img](./assets/wps1127.jpg) 

 

 

查看test.py

 

![img](./assets/wps1128.jpg) 

 

 

 

即读取第一个文件的内容，然后追加到第二个文件里

 

因此我们可以执行这一条命令，将fredf  ALL=(ALL:ALL) ALL追加到/etc/sudoers文件中

即fredf可以利用sudo执行任何命令

![img](./assets/wps1129.jpg) 

 

![img](./assets/wps1130.jpg) 

 

![img](./assets/wps1131.jpg) 

 

 