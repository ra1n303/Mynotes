# 介绍

busybox是一个轻量级的开源工具集，它将许多常见的Unix/Linux命令行工具集成到一个单一的可执行文件中，旨在为资源受限的环境（如嵌入式系统）提供基本的系统功能

busybox的设计目标是体积小，效率高，同时保持功能完整，适用于嵌入式设备，救援系统或容器环境



# 核心特点

- 单一二进制文件
  - busybox将众多工具（如ls，cat，grep等）打包为一个可执行文件，通过符号链接或命令参数调用不同功能
  - 如：/bin/busybox ls 和 /bin/ls效果相同



# 常用功能

busybox集成了上百个Unix工具

- 文本操作：cp，mv，rm，ls，mkdir
- 文本处理：cat，grep，awk，sed
- 系统管理：ps，top，kill，mount，umount
- 网络工具：wget，ping，netstat
- shell：sh（通常是ash）



# 使用方式

```
busybox ls
```

直接调用busybox

这里会执行busybox内置的ls功能



```
busybox --list
```

输出当前busybox支持的所有命令