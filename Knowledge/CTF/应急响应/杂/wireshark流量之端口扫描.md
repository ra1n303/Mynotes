端口扫描中有SYN扫描，FIN扫描和UDP扫描





## SYN扫描  

SYN 扫描是最为常见的扫描方式，扫描方会发送 SYN 数据包来试探目标端口是否开放  

```
tcp.flags.syn == 1 and tcp.flags.ack == 0 
```

 
此规则能筛选出所有 SYN 数据包（也就是那些 SYN 标志位为 1 且 ACK 标志位为 0 的 TCP 数据包）



## fin扫描

fin一般是连接关闭的过程，而syn是建立连接的过程，他们都属于TCP协议，只不过对应的不一样  

```
tcp.flags.fin == 1
```





## UDP扫描

UDP连接更不用说了，一般DDOS都用UDP，相比于TCP，UDP的成本会更低，因为UDP是简单封包后直接发送，没有所谓的三次握手四次挥手这一说，稳定性低于TCP  
筛选规则：UDP