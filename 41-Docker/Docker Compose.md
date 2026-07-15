---
tags:
  - subject/docker
  - compose
aliases:
  - Docker Compose
  - docker compose
  - 多容器编排
created: 2026-07-15
status: 🟢 已完成
---

# Docker Compose

> [!info] 所属：[[Docker MOC]] · 上一篇 [[数据卷与网络]]

## 核心概念

真实应用通常不止一个容器（Web + 数据库 + 缓存）。手动 `docker run` 一长串命令既繁琐又易错。**Docker Compose** 用一个 YAML 文件声明**所有容器及其配置**，一条命令把整个多服务栈拉起来——这是本地开发、小规模部署的标配。

### `docker-compose.yml` 结构
顶层声明 **services**（各容器）、**volumes**（数据卷）、**networks**（网络）。每个 service 就是一条结构化的 `docker run`。
- 关键：同一 compose 文件里的服务**自动加入同一网络**，可用**服务名互访**。

### 核心命令
| 命令 | 作用 |
| --- | --- |
| `docker compose up -d` | 按文件启动全部服务（后台） |
| `docker compose down` | 停止并删除容器/网络 |
| `docker compose ps` | 查看服务状态 |
| `docker compose logs -f` | 看全部日志 |
| `docker compose build` | 重新构建镜像 |
| `docker compose restart web` | 重启某服务 |

> 注意：新版用 `docker compose`（空格，v2 插件），旧版 `docker-compose`（连字符，独立程序）。功能一致，推荐 v2。

### 与 `docker run` 的对应
Compose 的每个 service 字段，就是 `docker run` 参数的声明式写法：
- `ports:` → `-p`
- `volumes:` → `-v`
- `environment:` → `-e`
- `depends_on:` → 启动顺序依赖

### 适用边界
- ✅ 本地开发、CI、中小型单机部署、微服务本地联调。
- ❌ 多机集群、跨节点调度、自动扩缩容 → 用 [[DevOps 概览|Kubernetes]]。

## 代码示例

```yaml
# docker-compose.yml —— 一个 Web + 数据库 + 缓存 的典型栈
services:
  web:                              # 服务名（也是网络里的主机名）
    build: ./webapp                 # 从本地 Dockerfile 构建
    ports:
      - "8000:8000"                 # 对外暴露
    environment:
      - DB_HOST=db                  # 用服务名 db 连数据库
      - REDIS_HOST=cache
    volumes:
      - ./webapp/src:/app/src       # 开发热重载
    depends_on:
      - db                          # 先启动 db（注意：只管启动顺序，不等「就绪」）
    restart: unless-stopped

  db:
    image: postgres:16
    environment:
      POSTGRES_PASSWORD: secret
      POSTGRES_DB: myapp
    volumes:
      - pgdata:/var/lib/postgresql/data   # 命名卷持久化
    # 不加 ports：仅内部服务访问，更安全

  cache:
    image: redis:7-alpine
    # 无需持久化配置，默认即可

volumes:
  pgdata:                           # 声明命名卷
```

```bash
# 一键拉起整个栈
docker compose up -d                # 构建镜像（若 build）+ 启动全部，后台
docker compose ps                   # 看各服务状态
docker compose logs -f web          # 跟踪 web 日志
docker compose exec db psql -U postgres myapp   # 进 db 执行命令

# 改了代码/Dockerfile 后重建
docker compose up -d --build        # 重建镜像再启动

# 停止并清理
docker compose down                 # 停删容器和网络（保留卷）
docker compose down -v              # 连数据卷一起删（数据清空，慎用）
```

## ⚠️ 易错点 / 面试要点

- **`depends_on` 不等于「等就绪」**：它只保证**启动顺序**（db 容器先启动），不等数据库**能接受连接**。应用要有重试逻辑（连不上等会儿再连），或用 `healthcheck` + `condition: service_healthy`。
- **服务名即主机名**：同一 compose 内，`web` 连数据库用主机名 `db`，不是 `localhost`（容器内 localhost 是它自己）。这是新手最常踩的坑。
- **数据持久化靠 `volumes`**：`docker compose down` 默认**保留**命名卷（数据不丢），加 `-v` 才删。生产数据务必挂卷。
- **改配置要 `up -d` 重建**：改了 compose 文件或 Dockerfile，需 `docker compose up -d`（必要时 `--build`），否则用的还是旧容器/镜像。
- **`build` vs `image`**：`build` 从本地 Dockerfile 构建；`image` 用现成镜像。可同时用（`build` + `image: 名字`）给构建出的镜像命名。
- **环境变量与敏感信息**：密码别硬编码进 yml（会被提交进 [[Git MOC|Git]]）。用 `.env` 文件或 `${VAR}` 引用环境变量，`.env` 加入 `.gitignore`。
- **Compose 的边界**：单机多容器编排用它；跨多台服务器、自动扩缩容、滚动升级用 Kubernetes。面试常问二者区别。

## 相关链接

- [[Docker MOC]] · [[数据卷与网络]]
- 前置：[[容器操作]]、[[镜像与 Dockerfile]]
- 编排进阶：[[DevOps 概览]]（Kubernetes）
