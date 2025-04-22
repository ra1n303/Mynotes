靶机地址：

[DC: 7 ~ VulnHub](https://www.vulnhub.com/entry/dc-7,356/)

![img](./assets/wps984.jpg) 

靶机ip：192.168.56.127

nmap扫描靶机开放端口

 

![img](./assets/wps985.jpg) 

 

 

22：ssh服务

80：http服务

访问192.168.56.127

![img](./assets/wps986.jpg) 

只有一个搜索框，并提示我们跳出框框

 

![img](./assets/wps987.jpg) 

搜索框随便输入内容

![img](./assets/wps988.jpg) 

 

利用sqlmap测试是否存在sql注入

![img](./assets/wps989.jpg) 

 

不存在SQL注入漏洞

 

dirsearch扫描目录

![img](./assets/wps990.jpg) 

 

![img](./assets/wps991.jpg) 

 

![img](./assets/wps992.jpg) 

 

![img](./assets/wps993.jpg) 

 

![img](./assets/wps994.jpg) 

 

![img](./assets/wps995.jpg) 

 

得到五个关键目录

分别访问

 

 

只有一处关键的login页面

![img](./assets/wps996.jpg) 

 

 

whatweb识别网站指纹信息

![img](./assets/wps997.jpg) 

 

 

 

wappalyzer识别

![img](./assets/wps998.jpg) 

得知是Drupal 8 CMS

 

搜索相关漏洞

![img](./assets/wps999.jpg) 

 

 

 

![img](./assets/wps1000.jpg) 

但是经过尝试后都不行

![img](./assets/wps1001.jpg) 

 

![img](./assets/wps1002.jpg) 

 

 

 

回到主页

![img](./assets/wps1003.jpg) 

 

 

左下角有一个id，搜索

![img](./assets/wps1004.jpg) 

 

找到一个github用户

查看他的项目内容

![img](./assets/wps1005.jpg) 

 

 

![img](./assets/wps1006.jpg) 

 

 

查看config.php

![img](./assets/wps1007.jpg) 

 

 

存在用户名和密码

 

尝试登录网站，失败

![img](./assets/wps1008.jpg) 

 

 

尝试登录ssh

成功

![img](./assets/wps1009.jpg) 

![img](./assets/wps1010.jpg) 

有一个mbox和backups文件

mbox是linux中的邮件存储箱

上传linpeas.sh

本地开启http服务

![img](./assets/wps1011.jpg) 

 

![img](./assets/wps1012.jpg) 

执行

![img](./assets/wps1013.jpg) 

![img](./assets/wps1014.jpg)
![img](./assets/wps1015.jpg)

到这里我们理一下思路：

首先我们目前是dc7user用户

而我们可以利用drush修改admin密码登录网站后台，

尝试利用后台漏洞弹回一个www-data用户

www-data可以修改backups.sh内容

而backups.sh属于root用户

因此，如果我们往backups.sh中添加一条反弹shell的命令

那么我们就可以收到root用户发出的shell

即取得了root权限

 

切换到网站目录

![img](./assets/wps1016.jpg) 

 

利用drush修改admin密码

![img](./assets/wps1017.jpg) 

成功登录网站后台

![img](./assets/wps1018.jpg) 

尝试模板注入

![img](./assets/wps1019.jpg) 

 

![img](./assets/wps1020.jpg) 

 

 

但是不支持解析为php语言

插件页面

![img](./assets/wps1021.jpg) 

点击下载新的模块

可以访问drupal官网下载模块然后在此处导入

![img](./assets/wps1022.jpg) 

 

访问drupal.org官网

搜索php，找到php解释器

![img](./assets/wps1023.jpg) 

最下面查看所有版本

![img](./assets/wps1024.jpg) 

 

我选择的是这一个

 

![img](./assets/wps1025.jpg) 

然后下载

![img](./assets/wps1026.jpg) 

 

然后将下载的压缩包直接导入

![img](./assets/wps1027.jpg) 

 

导入成功

![img](./assets/wps1028.jpg) 

 

接着开启该模块

![img](./assets/wps1029.jpg) 

 

 

 

![img](./assets/wps1030.jpg) 

 

返回content页面

![img](./assets/wps1031.jpg) 

 

可以解析php代码

 

写入反弹shell脚本

![img](./assets/wps1032.jpg) 

本地开启监听

![img](./assets/wps1033.jpg) 

 

 

返回content页面，点击触发该代码

![img](./assets/wps1034.jpg) 

成功弹回shell

![img](./assets/wps1035.jpg) 

转换终端

![img](./assets/wps1036.jpg) 

进入/opt/scripts目录下，往backups.sh追加反弹shell任务，然后本地开启监听

等待定时任务触发后，会得到root弹回的shell

![img](./assets/wps1037.jpg) 

![img](./assets/wps1038.jpg) 

注意点：

虽然backups.sh这个shell脚本我们也可以执行，但是如果写入了反弹shell的脚本，那么本地接收到的终端是执行者的

即如果是www-data用户执行该脚本，那么本地接收到的终端就是www-data用户的，而我们要获取的是root用户

即我们要等到定时任务，每十五分钟触发一次拿到root用户的shell，得到root用户权限

 