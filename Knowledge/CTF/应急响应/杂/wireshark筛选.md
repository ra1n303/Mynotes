## SYN扫描

```
tcp.flags.syn == 1 and tcp.flags.ack == 0
```



## 源目ip

```
ip.src==192.168.18.133 && ip.dst==192.168.18.151
```



## 端口过滤

```
tcp.dstport=xx
```

