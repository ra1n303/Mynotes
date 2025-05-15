检查所有定时任务相关文件

```
sudo ls -l /var/spool/cron/crontabs/ /etc/crontab /etc/cron.d/

```



查看隐藏的恶意cron任务

```
cat -v /var/spool/cron/crontabs/root
```



```
关键在于^M 和 ^@ 特殊控制字符，它们可以被⽤来隐藏恶意 cron 任务，使其在常规检查中不可⻅但仍能执⾏
^M (0x0D)：回⻋(Carriage Return, CR) 
^@ (0x00)：空字符(Null)
这些字符在终端显⽰为可⻅符号，但在⽂本处理中会被特殊解释
```



攻击原理
攻击者通过以下⽅式利⽤这些字符：
破坏显⽰：crontab -l 命令遇到这些字符会提前终输出
保持执⾏：cron 服务仍能正确解析并执⾏这些任务
逃避检测：常规审计⼯具可能⽆法正确显⽰这些任务



使⽤hexdump可以查看

```
crontab -l | hexdump -C
```

