题目中提示flag在另一张图里

在kali中利用binwalk分离图像

![image-20250422204348787](./assets/image-20250422204348787.png)

发现两个zlib文件，说明此图片中有隐藏文件

利用foremost也未分离出文件

![image-20250422204351846](./assets/image-20250422204351846.png)

利用tweakpng打开文件

![image-20250422204357853](./assets/image-20250422204357853.png)

发现其中有两个IDAT块，删除其中一个,f7预览图像

一个为

![image-20250422204402910](./assets/image-20250422204402910.png)

另一个即为flag

![image-20250422204407608](./assets/image-20250422204407608.png)

![image-20250422204409682](./assets/image-20250422204409682.png)