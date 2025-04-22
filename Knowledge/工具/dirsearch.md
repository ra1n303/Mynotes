### 介绍

dirsearch是一个用Python编写的命令行工具，主要用来执行网站目录和文件的字典扫描。

它通过暴力字典攻击来尝试发现网站上隐藏的目录和文件。

其广泛应用于渗透测试和Web安全评估中，尤其是在进行目录和文件枚举时





### github项目地址

[GitHub - maurosoria/dirsearch：Web 路径扫描器](https://github.com/maurosoria/dirsearch)



     --prefixes=PREFIXES
              Add  custom  prefixes  to  all wordlist entries (separated by
              commas)

### 基本用法



#### 指定目标URL

```
dirsearch -u <url>
```

-u或--url  指定目标url

最基本的选项，用于指定要扫描的目标网站





#### 指定字典文件路径

```
dirsearch -u <url> -w /usr/share/dirbuster/wordlists/directory-list-lowercase-2.3-medium.txt
```

-w 或 --wordlist  指定字典文件路径，dirsearch将使用该字典进行目录/文件扫描

/usr/share/dirbuster/wordlists/directory-list-lowercase-2.3-medium.txt

kali下的目录字典





#### 设置要扫描的文件扩展名

```
dirsearch -u <url> -e php,html,txt
```

-e 或 --extensions  设置要扫描的文件的扩展名（默认为.php .html .txt .js）





#### 设置线程

```
dirsearch -u <url> -t 50
```

-t 或 --threads  设置扫描的并发线程数（增加线程数可以加速扫描，但可能导致目标服务器过载）





### 排除指定的HTTP状态码

```
dirsearch -u <url> -x 403,404
```

-x 或 --exclude-status  排除指定的HTTP状态码，不显示这些状态码的结果

排除403和404错误的响应，只显示其他有效的目录/文件





#### 启用递归扫描

```
dirsearch -u http://example.com -r
```

-r 或 --recurisive  启用递归扫描，进一步扫描子目录

在扫描过程中，如果发现某个目录存在，dirsearch会进入该目录并扫描其中的内容，直到达到最大深度或找不到更多目录。





#### 设置扫描的深度

```
dirsearch -u http://example.com -r --depth 3
```

--depth 设置扫描的深度（配合-r使用），即递归扫描的最大层数。

限定递归的最大深度，避免扫描过深，节省时间并减少对目标的压力