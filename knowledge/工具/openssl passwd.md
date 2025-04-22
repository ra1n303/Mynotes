### 介绍

openssl passwd是 OpenSSL 工具套件中的一个命令，用于生成和验证密码散列值。它常用于在 Unix 和 Linux 系统中创建密码散列，以便在 `/etc/shadow` 或其他需要存储加密密码的地方使用。





### 基本功能

- 生成密码散列：`openssl passwd` 可以生成多种算法的密码散列，以便存储和验证用户密码。
- 支持多种编码标准：可以使用不同的加密标准，如 DES、MD5、SHA-256 和 SHA-512。
- 增强安全性：使用随机盐值（salt）来增强散列的安全性，防止彩虹表攻击。





### 基本用法



#### 生成密码

```
openssl passwd [options] [password]  
```

选项：

-1：MD5，默认

-5：SHA-256加密

-6：使用SHA-512加密

-salt：指定盐值，增加安全性，如果不指定，OpebSSL将自动生成盐值

-crypt：采用传统的crypt()函数加密