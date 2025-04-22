010打开后无异常

binwalk运行后得到

![image-20250422204512958](./assets/image-20250422204512958.png)

但是其中文件无法打开

 

 

tweakpng将idat块合并

binwalk运行后得到

![image-20250422204517093](./assets/image-20250422204517093.png)

![image-20250422204518280](./assets/image-20250422204518280.png)

其中D6E即为flag

 

 

也可以选择

zsteg跑一下

发现隐藏的数据，位置处于extractdata:0

![image-20250422204522142](./assets/image-20250422204522142.png)

接着用zsteg分离

![image-20250422204526759](./assets/image-20250422204526759.png)

 

接着binwalk分离即可

![image-20250422204532845](./assets/image-20250422204532845.png)