### 介绍

开源的子域收集工具，旨在为网络安全研究人员和渗透测试人员提供高效、全面的子域名信息收集功能。以强大的收集能力和灵活的处理方式著称，支持多线程操作，速度快且易于使用。





### 基本用法



#### 收集单个域名子域

```
python3 oneforall.py --target <url> run
```

target： 目标url



### 批量收集域名子域

创建domains.txt文件，每行一个域名，然后运行

```
python3 oneforall.py --targets ./domains.txt run
```

