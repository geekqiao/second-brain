---
tags:
  - subject/git
  - cheatsheet
aliases:
  - git命令
  - git速查
  - git cheatsheet
  - git clone
created: 2026-07-15
status: 🟢 已完成
---

# Git 常用命令速查表

> [!info] 速查附录 · [[Git MOC]] · 上一篇 [[排错与最佳实践]]
> 概念与原理见前 6 篇；本页是**常用命令清单**，按场景分组，复制即用。

## 🔧 配置（一次性）
```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
git config --global pull.rebase true        # pull 时 rebase，避免无意义合并提交
git config --global --list                  # 查看全部配置
```

## 📦 获取仓库
```bash
git init                                   # 当前目录初始化为仓库
git clone <url>                            # 克隆远程仓库（含全部历史）
git clone <url> <dir>                      # 克隆到指定目录
git clone --depth 1 <url>                  # 浅克隆：只取最新一次提交（省时间/流量）
git clone -b <branch> <url>                # 克隆后直接切到指定分支
```

## 📝 日常改动（最高频循环）
```bash
git status                                 # 看哪些文件改了 / 已暂存
git status -s                              # 精简模式
git diff                                   # 工作区改动（还没 add）
git diff --staged                          # 暂存区改动（add 了没 commit）
git add <file>                             # 暂存指定文件
git add .                                  # 暂存当前目录全部改动
git add -p                                 # 交互式逐块挑选暂存（推荐）
git commit -m "feat: 描述"                  # 提交
git commit --amend --no-edit               # 并入漏 add 的文件到上次提交（未推送时）
git commit --amend -m "新信息"              # 改最近一次提交信息
```

## 📜 查看历史与差异
```bash
git log --oneline --graph --all -n 20      # 图形化最近 20 条（最实用）
git log --stat                            # 每次提交改了哪些文件
git log -p <file>                         # 某文件的完整变更历史
git show <hash>                           # 看某次提交的详细 diff
git show HEAD~1                           # 看上一次提交
git blame <file>                          # 逐行查「谁、哪次提交」写的
git diff <a>..<b>                         # 比较两个提交/分支的差异
git log -S "someFunc"                     # 搜索「何时引入/删除了某字符串」
```

## 🌿 分支
```bash
git branch                                # 列本地分支
git branch -a                             # 列全部（含远程）
git switch <branch>                       # 切换分支（现代命令）
git switch -c <new-branch>                # 新建并切换
git branch -d <branch>                    # 删除已合并分支
git branch -D <branch>                    # 强制删除（未合并也删）
git checkout -b <branch> origin/<branch>  # 基于远程分支建本地分支
```

## 🔀 合并与变基
```bash
git merge <branch>                        # 把 branch 合入当前分支
git merge --abort                         # 合并冲突时放弃合并
git rebase <branch>                       # 把当前分支提交挪到 branch 顶端
git rebase --abort / --continue           # 变基冲突：放弃 / 解决后继续
git rebase -i HEAD~3                      # 交互式整理最近 3 个提交（合并/改信息/调序）
```

## 🌐 远程协作
```bash
git remote -v                             # 查看远程地址
git remote add origin <url>               # 关联远程
git fetch                                 # 拉远程更新（不动工作区）
git fetch origin                          # 拉指定远程
git pull                                  # fetch + merge（已跟踪分支）
git pull --rebase                         # fetch + rebase（更干净的历史）
git push                                  # 推送（已跟踪分支）
git push -u origin <branch>               # 首次推送并建立跟踪
git push --force-with-lease               # 强推（rebase 后），比 --force 安全
git push --tags                           # 推送所有标签（tag 默认不推）
```

## ↩️ 撤销与恢复
```bash
git restore <file>                        # 丢弃工作区未暂存的改动
git restore --staged <file>               # 把已 add 的撤出暂存区（改动保留）
git reset --soft HEAD~1                   # 撤销最近提交，改动留暂存区（未推送）
git reset HEAD~1                          # 撤销提交，改动退回工作区
git reset --hard HEAD~1                   # ⚠️ 彻底丢弃最近提交+工作区改动
git reset --hard origin/main              # 本地强制对齐远程（丢弃所有本地改动）
git revert <hash>                         # 生成反向提交撤销（已推送的公共分支用）
git clean -fd                             # 删除未跟踪的文件/目录（先 -n 预览）
```

## 🗃️ 暂存 / 标签 / 高级
```bash
git stash                                 # 临时搁置当前改动
git stash list                            # 看搁置列表
git stash pop                             # 恢复最近一次并删除
git stash drop                            # 删除某次搁置

git tag v1.0.0                            # 轻量标签
git tag -a v1.0.0 -m "发布说明"            # 附注标签（推荐）
git tag                                   # 列标签

git cherry-pick <hash>                    # 把某次提交搬到当前分支
git reflog                                # 后悔药：HEAD 移动历史，找回误删提交
git bisect                                # 二分查找引入 bug 的提交
```

> [!tip] 三个救命命令
> - **搞砸了想找回** → `git reflog`（几乎没真删过）
> - **想撤销已推送的提交** → `git revert`（不改历史）
> - **不知道现在啥状态** → `git status`（永远的第一步）

## 相关链接

- [[Git MOC]] · [[基础概念与配置]] · [[基本工作流]] · [[排错与最佳实践]]
