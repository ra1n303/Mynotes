### 介绍

LinPEAS 是一个开源脚本，全称是Linux Privilege Escalation Awesome Script，由网络安全研究者 Carlos Polop 开发，隶属于 PEASS-ng（Privilege Escalation Awesome Scripts Suite）项目。它旨在帮助渗透测试人员、安全研究者和系统管理员自动化发现 Linux 系统上的权限提升路径。LinPEAS 通过扫描系统配置、权限、文件、服务等，找出可能被攻击者利用的漏洞或弱点，从而提升权限（通常是从普通用户提升到 root 用户）。

LinPEAS 的设计目标是全面性和易用性，它以颜色编码的方式输出结果，帮助用户快速识别高危配置（例如红色表示 99% 可能导致权限提升的配置）。该工具广泛应用于 CTF（Capture The Flag）竞赛、渗透测试和安全审计中。





### 基本功能：

- 系统信息收集
  - 操作系统版本、内核版本、已安装软件包。
  - 检查是否存在已知漏洞（如未修补的内核漏洞）。
- 权限和文件分析
  - 查找 SUID/SGID 文件（可以以更高权限运行的二进制文件）。
  - 检测世界可写文件或目录（可能被恶意修改）。
  - 检查关键配置文件（如 /etc/passwd、/etc/shadow）的权限。
- 用户和服务信息
  - 列出所有用户、当前用户权限、sudo 权限。
  - 检查运行的服务及其权限（例如 MySQL 是否以 root 运行）。
- 定时任务和进程监控
  - 分析 cron 任务，寻找以 root 权限运行但可修改的脚本。
  - 监控进程以发现频繁运行的定时任务。





### 颜色编码输出

- 红/黄色：99% 可能导致权限提升的高危配置。

- 红色：需要关注的潜在风险。

-	绿色：安全的配置。

- 其他颜色：用于区分用户、设备等信息。





### github项目地址

```
https://github.com/peass-ng/PEASS-ng/releases/latest/download/linpeas.sh
```





