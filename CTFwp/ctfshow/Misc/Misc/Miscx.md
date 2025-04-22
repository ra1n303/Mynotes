得到misc2压缩包

里面有三个文件

![image-20250422205239762](./assets/image-20250422205239762.png)

其中misc1.zip可以直接解压

另外两个要密码

进入misc1.zip

其中misc.png可以直接解压

music.doc需要密码

打开misc.png

![image-20250422205246151](./assets/image-20250422205246151.png)

爆破修复宽高后无结果

推测2020为music.doc的解压密码

成功解压后打开文件后是一串音符

![image-20250422205251171](./assets/image-20250422205251171.png)

进行音符解密

![image-20250422205255730](./assets/image-20250422205255730.png)

怀疑是base64编码

不对

（rabbit编码）

（misc2注释中说明）

![image-20250422205302285](./assets/image-20250422205302285.png)

进行rabbit解码

密码为2020

![image-20250422205306748](./assets/image-20250422205306748.png)

将解密出的内容作为解压密码可以解密出hint.txt

![image-20250422205312957](./assets/image-20250422205312957.png)

将文本中的内容进行六次base64解码(每次解码后删除%3d)

得到

![image-20250422205318605](./assets/image-20250422205318605.png)

进行url解码

![image-20250422205325491](./assets/image-20250422205325491.png)

使用hello 2020！解压flag.txt

得到flag









