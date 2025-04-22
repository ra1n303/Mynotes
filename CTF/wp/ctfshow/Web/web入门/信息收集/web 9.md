dirsearch扫描目标网站未得出结果

根据题目

发现网页有个错别字？赶紧在生产环境vim改下，不好，死机了

判断是swp泄露

![image-20250309093803663](./assets/image-20250309093803663.png)

直接访问 index.php.swp

得到下载文件

将swp后缀名删除后得到 index.php

打开后得到flag

但是推荐使用vim -r filename.swp修复文件之后打开

![image-20250309093810852](./assets/image-20250309093810852.png)

![image-20250309093813085](./assets/image-20250309093813085.png)