```
proxychains4 mysql -h 10.10.135.66 -u root -p
```

![image-20250603124127958](./assets/image-20250603124127958.png)

2026报错，TLS/SSL错误

添加--skip-ssl参数

```
proxychains4 mysql -h 10.10.135.66 -u root -p --skip-ssl
```

![image-20250603124134847](./assets/image-20250603124134847.png)