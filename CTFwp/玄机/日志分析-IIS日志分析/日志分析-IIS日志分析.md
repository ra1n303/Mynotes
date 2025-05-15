## IIS日志基础

默认路径

```
%SystemDrive%\inetpub\logs\LogFiles\
```





```
1.phpstudy-2018站点日志.(.log文件)所在路径，提供绝对路径
2.系统web日志中状态码为200请求的数量是多少
3.系统web日志中出现了多少种请求方法
4.存在文件上传漏洞的路径是什么（flag{/xxxxx/xxxxx/xxxxxx.xxx})
5.攻击者上传并且利⽤成功的webshell的⽂件名是什么
```





访问

```
%SystemDrive%\inetpub\logs\LogFiles\
```

![image-20250515163442173](./assets/image-20250515163442173.png)

![image-20250515163448514](./assets/image-20250515163448514.png)

其中C:\inetpub\logs\LogFiles\W3SVC2\u_ex250220.log

存在大量数据

保存到本地

简单查看

![image-20250515163625970](./assets/image-20250515163625970.png)

提取响应状态码

```
cat u_ex250220.log | awk '{print $12}' | sort -nr | uniq -c
```

![image-20250515163703703](./assets/image-20250515163703703.png)

200的为2315次

提取请求方式

```
cat u_ex250220.log | awk '{print $4}' | sort -nr | uniq
```

![image-20250515164138765](./assets/image-20250515164138765.png)

```
TRACE
PUT
POST
OPTIONS
HEAD
GET
DELETE
```

七种



存在文件上传漏洞的路径，文件上传，首先过滤upload，然后POST方式，最后再利用-v筛选404状态码

```
cat u_ex250220.log | grep upload | grep POST | grep -v 404
```

![image-20250515164228801](./assets/image-20250515164228801.png)

```
/emlog/admin/plugin.php
```



d盾查杀

![image-20250515164709556](./assets/image-20250515164709556.png)

两个木马文件