![img](./assets/wps407.jpg)

bp抓包无发现，dirsearch扫描目录无发现，查看源码无发现

nikto扫描目标网站

![img](./assets/wps408.jpg) 

可以用PUT方式提交

抓包

![img](./assets/wps409.jpg) 

 

将原始GET方式修改为PUT

创建a.php文件并写入一句话木马

访问a.php文件

空白表示创建成功

![img](./assets/wps410.jpg) 

 

get提交请求

/a.php?a=system("ls /");

![img](./assets/wps411.jpg) 

 

![img](./assets/wps412.jpg) 

 