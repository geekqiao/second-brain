---
tags:
  - subject/docker
  - cheatsheet
aliases:
  - docker命令
  - docker速查
  - docker cheatsheet
  - docker run
created: 2026-07-15
status: 🟢 已完成
---

# Docker 常用命令速查表

> [!info] 速查附录 · [[Docker MOC]] · 上一篇 [[Docker Compose]]
> 概念与原理见前 5 篇；本页是**常用命令清单**，按场景分组，复制即用。

## 🖼️ 镜像（Image）
```bash
docker pull <image>:<tag>                  # 拉取镜像（不写 tag 默认 latest）
docker images                              # 列本地镜像
docker image ls                            # 同上（新版语法）
docker build -t <name>:<tag> .             # 用当前目录 Dockerfile 构建镜像
docker build -t myapp . -f Dockerfile.prod # 指定 Dockerfile
docker tag <src> <dst>                     # 打标签
docker push <image>:<tag>                  # 推送到仓库
docker rmi <image>                         # 删除镜像
docker history <image>                     # 看镜像分层（每条指令）
docker image prune -a                      # 删除所有未使用的镜像
```

## 📦 容器（Container）—— 生命周期
```bash
docker run [选项] <image>                   # 创建并启动（最核心，见下表）
docker ps                                  # 运行中的容器
docker ps -a                               # 全部容器（含已停）
docker start <容器>                         # 启动已停止的容器
docker stop <容器>                          # 优雅停止
docker restart <容器>                       # 重启
docker rm <容器>                            # 删除已停止容器
docker rm -f <容器>                         # 强制删（运行中也删）
```

### `docker run` 常用选项
```bash
docker run -d --name web -p 8080:80 -e ENV=prod -v mydata:/app/data nginx:1.25
#   -d              后台运行
#   --name          容器名
#   -p 宿主:容器      端口映射
#   -e KEY=VAL      环境变量（也可 --env-file 指定文件）
#   -v 宿主:容器      挂载数据卷/目录（持久化）
#   -it             交互式终端（进容器）
#   --rm            停止后自动删除（临时容器）
#   --network       加入指定网络
docker run --rm -it alpine sh              # 一次性进 alpine 试试
```

## 🔍 容器调试与信息
```bash
docker logs <容器>                          # 看日志（应用的 stdout/stderr）
docker logs -f <容器>                       # 实时跟踪（类似 tail -f）
docker logs --tail 100 <容器>               # 最后 100 行
docker exec -it <容器> bash                 # 进运行中的容器开 bash（alpine 用 sh）
docker exec -it <容器> <cmd>                # 在容器内执行一条命令
docker top <容器>                           # 容器内进程
docker stats                               # 实时 CPU/内存（类似 top）
docker inspect <容器>                       # 详细配置（JSON）
docker cp <容器>:/path ./local              # 容器 → 宿主 复制文件
docker cp ./local <容器>:/path              # 宿主 → 容器
```

## 💾 数据卷（Volume）
```bash
docker volume create <name>                # 创建命名卷
docker volume ls                           # 列所有卷
docker volume inspect <name>               # 卷详情
docker volume rm <name>                    # 删除卷
# 挂载：docker run -v <卷名>:/app/data   （命名卷，Docker 管理）
#       docker run -v /宿主/路径:/app/data （bind mount，开发热重载）
```

## 🌐 网络（Network）
```bash
docker network ls                          # 列网络
docker network create <name>               # 创建自定义网络
docker network inspect <name>              # 网络详情与成员容器
docker network connect <net> <容器>         # 把容器加入网络
docker network rm <net>                    # 删除网络
# 同一自定义网络内，容器可用「容器名」互访（自动 DNS）
```

## 🐙 Docker Compose（多容器）
```bash
docker compose up -d                       # 按 compose 文件启动全部（后台）
docker compose up -d --build               # 改代码后重建镜像再启动
docker compose down                        # 停止并删容器/网络
docker compose down -v                     # 连数据卷一起删（数据清空！）
docker compose ps                          # 各服务状态
docker compose logs -f <服务>               # 跟踪某服务日志
docker compose exec <服务> <cmd>            # 在某服务容器执行命令
docker compose restart <服务>               # 重启某服务
docker compose build                       # 重新构建镜像
```

## 🧹 清理
```bash
docker system prune                        # 清悬空镜像、停止容器、无用网络
docker system prune -a                     # 连未使用的镜像一起清（更彻底）
docker system prune -a --volumes           # 连未使用的数据卷也清（⚠️ 数据）
docker image prune -a                      # 只清镜像
docker container prune                     # 只清停止的容器
docker system df                           # 看磁盘占用情况
```

## 📄 Dockerfile 关键指令速查
```dockerfile
FROM python:3.12-slim          # 基础镜像（起点）
WORKDIR /app                   # 工作目录
COPY requirements.txt .        # 复制文件进镜像（推荐，优于 ADD）
RUN pip install -r requirements.txt   # 构建时执行命令
COPY . .                       # 拷业务代码（放依赖安装之后，命中缓存）
ENV KEY=VAL                    # 环境变量
EXPOSE 8000                    # 声明端口（文档性，真正映射靠 -p）
CMD ["uvicorn","main:app"]     # 启动默认命令（可被覆盖）
ENTRYPOINT ["python"]          # 固定启动命令（配 CMD 传参）
```

> [!tip] 三个救命命令
> - **容器起不来/行为异常** → `docker logs <容器>` + `docker exec -it <容器> sh`
> - **端口被占/连不上** → `docker ps` 看端口映射，`docker network inspect`
> - **磁盘满了** → `docker system df` 查占用，`docker system prune -a` 清理

## 相关链接

- [[Docker MOC]] · [[核心概念]] · [[容器操作]] · [[Docker Compose]]
