tweakpng打开后发现idat块没有均匀铺开

![image-20250422204416929](./assets/image-20250422204416929.png)

将idat全选合并后放入kali中利用binwalk

![image-20250422204421023](./assets/image-20250422204421023.png)

得到数据块 41+3149

删除前八个idat块 保存后即可得到flag

![image-20250422204423880](./assets/image-20250422204423880.png)