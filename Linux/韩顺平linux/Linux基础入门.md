# 目录结构

基本介绍

文件系统采用级层的树状目录解耦给，在此结构中的最上层是 "/" 根目录，,然后在此目录下再创建其他的目录。

在linux世界里，一切皆文件

文件系统采用级层的树状目录结构，在此结构中的最上层是 "/" 根目录，,然后在此目录下再创建其他的目录。

在linux世界里，一切皆文件

 

![img](./assets/wps729.jpg) 

![img](./assets/wps730.jpg) 

![img](./assets/wps731.jpg) 

![img](./assets/wps732.jpg) 

 

#  vi和vim文本编辑器

![img](./assets/wps733.jpg)

![img](./assets/wps734.jpg) 

拷贝当前行 yy

拷贝当前行向下的5行 5yy

粘贴 p

删除当前行 dd

删除当前行向下的5行 5dd

查找关键词 命令行下 /关键字 回车查找 输入n就是查找下一个

设置行号 :set nu

取消设置行号 :set nonu

定位到文档首行 gg

定位到文档末行 G

撤销动作 u

跳转到某一行 数字（看不到） 然后shift+g 

![img](./assets/wps735.jpg) 

 

#  关机&重启命令

 

![img](./assets/wps736.jpg) 

-h halt 停止

-r  reboot

![img](./assets/wps737.jpg) 

 

#  用户登录和注销

 

![img](./assets/wps738.jpg) 

 

![img](./assets/wps739.jpg) 

 

# 用户管理

![img](./assets/wps740.jpg)

 

##  添加用户

![img](./assets/wps741.jpg)

![img](./assets/wps742.jpg) 

![img](./assets/wps743.jpg) 

 

![img](./assets/wps744.jpg) 

##  指定/修改密码

 

![img](./assets/wps745.jpg) 

 

![img](./assets/wps746.jpg) 

 

![img](./assets/wps747.jpg) 

 

![img](./assets/wps748.jpg) 

 

![img](./assets/wps749.jpg) 

 

##  删除用户

 

![img](./assets/wps750.jpg) 

 

![img](./assets/wps751.jpg) 

 

![img](./assets/wps752.jpg) 

 

该命令仅删除用户，保留家目录

 

![img](./assets/wps753.jpg) 

 

该命令删除用户以及用户主目录

##  查询用户信息

 

![img](./assets/wps754.jpg) 

 

![img](./assets/wps755.jpg) 

 

## 切换用户

![img](./assets/wps756.jpg)



## 查看当前用户/登录用户

 

![img](./assets/wps757.jpg) 

 

 

![img](./assets/wps758.jpg) 

 

# 用户组

## 新增组

![img](./assets/wps759.jpg)

共性可以理解为权限

 

![img](./assets/wps760.jpg) 

 

![img](./assets/wps761.jpg) 

 

![img](./assets/wps762.jpg) 

 

当添加用户时未指定其组，会自动生成一个与用户同名的组，并将其添加进去

 

##  修改用户的组

 

![img](./assets/wps763.jpg) 

 

![img](./assets/wps764.jpg) 

 

![img](./assets/wps765.jpg) 





##  运行级别

![img](./assets/wps769.jpg)

 

init 3

init 5

init 0

 

![img](./assets/wps770.jpg) 

 

 











 