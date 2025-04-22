### 介绍

hydra是一个网络登录暴力破解工具，可以对多种服务（HTTP，SSH，FTP，SMTP等）进行密码爆破。





### 基本用法



#### 爆破SSH服务（指定字典）

```
hydra -L user.txt -P passwd.txt ssh://ip -s 22
```

-L：指定用户名字典

-P：指定密码字典

ssh：爆破的服务

ip：目标ip

-s：指定端口（默认使用协议标准端口）

利用用户名和密码字典爆破目标的ssh服务





### 利用单个用户名或密码爆破SSH服务

```
hydra -l user -P passwd.txt ssh://ip
```

-l：指定单个用户名

```
hydra -L user.txt -p passwd ssh://ip
```

-p 指定单个密码
