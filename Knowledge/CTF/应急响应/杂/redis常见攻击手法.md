## 未授权访问





## 写入webshell

需要知道web目录的路径，具有文件读写和增删改查权限



## 写入公钥

服务器开启了ssh服务并且具备往root用户的.ssh目录写入文件的权力



## 定时任务反弹shell

需要root用户开启的redis服务，且存在cron服务



## 主从复制



```
find / -name "redis.conf" 
```

查找redis配置文件
