### 介绍

Linux-Exploit-Suggester.sh（以下简称 LES）是由 The-Z-Labs 开发的脚本，最初受到 PenturaLabs 的 Linux_Exploit_Suggester 的启发。与后者相比，LES 在功能上更全面，包含了更多最新的 Linux 内核漏洞利用（截至 2017 年初），并且增加了对用户空间程序（userspace packages）的权限提升向量的检测。它的主要目标是通过分析系统信息（如内核版本、已安装的软件包等），快速列出可能适用的公开漏洞利用（exploits），从而帮助用户找到提权路径。

LES 是一个用 Bash 编写的脚本，设计上注重轻量化和易用性，运行时无需额外依赖，通常可以直接在目标系统上执行。它通过启发式方法（heuristics）评估目标系统的漏洞暴露情况，并提供潜在的利用建议。





### 保存

```
wget https://raw.githubusercontent.com/mzet-/linux-exploit-suggester/master/linux-exploit-suggester.sh -O les.sh
```



直接执行脚本