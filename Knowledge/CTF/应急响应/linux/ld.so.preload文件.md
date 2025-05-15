/etc/ld.so.preload 是 Linux 动态链接器（ ld.so ）的配置⽂件，⽤于强制预加载制定的共享库，该⽂件会 在所有其他动态库之前加载 



攻击者通过在此⽂件中写⼊恶意库路径可以实现劫持正常的函数调⽤， 

如

-- readdir()  隐藏⽂件 / 进程

-- connect()  隐藏⽹络链接

-- fopen()  隐藏⽇志记录 

在绝⼤多数标准的 Linux  发⾏版中，/etc/ld.so.preload  ⽂件默认是不存在的。 这是正常的安全设计



攻击流程 # 

```
1.  编译恶意动态库（劫持关键函数） 
gcc -shared -fPIC -o /lib/libhijack.so hijack.c
2.  写⼊预加载配置 
echo "/lib/libhijack.so" > /etc/ld.so.preload 
3.  所有新启动进程都会加载该库
```



```
 grep -E 'readdir|connect|exec'
```

