### 介绍

wfuzz是一款功能强大的web应用程序模糊测试工具，用于发现web应用中的漏洞和隐藏的资源。它通过向目标发送大量自定义请求，分析响应来检测潜在的安全问题，如目录遍历，sql注入，xss等





### 基本功能

- 目录和文件枚举
- 通过字典文件暴力破解目标网站的目录和文件

- 参数模糊测试
  - 对URL参数，表单字段等进行模糊测试，检测漏洞

- 支持多种请求类型
  - 支持GET，POST，PUT，DELETE等多种HTTP方法

- 灵活的过滤器和匹配器
  - 可以根据响应代码，响应大小，响应时间等过滤结果

- 多线程支持
  - 支持多线程操作，提高扫描效率





#### 基本用法



#### 目录检测

```
wfuzz -w /usr/share/dirbuster/wordlists/directory-list-lowercase-2.3-medium.txt -u <url/FUZZ>
```

-w：指定字典文件

FUZZ：占位符，表示字典中的内容将会被替换到这里





#### 参数模糊匹配

```
wfuzz -w /usr/share/wfuzz/wordlist/general/common.txt -u <url>?FUZZ=../../../../etc/passwd
```

-w：指定字典文件

FUZZ=../../../../etc/passwd：其中FUZZ表示占位符，利用字典中的内容替代FUZZ，查看/etc/passwd能否被访问，以便检查是否存在文件包含漏洞或其他未授权访问的问题



### 字典

```
wfuzz -z file,/usr/share/seclists/Discovery/Web-Content/directory-list-2.3-small.txt --hh <xx> <url/FUZZ>
```



#### 其他参数

--hc/hl/hw/hh

表示要隐藏的code/lines/words/chars



--hw 288 即过滤words列的值为288的内容
（类似于bp爆破时过滤不同的length）
当不进行任何过滤时，wfuzz会将所有的结果输出，我们很难找到关键