# smb服务

## 介绍

smb（ServerMessage Block）是一种网络文件共享协议



## 默认端口

445



 

## SMB的常用工具

- smbclient
- enum4linux





### smbclient

smbclient：用于访问SMB共享资源

```
smbclient -L //<服务器ip> -U <用户名>
```

访问smb共享资源



```
smbclient //ip/share -U anonymous
```

```
smbclient -N //ip/share
```

匿名访问smb共享资源（如果共享文件夹允许匿名访问）



```
put filename
```

上传文件







### enum4linux



enum4linux：枚举smb服务信息

```
enu4linux -u <ip>
```

列出目标系统上的所有用户账户



```
enum4linux -a <ip>
```

-a 选项将运行所有默认扫描项目

包括用户，共享，木马策略，组等的枚举



```
enum4linux -S <目标IP>
```

-S 选项会列出目标系统上所有的共享文件夹。



```
enum4linux -v -a <目标IP>
```

-v   启用详细模式将输出更详细的扫描信息，帮助更好地理解扫描的结果



