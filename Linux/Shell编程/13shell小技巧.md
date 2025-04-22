/dev/null

![image-20250315191439542](assets/image-20250315191439542.png)

垃圾桶



![image-20250315191506414](assets/image-20250315191506414.png)



```
ping -c 4 www.baidu.com >/dev/null 2>&1
```

将标准输出和标准错误输出重定向到垃圾桶，即不输出内容

或者

```
&>/dev/null
```

同等效果



![image-20250315191820742](assets/image-20250315191820742.png)





cmp命令

compare 比较，比较两个内容是否相同

![image-20250315192101661](assets/image-20250315192101661.png)

![image-20250315192107850](assets/image-20250315192107850.png)



相同内容无输出，但是查看$?可知成功执行

内容不同会报错





diff 

different

![image-20250315192645798](assets/image-20250315192645798.png)





cat -n 显示行数,结合 tail -1

![image-20250315192942041](assets/image-20250315192942041.png)

![image-20250315192945971](assets/image-20250315192945971.png)

或者wc -l

![image-20250315193013276](assets/image-20250315193013276.png)



![image-20250315193425958](assets/image-20250315193425958.png)

linux文本中结尾是$符号，和windows不同

windows结尾一般是 ^M

因此可能某些windows脚本无法直接在linux执行

dos2unix filename

通过dos2unix转换

![image-20250315193638517](assets/image-20250315193638517.png)