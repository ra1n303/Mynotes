如某些由js开发游戏题目

结束后有弹窗提示

可以查看js源码

搜索alert，接着查找关键信息就好



如果返回url+参数

可以去js源码中分析参数信息

 如game1





可以去控制台中选择弹窗或者编码解码

(当存在某些加密如base64，但是经过某些网站加密后仍然无效，此时可以选用网站控制台进行加密，详见game 1)



# 例题

## game1

![image-20250402194729447](./assets/image-20250402194729447.png)

抓包

![image-20250402194737560](./assets/image-20250402194737560.png)

提交score ip sign等参数

score和ip可以理解

sign像是base64编码

解码后出错

搜索sign

![image-20250402194747032](./assets/image-20250402194747032.png)

发现一处定义sign

![image-20250402194752530](./assets/image-20250402194752530.png)

sign值为Base64.encode（score）

修改分数为1000000000000000000000000000

并将其base64加密

bp抓包修改

![image-20250402194801058](./assets/image-20250402194801058.png)

报错

 

尝试利用控制台加密

设置分数为100000000

![image-20250402194805844](./assets/image-20250402194805844.png)

![image-20250402194807783](./assets/image-20250402194807783.png)

成功得到flag







## Classic Childhood Game

![image-20250402194822139](./assets/image-20250402194822139.png)

mota函数中存在字符串

控制台调用该函数

得到flag

![image-20250402194827957](./assets/image-20250402194827957.png)







## 2048

提示需要达到20000分

 

查看页面源码

找到

![image-20250402194841278](./assets/image-20250402194841278.png)

控制台执行

![image-20250402194847898](./assets/image-20250402194847898.png)







## ezgame

![image-20250402194858018](./assets/image-20250402194858018.png)

查看源码

![image-20250402194902991](./assets/image-20250402194902991.png)

如果分数超过65 可以得到flag

查看网页js信息

发现flag

![image-20250402194907825](./assets/image-20250402194907825.png)

或者控制台修改分数

首先找到分数的变量

scorePoint

![image-20250402194913152](./assets/image-20250402194913152.png)

修改值

![image-20250402194917965](./assets/image-20250402194917965.png)







## Welcome To HDCTF 2023

![image-20250402194927979](./assets/image-20250402194927979.png)

F12查看js源码

![image-20250402194933662](./assets/image-20250402194933662.png)

jsfuck解码

 

或者控制台直接输出

![image-20250402194940594](./assets/image-20250402194940594.png)