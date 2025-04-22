zip文件

解压出两个图片

![image-20250327210302148](./assets/image-20250327210302148.png)

010打开boki.jpg

![image-20250327210305168](./assets/image-20250327210305168.png)

发现其中附加了一个zip文件

 

分离打开

真加密

尝试爆破

得到密码

![image-20250327210308975](./assets/image-20250327210308975.png)

得到

![image-20250327210311291](./assets/image-20250327210311291.png)

根据文档名推测是base加密

一次解密后

![image-20250327210319244](./assets/image-20250327210319244.png)

二次解密后

![image-20250327210323913](./assets/image-20250327210323913.png)

三次解密后

![image-20250327210328586](./assets/image-20250327210328586.png)

得到key：killerqueen

 

有key 有jpg

推测是f5或者outguess

Outguess

![image-20250327210334748](./assets/image-20250327210334748.png)

![image-20250327210337231](./assets/image-20250327210337231.png)