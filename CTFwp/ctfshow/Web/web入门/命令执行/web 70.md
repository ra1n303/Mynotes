![image-20250404163310898](./assets/image-20250404163310898.png)



同样利用上题的payload直接出flag

```
c=include("/flag.txt");
```

![image-20250404163324698](./assets/image-20250404163324698.png)



然后我试了一下，var_dump()被禁用，但是var_export()还是可以

![image-20250404163344243](./assets/image-20250404163344243.png)

![image-20250404163421230](./assets/image-20250404163421230.png)