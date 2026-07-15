---
tags:
  - subject/docker
  - dockerfile
aliases:
  - Dockerfile
  - 构建镜像
  - docker build
  - 多阶段构建
created: 2026-07-15
status: 🟢 已完成
---

# 镜像与 Dockerfile

> [!info] 所属：[[Docker MOC]] · 上一篇 [[核心概念]] · 下一篇 [[容器操作]]

## 核心概念

**Dockerfile** 是构建镜像的「配方」——一段纯文本指令，告诉 Docker 怎么从基础镜像一步步搭出你要的环境。写好 Dockerfile 是 Docker 的核心技能，决定了镜像的大小、构建速度、安全性。

### 常用指令
| 指令 | 作用 |
| --- | --- |
| `FROM` | 基础镜像（起点，必须有） |
| `WORKDIR` | 设定工作目录（后续指令在此进行） |
| `COPY` | 把宿主文件复制进镜像（推荐，优于 ADD） |
| `RUN` | 构建时执行命令（装依赖等） |
| `ENV` | 设环境变量 |
| `EXPOSE` | 声明监听端口（文档性质，真正映射靠 `-p`） |
| `CMD` | 容器启动默认命令（可被覆盖，只有一个生效） |
| `ENTRYPOINT` | 固定的启动命令（不易覆盖，配 CMD 传参） |

### 构建缓存与指令顺序（性能关键）
Docker 按指令分层，且**每层有缓存**。一旦某层变了，它及之后所有层缓存失效、重新构建。
> **铁律：把「不常变的」放前面（装依赖），「常变的」放后面（拷代码）。** 这样改代码时，前面装依赖的层命中缓存，秒级完成。

### 多阶段构建（Multi-stage Build）
用多个 `FROM`，把「编译环境」和「运行环境」分开——最终镜像只保留运行所需，**大幅瘦身**（如 Go 应用最终镜像从 1GB 降到 20MB）。

### `.dockerignore`
类似 `.gitignore`，告诉构建时**哪些文件不传进镜像**（如 `node_modules`、`.git`），加速构建、减小上下文。

## 代码示例

```dockerfile
# 一个 Python Web 应用的 Dockerfile
FROM python:3.12-slim                # 精简基础镜像（比完整版小很多）

WORKDIR /app                         # 工作目录

# ✅ 先拷依赖文件并安装（不常变，命中缓存）
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# ⬇ 再拷业务代码（常变，放后面）
COPY . .

ENV PYTHONUNBUFFERED=1
EXPOSE 8000                          # 声明端口（文档作用）

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

```bash
# 构建镜像（在含 Dockerfile 的目录执行）
docker build -t myapp:1.0 .          # -t 名字:标签  . = 构建上下文目录
docker build -t myapp:1.0 -f Dockerfile.prod .   # -f 指定其它 Dockerfile

# 查看构建历史（每层）
docker history myapp:1.0

# 给镜像打标签、推送
docker tag myapp:1.0 registry.example.com/myapp:1.0
docker push registry.example.com/myapp:1.0
```

```dockerfile
# 多阶段构建示例：编译 + 运行分离，镜像瘦身
# ---- 阶段1：构建（含完整编译工具，体积大）----
FROM node:20 AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build                   # 产出 dist/

# ---- 阶段2：运行（只含产物 + 轻量运行时）----
FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html   # 只拷阶段1的产物
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

## ⚠️ 易错点 / 面试要点

- **指令顺序决定缓存命中**：把 `COPY requirements.txt` + `RUN pip install` 放在 `COPY . .` 之前。否则改一行代码就要重装所有依赖，慢到怀疑人生。
- **用 `slim`/`alpine` 基础镜像瘦身**：`python:3.12`（~1GB）vs `python:3.12-slim`（~150MB）vs alpine（更小但可能有兼容问题）。
- **`.dockerignore` 很重要**：不写的话 `COPY . .` 会把 `node_modules`、`.git`、本地数据全打进镜像，又大又慢还可能泄露敏感文件。
- **`COPY` vs `ADD`**：优先 `COPY`（语义清晰）；`ADD` 能自动解压 tar、拉 URL，但行为隐晦易错。
- **`CMD` vs `ENTRYPOINT`**：`CMD` 易被 `docker run` 后的命令覆盖；`ENTRYPOINT` 固定执行，`CMD` 作默认参数。需要「固定命令 + 可变参数」用 ENTRYPOINT+CMD 组合。
- **容器要前台运行**：如 nginx 要 `nginx -g "daemon off;"`，否则容器启动命令一退出，容器就退出了（容器生命周期 = 主进程生命周期）。
- **`--no-cache-dir` / 合并 RUN**：装依赖别留缓存；多条 `RUN` 能合并就合并（减少层数），如 `RUN apt-get update && apt-get install -y x && rm -rf /var/lib/apt/lists/*`。

## 相关链接

- [[Docker MOC]] · [[核心概念]]
- 跑起构建好的镜像：[[容器操作]]
- 类似 .gitignore 的 [[基础概念与配置]]
