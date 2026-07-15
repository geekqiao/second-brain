---
tags:
  - subject/docker
  - moc
aliases:
  - Docker
  - 容器
  - Docker索引
created: 2026-07-15
status: 🟢 已完成
---

# Docker MOC

> [!info] 重点科目 · [[🗺️ 知识地图 MOC]] · [[Home]]

## 📖 概述

**Docker** 是最流行的**容器化平台**——把应用**连同它的一切依赖**（代码、运行时、库、配置）打包成一个标准「容器」，保证在任何机器上行为一致，彻底消灭「在我电脑上能跑」的问题。它是 [[DevOps 概览]] 的基石，也是 [[Linux MOC]] 实战的高阶应用。

> 学 Docker 抓住三对概念：**镜像 vs 容器**（类 vs 实例）、**Dockerfile vs 镜像**（配方 vs 成品）、**镜像仓库 vs 本地**（云 vs 本地）。理清这三层，命令都是对它们的操作。

## 📚 学习路径

1. [[核心概念]] — 镜像/容器/仓库/Dockerfile/分层存储
2. [[镜像与 Dockerfile]] — 写 Dockerfile 构建自己的镜像
3. [[容器操作]] — run/exec/logs 等日常容器管理
4. [[数据卷与网络]] — 数据持久化与容器间通信
5. [[Docker Compose]] — 多容器一键编排
6. [[Docker 常用命令速查表]] — 📋 按场景分组的常用命令清单（镜像/容器/卷/网络/Compose…）

## 🧠 核心速查表

| 需求 | 命令 |
| --- | --- |
| 拉镜像 | `docker pull nginx` |
| 列镜像/容器 | `docker images` / `docker ps -a` |
| 跑容器 | `docker run -d -p 8080:80 --name web nginx` |
| 进容器执行 | `docker exec -it web bash` |
| 看日志 | `docker logs -f web` |
| 停/起/删 | `docker stop/rm web` |
| 构建镜像 | `docker build -t myapp .` |
| 清理 | `docker system prune` |
| 多容器编排 | `docker compose up -d` |
| 数据持久化 | `-v mydata:/app/data`（volume） |

## 📈 进阶方向

- **编排**：多容器/集群用 [[DevOps 概览|Kubernetes]]。
- **镜像瘦身**：多阶段构建（multi-stage）、alpine 基础镜像。
- **安全**：最小权限、非 root 用户、扫描漏洞（`docker scout`）。
- **CI/CD**：构建镜像推送仓库，自动化部署（见 [[远程协作]] + [[DevOps 概览]]）。
- **云原生**：Docker 是云原生生态（K8s、Service Mesh）的容器标准。

---

*🔗 返回 [[🗺️ 知识地图 MOC]] · [[Home]]*
