扫描mianju.jpg

![image-20250327203335220](./assets/image-20250327203335220.png)

foremost分离

得到zip文件

需要解压密码

010打开后发现是伪加密

修复后得到

![image-20250327203340979](./assets/image-20250327203340979.png)

![image-20250327203342499](./assets/image-20250327203342499.png)

虚拟机打开该文件报错

查阅资料后发现：

vmdk文件可以解压

kali中运行7z x

分割VMDK文件

得到四个文件

![image-20250327203402241](./assets/image-20250327203402241.png)

打开key_part_one

![image-20250327203409784](./assets/image-20250327203409784.png)

Brainfuck加密

解密得到flag

![image-20250327203416798](./assets/image-20250327203416798.png)

打开key_part_two

![image-20250327203421545](./assets/image-20250327203421545.png)

0ok！加密

解密后得到flag

![image-20250327203426032](./assets/image-20250327203426032.png)

拼接得到完整flag

 

flag{N7F5_AD5_i5_funny!}