/proc 是Linux 系统中一个特殊的文件系统，它包含了系统和内核的信息。

/proc 目录下的每一个文件都表示一个进程或系统的信息，例如内存使用情况、CPU 使用情况、磁盘分区、系统负载等。



常见的/proc目录下的文件或目录及其含义

```
/proc/cmdline：系统启动时的内核命令行参数。
/proc/cpuinfo：CPU 信息，如型号、速度、使用情况等。
/proc/meminfo：内存信息，如总量、使用量等。
/proc/mounts：已挂载的文件系统列表。
/proc/version：内核版本信息。
/proc/[pid]：每个进程的信息目录，[pid] 是进程的 PID。
/proc/[pid]/cmdline：进程启动时的命令行参数。
/proc/[pid]/status：进程的状态信息，比如 CPU 使用量、内存使用量等。
```



