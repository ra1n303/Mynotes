rar.rar文件

解压得到办公文档.doc

打开后提示会乱码

![img](./assets/wps19.jpg) 

 

![img](./assets/wps20.jpg) 

开头显示PK，猜测是压缩包

010打开

![img](./assets/wps21.jpg) 

文件头 50 4B 03 04

zip文件，修改扩展名为zip

![img](./assets/wps22.jpg) 

将里面的内容解压

最终发现在Documents/1/Pages/1.fpage中存在flag

![img](./assets/wps23.jpg) 

将UnicodeString后的文字提取出来拼接得到flag

flag{xps?Oh,Go0d!}

 