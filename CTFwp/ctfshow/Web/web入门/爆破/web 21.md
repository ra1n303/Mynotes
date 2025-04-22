提示

用户名admin

密码为shark63



![image-20250401201215313](./assets/image-20250401201215313.png)

登录界面

随便输入账号密码

抓包

![image-20250401201252176](./assets/image-20250401201252176.png)

身份认证处，base64加密

![image-20250401201450388](./assets/image-20250401201450388.png)

解码后得到密码规则

user:passwd





放入爆破模块

在身份认证出添加payload

![image-20250401201321544](./assets/image-20250401201321544.png)



然后选择Custodm iterator

第一处导入字典（记得添加admin）

![image-20250401201401702](./assets/image-20250401201401702.png)

第二处为:

![image-20250401201519780](./assets/image-20250401201519780.png)

第三处添加密码（记得添加shark63）

![image-20250401201837555](./assets/image-20250401201837555.png)

添加base64编码

![image-20250401201843970](./assets/image-20250401201843970.png)

取消勾选URL编码

![image-20250401201911209](./assets/image-20250401201911209.png)

开始爆破

Length排序

![image-20250401201928180](./assets/image-20250401201928180.png)

存在length不一样的字段

解码后得到账号密码

![image-20250401202007275](./assets/image-20250401202007275.png)

同时返回包中存在flag

![image-20250401202022061](./assets/image-20250401202022061.png)