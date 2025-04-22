### 介绍

WhatWeb是一款功能强大的信息收集工具，用于识别目标网站的技术栈和特征，例如使用的Web服务器，CMS（内容管理系统），框架和插件等

适合初步的渗透测试和信息收集阶段





### 基本用法



#### 扫描单个目标

```
whatweb <url>
```





#### 扫描多个目标

```
whatweb -i <文件路径>
```

-i：指定文件

通过文件批量扫描多个目标，每行一个URL



#### 详细扫描

```
whatweb -v <url>
```

获取更详细的扫描结果（默认输出级别为1）

或调整更详细级别

```
whatweb -v -a <详细级别> <url>
```



#### 自定义请求头（UA或COOKIE）

```
whatweb --user-agent "Mozilla/5.0" <url>
```

添加UA头

```
whatweb --cookide "Auto=123" <url>
```

添加Cookie