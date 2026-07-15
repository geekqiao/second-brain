---
tags:
  - overview/ecosystem
  - devops
aliases:
  - DevOps
  - CI CD
  - Docker K8s
created: 2026-07-15
status: 🟢 已完成
---

# DevOps 概览

> [!info] 广度概览 · 回到 [[🗺️ 知识地图 MOC]]

## 是什么

**DevOps**（Development + Operations）是一种**文化与一套实践**：打通「写代码」和「上线运维」，用自动化让软件从提交到部署**快、稳、可重复**。它不是某个工具，而是一整条流水线。

## 为什么重要

- 没有 DevOps，发布 = 手动拷文件、人肉配置、半夜加班踩坑。
- 自动化流水线让「每次提交都能安全上线」，是敏捷迭代和 [[系统设计概览|高可用]] 的基础。
- 容器化让「在我电脑上能跑」变成「在哪都能跑」。

## 核心要点（点到为止）

### 核心环节（流水线）
需求 → 编码 → **构建 → 测试 → 部署 → 监控** → 反馈。后半段就是 DevOps 主战场（CI/CD）。

### CI / CD
- **CI（持续集成）**：代码一提交，自动**构建 + 跑测试**，尽早发现问题。
- **CD（持续交付/部署）**：通过测试后，自动**部署**到测试/生产环境。
- 工具：GitHub Actions、GitLab CI、Jenkins。

### 容器化（Docker）
- **Docker**：把应用**和它的所有依赖打包**成一个标准「容器」，保证在任何环境行为一致——彻底解决「环境差异」。
- 类比：集装箱——货物（应用）装进标准箱，任何船/车（服务器）都能运。

### 容器编排（Kubernetes / K8s）
- 管理成百上千个容器的「调度员」：自动扩缩容、故障自愈、滚动更新、负载均衡。
- 适合大规模微服务；小项目用 Docker Compose 即可。

### 基础设施即代码（IaC）
用代码描述服务器/云资源配置（Terraform、Ansible），可版本化、可复现，告别手动点控制台。

### 可观测性（Observability）
上线后要知道「健不健康」：**日志（logs）、指标（metrics）、链路追踪（traces）**（Prometheus、Grafana、ELK）。

> [!tip] 一句话地图
> Docker 打包 → CI/CD 自动测试部署 → K8s 调度运行 → 监控守护。从 [[Git MOC]] 的 push 开始，到生产环境运行，全链路自动化。

## 去哪深入

- 入门 Docker：官方互动教程（Docker 命令行，见 [[Linux MOC]]）。
- CI：在自己的 GitHub 项目配一个 GitHub Actions（跑测试）。
- 进阶：Kubernetes（较陡，建议先有 Docker 基础）、Terraform。

## 相关链接

- [[🗺️ 知识地图 MOC]]
- 前置：[[Linux MOC]]（命令行是 DevOps 的工作台）· [[Git MOC]]（CI 的触发源）
- 关联：[[软件工程概览]] · [[系统设计概览]] · [[测试基础]]
