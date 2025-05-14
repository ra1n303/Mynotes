## index1.php

访问http://127.0.0.1/dedecms/index1.php

![image-20250513151316356](./assets/image-20250513151316356.png)

找到index1.php

![image-20250513151439451](./assets/image-20250513151439451.png)

源码被篡改，删除title和h1

![image-20250513151520063](./assets/image-20250513151520063.png)

重新访问

![image-20250513151545785](./assets/image-20250513151545785.png)

修复成功





## index2.php

访问http://127.0.0.1/dedecms/index2.php

![image-20250513151609545](./assets/image-20250513151609545.png)

url跳转到http://127.0.0.1/dedecms/heiye/index2.html

首先查看index2.php

![image-20250513151748706](./assets/image-20250513151748706.png)

有一处文件包含（/include/common.inc.php）

找到该文件

![image-20250513151945760](./assets/image-20250513151945760.png)

删除

```
Header("Location:./heiye/index2.html");
```

![image-20250513152033818](./assets/image-20250513152033818.png)

重新访问http://127.0.0.1/dedecms/index2.php

![image-20250513152115893](./assets/image-20250513152115893.png)

修复成功





## index3.php

访问http://127.0.0.1/dedecms/index3.php

![image-20250513152152135](./assets/image-20250513152152135.png)

跳转到http://127.0.0.1/dedecms/heiye/index3.html

查看index3.php源码

![image-20250513152231143](./assets/image-20250513152231143.png)

js源码解密

![image-20250513152258624](./assets/image-20250513152258624.png)



找到该js文件

![image-20250513152342118](./assets/image-20250513152342118.png)

删除该index3.php中的js源码和1.js文件

![image-20250513152430285](./assets/image-20250513152430285.png)

修复成功





## index4.php

访问127.0.0.1/dedecms/index4.php

![image-20250513152527963](./assets/image-20250513152527963.png)

跳转到http://127.0.0.1/dedecms/heiye/index2.html

找到index4.php

![image-20250513152552191](./assets/image-20250513152552191.png)

依旧找到包含的文件（/include/common3.inc.php）

但是目录下没找到，利用给出的everything查找

![image-20250513152740019](./assets/image-20250513152740019.png)

查看源码

![image-20250513152803398](./assets/image-20250513152803398.png)

依旧删除Header部分

![image-20250513152832692](./assets/image-20250513152832692.png)

修复成功



## index5.php

访问http://127.0.0.1/dedecms/index5.php

![image-20250513152955899](./assets/image-20250513152955899.png)

跳转到http://127.0.0.1/dedecms/heiye/index5.html

找到index5.php

![image-20250513153032084](./assets/image-20250513153032084.png)

解密include包含的内容

![image-20250513153319788](./assets/image-20250513153319788.png)

找到该文件

![image-20250513153403672](./assets/image-20250513153403672.png)

删除该文件和include内容

![image-20250513153443604](./assets/image-20250513153443604.png)

成功修复