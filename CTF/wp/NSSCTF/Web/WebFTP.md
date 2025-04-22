![image-20250403173642290](./assets/image-20250403173642290.png)

尝试登录失败



dirsearch扫描目录

```
dirsearch -u http://node7.anna.nssctf.cn:22207/
```

![image-20250403173705072](./assets/image-20250403173705072.png)

存在git泄露

![image-20250403174717290](./assets/image-20250403174717290.png)

同时也存在phpinfo.php

README.md



README.md中给出了管理员账户

![image-20250403174951707](./assets/image-20250403174951707.png)

但是登进去没扫描内容



phpinfo.php中藏了flag

![image-20250403175023113](./assets/image-20250403175023113.png)