### 介绍

gobuster是一款用于目录和文件枚举的开源工具，常用于渗透测试和网络安全评估，它通过暴力破解的方式，扫描目标网站或服务器的目录，文件和资源。





### 基本功能

- 目录枚举
  - 通过字典文件暴力破解目标网站的目录结构。

- 子域名枚举
  - 通过字典文件暴力破解目标的子域名

- 虚拟主机枚举
  - 扫描目标服务器的虚拟主机

- 文件枚举
- 通过字典文件暴力破解目标的DNS记录

- 支持多种模式
  - dir（目录枚举）dns（DNS枚举）vhost（虚拟主机枚举）等





#### 基本用法



#### 目录枚举

```
gobuster dir -w /usr/share/dirbuster/wordlists/directory-list-lowercase-2.3-medium.txt -u <url> -x php,html -t 50
```

dir：目录枚举

-w：自定文件路径

-u：目标url

-x：指定扩展名

-t：设置线程数



#### 虚拟主机枚举

```
gobuster vhost -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -u <url>
--append-domain
```

vhost；虚拟主机枚举（Virtual Host Enumeration）。

--append-domain：将单词表中的每个条目附加到目标 URL 的域名后，形成完整的域名进行测试。例如，如果目标是 example.com，单词表中有 admin，则测试 admin.example.com。