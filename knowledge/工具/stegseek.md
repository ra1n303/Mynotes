### 介绍

官方介绍：

Stegseek 是一个快如闪电的 steghide 破解器，可用于从文件中提取隐藏数据。它是作为原始 steghide 项目的分支构建的，因此，它比其他 cracker 快数千倍，并且可以在 2 秒内运行整个 rockyou.txt。

Stegseek 还可用于提取没有密码的 steghide 元数据，这可用于测试文件是否包含 steghide 数据。





### github项目地址

[RickdeJager/stegseek: :zap: Worlds fastest steghide cracker, chewing through millions of passwords per second :zap:](https://github.com/RickdeJager/stegseek)





### 基本用法：

爆破图像隐写的密码

```
stegseek -sf [stegofile.jpg] -wl [wordlist.txt]
```

