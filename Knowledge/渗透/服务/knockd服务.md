# knockd

## 介绍 

端口敲门（端口碰撞）

knockd服务，该服务通过动态添加iptables规则来隐藏系统开启的服务，使用自定义的一系列序号来“敲门”，是系统开启需要访问的服务端口，才能对外访问。不使用时，再使用自定义的序列号来“关门”，将端口关闭，不对外监听，进一步实现了服务和系统的安全性。



## 配置文件

/etc/knockd.conf




### 配置文件说明

```
[openSSH]

　　　　sequence = 7000,8000,9000 //定义敲门顺序号

　　　　seq_timeout = 30 //设置超时时间

　　　　command = /sbin/iptables -D INPUT -p tcp --dport 22 -j DROP && /sbin/iptables -A INPUT -s [允许远程的IP] -p tcp --dport 22 -j ACCEPT && /sbin/iptables -A INPUT -p tcp --dport 22 -j DROP

　　　　tcpflags = syn

 

[closeSSH]

　　　　sequence = 9000,8000,7000 //定义关门顺序号

　　　　seq_timeout = 30 //设置超时时间

　　　　command = /sbin/iptables -D INPUT -s [允许远程的IP] -p tcp --dport 22 -j ACCEPT

　　　　tcpflags = syn


```



即按顺序向目标主机的7000，8000，9000端口发送syn请求包

即可实现开放端口

 

即按顺序向目标主机的9000，8000，7000端口发送syn请求包

即可实现关闭端口





## 敲门

使用knock敲门，即可使目标开放相应端口

```
knock <ip> <port1> <port2>
```

