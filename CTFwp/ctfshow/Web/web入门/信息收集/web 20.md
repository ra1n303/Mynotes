根据标题

mdb文件是早期asp+access构架的数据库文件，文件泄露相当于数据库被脱裤了。

 

可以得知是mdb文件泄露

 

 

Dirsearch 扫描网站目录

存在/db

![image-20250309094210555](./assets/image-20250309094210555.png)

扫描/db

![image-20250309094214662](./assets/image-20250309094214662.png)

存在/db/db.mdb

 

访问 /db/db.mdb

得到db.mdb下载文件

文本打开后全局搜索flag得到flag

![image-20250309094224916](./assets/image-20250309094224916.png)