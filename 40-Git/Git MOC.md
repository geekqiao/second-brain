---
tags:
  - subject/git
  - moc
aliases:
  - Git
  - 版本控制
  - Git索引
created: 2026-07-15
status: 🟢 已完成
---

# Git MOC

> [!info] 重点科目 · [[🗺️ 知识地图 MOC]] · [[Home]]

## 📖 概述

**Git** 是分布式**版本控制系统**——它记录文件的每一次变更，让你能随时回到任意历史版本、安全地并行开发（分支）、与团队协作（合并）。它是现代软件开发协作的**事实标准**，也是 [[软件工程概览]] 的核心工具。

> 学 Git 的关键是建立**「三个区 + 一条链」**的心智模型：工作区 → 暂存区 → 版本库，加上分支这条提交链。命令只是操作这些区的接口；理解模型后，命令自然好记，排错也有方向。

## 📚 学习路径

1. [[基础概念与配置]] — 三区模型、.gitignore、初次配置
2. [[基本工作流]] — add/commit/status/diff/log 日常循环
3. [[分支与合并]] — branch/merge/rebase，冲突解决
4. [[远程协作]] — remote/push/pull、PR 工作流
5. [[高级操作]] — stash/cherry-pick/reset/revert/reflog
6. [[排错与最佳实践]] — 撤销错误、提交规范、分支策略
7. [[Git 常用命令速查表]] — 📋 按场景分组的常用命令清单（clone/分支/远程/撤销…）

## 🧠 核心速查表

| 需求 | 命令 |
| --- | --- |
| 看状态 | `git status` |
| 看改动 | `git diff` / `git diff --staged` |
| 暂存 | `git add .`（或具体文件） |
| 提交 | `git commit -m "msg"` |
| 看历史 | `git log --oneline --graph` |
| 建分支 | `git switch -c feature` |
| 切分支 | `git switch main` |
| 合并 | `git merge feature` |
| 拉取/推送 | `git pull` / `git push` |
| 暂存改动 | `git stash` |
| 撤销改动 | `git restore <file>` |
| 后悔药 | `git reflog`（找回丢失的提交） |

## 📈 进阶方向

- **工作流模型**：Git Flow / GitHub Flow / Trunk-based（见 [[排错与最佳实践]]）。
- **平台**：GitHub / GitLab / Gitea（PR/MR、CI/CD、Issues）。
- **进阶技巧**：interactive rebase 整理历史、hooks 自动化、submodule/subtree 管理子项目。
- 配合 [[DevOps 概览]] 的 CI/CD 流水线。

---

*🔗 返回 [[🗺️ 知识地图 MOC]] · [[Home]]*
