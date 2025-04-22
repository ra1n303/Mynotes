### 介绍

WPScan是一个专门用于扫描WordPress网站的安全工具，可以识别WordPress核心，插件，主题的漏洞，并支持暴力破解管理员账户密码。



### 基本功能

- 插件漏洞检测
  - 扫描已安装的wordpress插件，并查找已知的漏洞
- 主题漏洞检测
  - 检查wordpress使用的主题是否存在已知漏洞
- 用户名枚举
  - 发现公开的wordpress用户名，位密码暴力破解等操作提供线索
- 版本检测
  - 检测目标wordpress版本，查找与版本相关的已知漏洞
- 敏感信息扫描
  - 检查敏感文件（如备份文件，配置文件等）是否暴露



### 基本用法



#### 基本扫描

```
wpscan --url <url>
```





#### 枚举用户

```
wpscan --url <url> --enumerate u
```

u：枚举用户列表，可能被用来进行暴力破解

同时也可以利用其尝试得到用户信息

--enumerate可以写为 -e





#### 指定字典文件爆破用户名和密码

```
wpscan --url <url> -U users.txt -P passwd.txt
```

