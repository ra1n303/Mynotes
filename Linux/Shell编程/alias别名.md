格式

alias 'xxx=命令'

![image-20250310081654220](./assets/image-20250310081654220.png)

![image-20250310081656266](./assets/image-20250310081656266.png)

此时输入cls即可实现clear的效果

 

但是其是临时生效

所有的别名会在重启后失效

可以更改.bashrc（.zshrc）文件修改使其一直生效

![image-20250310081702900](./assets/image-20250310081702900.png)

![image-20250310081704889](./assets/image-20250310081704889.png)

然后source加载该配置文件

![image-20250310081710708](./assets/image-20250310081710708.png)

source= .

因此也可以写为

. /home/ra1n3/.zshrc

![image-20250310081716754](./assets/image-20250310081716754.png)