## 复盘*

## 靶机地址

[Deathnote: 1 ~ VulnHub](https://www.vulnhub.com/entry/deathnote-1,739/)

![image-20250313173956404](./assets/image-20250313173956404.png)



## 信息收集

### 主机探活

```
arp-scan -l
```

![image-20250313174027447](./assets/image-20250313174027447.png)

确定目标主机ip

192.168.6.20



### nmap扫描目标主机开放端口

```
nmap -Pn -sS -p- -sV 192.168.6.20
```

![image-20250313174047446](./assets/image-20250313174047446.png)

开放了：

- 80 http
- 22 ssh



### 80端口

#### 访问192.168.6.20

发现会自动跳转

![image-20250313174139516](./assets/image-20250313174139516.png)



#### 添加本地hosts解析

```
192.168.6.20 deathnote.vuln
```

![image-20250313174147888](./assets/image-20250313174147888.png)

![image-20250313174157577](./assets/image-20250313174157577.png)

成功访问

发现一条关键信息：

![image-20250313174220035](./assets/image-20250313174220035.png)

```
my fav line is iamjustic3
我最爱的一句台词是  iamjustic3
```

可能是用户密码



点击hint

![image-20250313174251511](./assets/image-20250313174251511.png)

```
Find a notes.txt file on server
or 
SEE the L comment

找到一个notes.txt的文件
或者
看L的评论
```

L的评论应该就是

![image-20250313174329329](./assets/image-20250313174329329.png)



#### dirsearch 扫描目标网站

```
dirsearch -u 192.168.6.20
```

![image-20250313174348282](./assets/image-20250313174348282.png)

存在：

- robots.txt

- wordpress CMS

  

#### 访问robots.txt文件

![image-20250313174412070](./assets/image-20250313174412070.png)

存在important.jpg文件

访问

![image-20250313174418730](./assets/image-20250313174418730.png)

报错



#### 利用curl访问并查看内容

```
curl  http://192.168.6.20/important.jpg | cat   
```

![image-20250313174446289](./assets/image-20250313174446289.png)

 

提示用户名为user.txt



#### dirsearch扫描wordpress目录

```
dirsearch -u 192.168.6.20/wordpress
```

![image-20250313174502737](./assets/image-20250313174502737.png)

存在登录页面



尝试登录

![image-20250313174516457](./assets/image-20250313174516457.png)

![image-20250313174519381](./assets/image-20250313174519381.png)

 

提示用户不存在



#### 利用wp-scan工具爆破出用户

```
wpscan --url http://deathnote.vuln/wordpress/  -e u   
```

![image-20250313174535406](./assets/image-20250313174535406.png)

![image-20250313174538019](./assets/image-20250313174538019.png)

 

爆破出user:kira

 

#### 成功登录后台

![image-20250313174545470](./assets/image-20250313174545470.png)

 

#### media中发现了note.txt

![image-20250313174556229](./assets/image-20250313174556229.png)

指向了一个链接

![image-20250313174602027](./assets/image-20250313174602027.png)

一个密码字典



#### 下载字典

```
curl http://deathnote.vuln/wordpress/wp-content/uploads/2021/07/notes.txt >notes.txt
```

![image-20250313174622787](./assets/image-20250313174622787.png)



#### 查看uploads的其他内容

![image-20250313174629633](./assets/image-20250313174629633.png)

还有一个字典



#### 下载另一个字典

```
curl http://deathnote.vuln/wordpress/wp-content/uploads/2021/07/user.txt >notes.txt
```

![image-20250313174648149](./assets/image-20250313174648149.png)



### 22 端口

 

#### 尝试利用这两个字典爆破22端口的ssh服务

```
hydra -P notes.txt -L user.txt ssh://192.168.6.20  
```

![image-20250313174715304](./assets/image-20250313174715304.png)

成功

```
l
death4me
```



#### 登录ssh

```
ssh l@192.168.6.20
```

![image-20250313174748958](./assets/image-20250313174748958.png)





## 提权

#### 查看当前目录内容

```
ls
cat user.txt
```

![image-20250313174814410](./assets/image-20250313174814410.png)

存在user.txt



brainfuck加密

![image-20250313174835004](./assets/image-20250313174835004.png)



### 搜索含有kira的文件

```
find / -name "*kira*" 2>/dev/null
```

![image-20250313174857171](./assets/image-20250313174857171.png)



![image-20250313174903771](./assets/image-20250313174903771.png)

查看kira.txt

提示权限不足



 

### 查看 /opt/L/kira-case内容

```
 cat /opt/L/kira-case
```

![image-20250313174915437](./assets/image-20250313174915437.png)

提示查看fake-notebook-rule

```
cd /opt/L/fake-notebook-rule
```

![image-20250313174937994](./assets/image-20250313174937994.png)

 

存在两个文件

![image-20250313175001397](./assets/image-20250313175001397.png)

提示使用cyberchef解密

十六进制数据

![image-20250313175006821](./assets/image-20250313175006821.png)

得到base64

Base 64 解码得到

![image-20250313175014713](./assets/image-20250313175014713.png)

得到一个密码kiraisevil

### 查看当前系统有哪些用户

![image-20250313175020438](./assets/image-20250313175020438.png)

### 尝试登录kira

![image-20250313175027112](./assets/image-20250313175027112.png)

成功登录

 

访问当前用户下的目录信息