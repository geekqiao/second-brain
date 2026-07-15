---
tags:
  - subject/linux
  - moc
aliases:
  - Linux
  - 命令行
  - Linux索引
created: 2026-07-15
status: 🟢 已完成
---

# Linux MOC

> [!info] 重点科目 · [[🗺️ 知识地图 MOC]] · [[Home]]

## 📖 概述

**Linux** 是开发者的「母语环境」——服务器、云、容器、嵌入式几乎都跑在它上面。掌握 Linux 主要是掌握**命令行（CLI）**：用文本命令高效操作系统，摆脱对图形界面的依赖。它是 [[操作系统概览]] 的最佳实战入口。

> 学 Linux 的心法：**不要背命令，要理解「每条命令解决什么问题」**。命令是工具，文件夹结构、权限、进程、管道才是底层概念。掌握 `--help` 与 `man`，任何命令都能自学。

## 📚 学习路径

1. [[文件系统与权限]] — 目录结构、路径、权限模型（rwx）
2. [[常用命令与文件操作]] — 增删改查、查找、查看文件内容
3. [[进程服务与资源管理]] — ps/top、systemd、后台运行
4. [[Shell 脚本基础]] — 把命令组合成自动化脚本
5. [[文本处理三剑客]] — grep / sed / awk 文本处理
6. [[网络磁盘与用户]] — 网络、磁盘、用户管理

## 🧠 核心速查表

| 需求 | 命令 |
| --- | --- |
| 我在哪 | `pwd` |
| 有什么 | `ls -la` |
| 去哪里 | `cd path` |
| 建目录/文件 | `mkdir` / `touch` |
| 复制/移动/删 | `cp` / `mv` / `rm` |
| 看文件 | `cat` / `less` / `head` / `tail` |
| 搜文本 | `grep "pat" file` |
| 找文件 | `find . -name "*.py"` |
| 看进程 | `ps aux` / `top` / `htop` |
| 看端口/网络 | `ss -tulpn` / `curl -v` |
| 权限 | `chmod` / `chown` |
| 管道 | `cmd1 \| cmd2`（前者的输出→后者的输入） |
| 求助 | `man cmd` / `cmd --help` |

## 📈 进阶方向

- **Shell 进阶**：bash/zsh 配置、`tmux` 终端复用、`vim` 编辑。
- **服务端运维**：systemd、日志（journalctl）、性能分析（`perf`/`strace`）。
- **容器**：Docker / Kubernetes（见 [[DevOps 概览]]）。
- **发行版**：Ubuntu（友好）、Debian、CentOS/RHEL（企业）、Arch（滚动、硬核）。

---

*🔗 返回 [[🗺️ 知识地图 MOC]] · [[Home]]*
