# 关于sudo -l

sudo -l 命令用于检查当前用户在系统上可以通过sudo执行的命令及权限

同时也要注意执行的命令所属用户，并通过-u参数指定用户

```
sudo -u ra1n3 /ls
sudo /ls
```

sudo -l是查看当前用户再/etc/sudoers或/etc/sudoers.d中配置的权限的快捷方式，他不仅显示允许执行的命令，还可能暴露配置细节（如是否需要密码，环境变量是否保留）

/etc/sudoers中的内容

```
(ALL:ALL) ALL 表示可作为任意用户运行任意命令
ra1n3=(ALL:ALL) ALL 表示ra1n3可以以sudo方式执行任何命令
```

如果该文件可读，可以直接查找其他用户的利用权限，

如果可写，则直接修改当前用户的sudo权限即可实现提取。

而-u 参数不仅限于提权到 root，还可横向移动到其他用户。

如已知另一个用户具有更高权限，则可以

```
sudo -u user /bin/bash
```



# 关于(ALL, !root)

```
hacker ALL=(ALL,!root) /bin/bash
```

- 第一个 ALL：可以在任何主机上运行
- (ALL,!root)：可以以除 root 外的任何用户身份运行
- /bin/bash：具体允许的命令

```
sudo -u#-1 /bin/bash
```

- -u 指定运行时的用户

- \#-1 使用 UID（用户 ID）的特殊语法

  - 由于 sudo 处理数字 UID 的方式，-1 被解释为 0

  - UID 0 = root（根用户）
