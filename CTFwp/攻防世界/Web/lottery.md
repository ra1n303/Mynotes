访问发现是一个彩票网站

 

Home

游戏规则：

 

![img](./assets/wps596.jpg) 

Buy 买一张彩票

![img](./assets/wps597.jpg) 

 

Account 当前账户金额

 

![img](./assets/wps598.jpg) 

 

购买flag

 

![img](./assets/wps599.jpg) 

 

 

dirsearch 扫描网站

发现存在git泄露

![img](./assets/wps600.jpg) 

 

 

githack下载得到源码
 

![img](./assets/wps601.jpg) 

 

![img](./assets/wps602.jpg) 

 

 

 

访问api.php(这是接口文件一般是定义方法类的)

发现buf方法存在弱类型判断漏洞

![img](./assets/wps603.jpg) 

这是获取代币的方法：

生成7个随机数并依次与传入的number比较，当为true时，$same_count记录的值加1

==弱类型比较绕过

![img](./assets/wps604.jpg) 

 

bp抓包

![img](./assets/wps605.jpg) 

 

将numbers改为数组：

"numbers":[true,true,true,true,true,true,true]

 

![img](./assets/wps606.jpg) 

 

 

 