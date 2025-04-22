看到shell就想到可疑函数



## 可疑函数调用

WebShell 通常会使用一些危险的函数来执行系统命令或代码，如：

```
PHP: eval(), system(), exec(), shell_exec(), passthru(), assert(), base64_decode()
```

```
ASP: Execute(), Eval(), CreateObject()
```



```
JSP: Runtime.getRuntime().exec()
```



## 编码和解码

WebShell 经常使用编码和解码技术来隐藏其真实意图，如 Base64 编码：

```
 eval(base64_decode('encoded_string'));
```



## 文件操作

WebShell 可能会包含文件操作函数，用于读取、写入或修改文件：

```
PHP: fopen(), fwrite(), file_get_contents(), file_put_contents()
```

```
ASP: FileSystemObject
```



## 网络操作

WebShell 可能会包含网络操作函数，用于与远程服务器通信：

```
PHP: fsockopen(), curl_exec(), file_get_contents('http://...')
```

```
ASP: WinHttp.WinHttpRequest
```





## 定位可疑文件

我们可以尝试定位一些特殊的后缀文件，例如：.asp, .php, .jsp, .aspx。

命令列如：

```matlab
find ./ -type f -name "*.jsp" | xargs grep "exec("
find ./ -type f -name "*.php" | xargs grep "eval("
find ./ -type f -name "*.asp" | xargs grep "execute("
find ./ -type f -name "*.aspx" | xargs grep "eval("
```

