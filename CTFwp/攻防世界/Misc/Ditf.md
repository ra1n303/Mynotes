png图片

010打开后发现尾部附加rar文件

同时crc提示报错

修复crc得到新的图片

![img](./assets/wps459.jpg) 

分离出rar文件

压缩包加密

利用密码StRe1izia解压出文件

![img](./assets/wps460.jpg) 

wireshark打开

搜索flag ctf无果

 

过滤http

![img](./assets/wps461.jpg) 

发现一个png文件

追踪流

![img](./assets/wps462.jpg) 

 

存在base64 加密

![img](./assets/wps463.jpg) 

解码得到flag

 

![img](./assets/wps464.jpg) 

 

 