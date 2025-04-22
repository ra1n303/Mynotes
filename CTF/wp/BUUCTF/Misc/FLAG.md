 

zsteg扫描图片

![image-20250327202604141](./assets/image-20250327202604141.png)

发现zip

 

binwalk和foremost提取失败

 

 

stegsolve打开

![image-20250327202610677](./assets/image-20250327202610677.png)

preview后发现存在压缩包

![image-20250327202617311](./assets/image-20250327202617311.png)

Sava bin保存为zip格式

7zip打开解压得到1（winrar会提示文件损坏）

![image-20250327202622685](./assets/image-20250327202622685.png)

010打开该文件

发现是ELF

 

放入kali执行

得到flag

![image-20250327202629208](./assets/image-20250327202629208.png)