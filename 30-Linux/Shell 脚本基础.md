---
tags:
  - subject/linux
  - shell
aliases:
  - Shell脚本
  - bash
  - 脚本基础
created: 2026-07-15
status: 🟢 已完成
---

# Shell 脚本基础

> [!info] 所属：[[Linux MOC]] · 上一篇 [[进程服务与资源管理]] · 下一篇 [[文本处理三剑客]]

## 核心概念

把一串 Linux 命令写进文件、加上**变量、条件、循环、函数**，就成了 Shell 脚本——实现自动化（备份、批量处理、部署）的最快方式。bash 是最常见的 Shell。

### 脚本骨架
- 第一行 `#!/bin/bash`（shebang）：声明用哪个解释器执行。
- 运行方式：① `chmod +x script.sh` 后 `./script.sh`；② `bash script.sh`（无需可执行权限）。
- 注释：`#` 开头。

### 变量
- 定义：`name="Ada"`（**等号两边不能有空格！**）。
- 使用：`$name` 或 `${name}`。
- 命令替换：`now=$(date +%F)` 或反引号 `` `date` ``。
- 特殊变量：`$0`（脚本名）、`$1`~`$9`（参数）、`$#`（参数个数）、`$@`（所有参数）、`$?`（上条命令退出码，0 成功）。

### 条件与退出码
- `if [ 条件 ]; then ... fi`——注意 `[` 后和 `]` 前必须有空格。
- 字符串/数字比较用不同操作符（`=` vs `-eq`）。
- 命令成功返回退出码 0，非 0 为失败——`$?` 据此判断。

### 循环
- `for` / `while` / `until`。
- 遍历：`for x in a b c; do ... done`。

### 函数
`myfunc() { ... }`，用 `$1` 取函数参数。

## 代码示例

```bash
#!/bin/bash
# 一个备份脚本：把 src 目录打包到带日期的文件
set -e                          # 遇错即退出（安全第一，强烈推荐）
set -u                          # 用未定义变量报错

# 变量
SRC="/home/geek/projects"
DEST="/backup"
DATE=$(date +%Y%m%d)            # 命令替换
ARCHIVE="backup-$DATE.tar.gz"

# 条件判断
if [ ! -d "$DEST" ]; then
  mkdir -p "$DEST"
  echo "已创建 $DEST"
fi

# 参数处理
if [ "$#" -lt 1 ]; then
  echo "用法: $0 <备注>" ; exit 1
fi
echo "备份备注: $1"

# 执行 + 判断结果
tar -czf "$DEST/$ARCHIVE" "$SRC"
if [ $? -eq 0 ]; then
  echo "✅ 备份成功: $DEST/$ARCHIVE"
else
  echo "❌ 备份失败" >&2        # 错误输出到 stderr
  exit 1
fi

# 循环：批量处理文件
for f in *.py; do
  echo "处理 $f"
  # python "$f"                 # 对每个文件执行
done

# while 循环 + 读取文件逐行
while IFS= read -r line; do
  echo "行: $line"
done < tasks.txt

# 函数
greet() {
  echo "Hello, $1!"             # $1 是函数参数
}
greet "Ada"

# 遍历所有参数
for arg in "$@"; do
  echo "参数: $arg"
done
```

## ⚠️ 易错点 / 面试要点

- **等号两边不能有空格**：`name = "x"`（有空格）会被当成命令 `name` 调用，报错。必须 `name="x"`。
- **`[` 周围必须有空格**：`[$a=$b]` 错；`[ "$a" = "$b" ]` 对。变量要**双引号包住**，防止为空或含空格时语法崩溃。
- **字符串比较 vs 数字比较**：`[ "$a" = "$b" ]`（字符串），`[ "$a" -eq "$b" ]`（数字）。混用是经典 bug。现代 bash 推荐 `[[ ]]`（增强版，支持 `&&`/`||`、模式匹配）。
- **`set -e` 让脚本更安全**：默认 bash 遇错继续跑，可能掩盖问题。`set -euo pipefail` 是生产脚本标配（遇错退出、未定义变量报错、管道任一环失败即失败）。
- **命令替换用 `$()` 而非反引号**：`$(...)` 可嵌套、可读，反引号已过时。
- **引号**：`"$var"` 双引号会展开变量；`'$var'` 单引号原样输出（不展开）。路径含空格时务必加引号。
- **`read -r`**：`-r` 防止反斜杠被转义，读文件逐行的标准写法是 `while IFS= read -r line`。

## 相关链接

- [[Linux MOC]] · [[进程服务与资源管理]]
- 脚本里常调用的文本工具：[[文本处理三剑客]]
- Python 脚本是 Shell 的强力替代：[[Python MOC]]
