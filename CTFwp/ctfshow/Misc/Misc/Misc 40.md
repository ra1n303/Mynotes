打开压缩包

![image-20250422205441440](./assets/image-20250422205441440.png)

其中只有svega.wav需要解压密码

将其中三个解压

 

Conversion.txt打开后显示

![image-20250422205449707](./assets/image-20250422205449707.png)

推测是二进制转十进制

![image-20250422205453401](./assets/image-20250422205453401.png)

得到202013

利用其解压wav文件提示错误

 

打开二维码文件

提示flag不在这里

![image-20250422205501390](./assets/image-20250422205501390.png)

010打开

![image-20250422205506714](./assets/image-20250422205506714.png)

发现brainFuck加密



![image-20250422205511795](./assets/image-20250422205511795.png)

解密后得到社会主义核心价值观密文

将其解密得到123456

![image-20250422205517933](./assets/image-20250422205517933.png)



利用123456解压wav提示错误

 

利用MP3Stego提取mp3文件（需要进入MP3Stego文件下的MP3Stego中运行Decode.exe）

![image-20250422205522517](./assets/image-20250422205522517.png)



![image-20250422205525317](./assets/image-20250422205525317.png)



![image-20250422205527614](./assets/image-20250422205527614.png)得到文本

![image-20250422205534155](./assets/image-20250422205534155.png)



提示静默之眼

silenteye软件

且密码为abc123

利用abc123将svega.wav文件解压

并用silenteye将其打开

decode解码

将参数：

Sound quality 设置为高

Type AES128

Key为未使用的202013

![image-20250422205539620](./assets/image-20250422205539620.png)



成功得到flag

![image-20250422205545329](./assets/image-20250422205545329.png)



