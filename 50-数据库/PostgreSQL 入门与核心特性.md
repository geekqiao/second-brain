---
tags:
  - subject/database
  - postgresql
aliases:
  - PostgreSQL
  - Postgres
  - psql
  - JSONB
created: 2026-07-15
status: 🟢 已完成
---

# PostgreSQL 入门与核心特性

> [!info] 所属：[[数据库 MOC]] · 上一篇 [[关系模型与 SQL 基础]] · 下一篇 [[表设计与关系建模]]

## 核心概念

**PostgreSQL（Postgres）** 是最强大的开源关系型数据库——严格遵循 SQL 标准、类型丰富、扩展性极强、可靠性高。它不只是「存数据」，更像个数据平台：原生支持 JSON、全文检索、地理（PostGIS）、甚至向量（pgvector）。近年备受推崇，是很多公司的首选。

### 为什么选 PostgreSQL
- **标准 + 严谨**：SQL 标准符合度高，数据完整性约束强。
- **类型极其丰富**：数组、JSON/JSONB、UUID、范围类型、几何类型、自定义类型。
- **可扩展**：扩展机制（`CREATE EXTENSION`）——PostGIS（地理）、pgvector（向量）、pg_trgm（模糊匹配）等。
- **MVCC 并发**：读不阻塞写、写不阻塞读（见 [[事务与并发控制]]）。
- **成熟可靠**：WAL 崩溃恢复、流复制、点在时间恢复（PITR）。

### psql（命令行客户端）
最强大的 PG 交互工具。`psql -U user -d dbname` 连接。常用元命令：`\d`（看表结构）、`\dt`（列表）、`\l`（列库）、`\q`（退出）。

### 核心特色类型
- **`JSONB`**：二进制 JSON，**可索引、可查询**。让 PG 兼具文档库的灵活性（一个表里存半结构化数据）。
- **数组**：列可直接存数组（`INTEGER[]`），并支持数组运算。
- **`UUID`**：全局唯一 ID，分布式友好。
- **枚举/复合/范围类型**：建模复杂领域。

### 全文检索
原生支持中文/英文全文搜索：`tsvector`（分词后存储）+ `tsquery`（查询）+ GIN 索引。小型搜索需求不用上 Elasticsearch。

### schema（模式）
PG 里 `schema` 是数据库下的命名空间（如 `public`），用来**分组管理**对象，比 MySQL 的「database=schema」更灵活。

## 代码示例

```sql
-- 1) JSONB：半结构化数据 + 可查询可索引
CREATE TABLE events (
    id    SERIAL PRIMARY KEY,
    name  TEXT,
    meta  JSONB                       -- 存任意 JSON
);
INSERT INTO events (name, meta) VALUES
  ('click', '{"page":"home","duration":12}'),
  ('view',  '{"page":"about","user":"ada"}');

-- JSONB 查询（->> 取文本，-> 取 JSON）
SELECT name FROM events WHERE meta->>'page' = 'home';
SELECT meta->>'duration' FROM events WHERE name = 'click';
-- 给 JSONB 建 GIN 索引后可高效查键值
CREATE INDEX idx_events_meta ON events USING GIN (meta);

-- 2) 数组类型
CREATE TABLE tags_post (
    id    SERIAL PRIMARY KEY,
    tags  TEXT[]                       -- 文本数组
);
INSERT INTO tags_post (tags) VALUES (ARRAY['py', 'db']);
SELECT * FROM tags_post WHERE 'py' = ANY(tags);   -- 数组包含某元素

-- 3) UUID 主键（需扩展）
CREATE EXTENSION IF NOT EXISTS "pgcrypto";
CREATE TABLE sessions (
    id  UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    ts  TIMESTAMP DEFAULT now()
);

-- 4) 全文检索
CREATE TABLE docs (id SERIAL PRIMARY KEY, body TEXT);
CREATE INDEX idx_docs_fts ON docs USING GIN (to_tsvector('english', body));
SELECT body FROM docs
WHERE to_tsvector('english', body) @@ to_tsquery('english', 'quick & fox');

-- 5) psql 常用元命令（在 psql 里执行，不是 SQL）
-- \dt          列出表
-- \d users     查看 users 表结构（列、类型、索引、约束）
-- \l           列出所有数据库
-- \q           退出

-- 6) 视图：把常用复杂查询存成「虚拟表」
CREATE VIEW active_users AS
SELECT id, name FROM users WHERE email IS NOT NULL;
SELECT * FROM active_users;
```

## ⚠️ 易错点 / 面试要点

- **`JSON` vs `JSONB`**：`JSON` 存原文（保留空格/顺序，慢、不可高效索引）；`JSONB` 存解析后的二进制（可索引、去重、快，但稍占空间、不保顺序）。**默认用 JSONB**。
- **`SERIAL` vs `GENERATED ... AS IDENTITY`**：`SERIAL` 是老语法（内部用序列），新标准推荐 `id INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY`（更符合 SQL 标准、不易出错）。
- **大小写敏感**：PG 默认对**字符串比较大小写敏感**（`'Ada' != 'ada'`）。要忽略大小写用 `ILIKE` 或 `LOWER()`。表名未加引号会折叠成小写。
- **schema 不要和「表结构」混淆**：PG 的 schema 是命名空间（`public.books`），不是「表设计」。概念易混。
- **扩展要先启用**：`pgcrypto`、`pgvector`、`pg_trgm` 等需 `CREATE EXTENSION` 才能用。
- **PSQL 是运维利器**：`\d`、`\timing`（计时）、`\x`（宽表竖排）等元命令极实用，比 GUI 高效。配合 [[Docker MOC|Docker]] 跑 PG 很方便。
- **字符列别用 VARCHAR(n) 限死**：PG 里 `TEXT` 和 `VARCHAR(n)` 性能一样，除非业务真要限制长度，用 `TEXT` 更灵活。

## 相关链接

- [[数据库 MOC]] · [[关系模型与 SQL 基础]]
- 设计表：[[表设计与关系建模]]
- 性能：[[索引与查询优化]]
- 向量扩展：[[向量数据库]]（pgvector）
