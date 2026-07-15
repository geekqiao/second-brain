---
tags:
  - subject/database
  - moc
aliases:
  - 数据库
  - DB
  - PostgreSQL
  - 数据库索引
created: 2026-07-15
status: 🟢 已完成
---

# 数据库 MOC

> [!info] 重点科目 · [[🗺️ 知识地图 MOC]] · [[Home]]

## 📖 概述

几乎所有应用的核心都是**数据**——而数据库负责把它们**持久化、可查询、一致、并发安全**地存起来。本科目以 **PostgreSQL**（最强大的开源关系型数据库）为主线深入，再横向覆盖**非关系型（NoSQL）**与**向量数据库**两大新形态。

> 学数据库的关键：先吃透**关系模型 + SQL + 事务**（这是所有关系库的通用底座），再学**索引与查询优化**（决定快慢），最后理解不同**数据库类型**的取舍——什么场景用什么库。向量库尤其和 [[PyTorch MOC]] 的 embedding / RAG 紧密相关。

## 📚 学习路径

### 关系型 · PostgreSQL（深入）
1. [[关系模型与 SQL 基础]] — 表/键/关系、SQL 四大类、join
2. [[PostgreSQL 入门与核心特性]] — psql、丰富类型、JSONB、全文检索
3. [[表设计与关系建模]] — 范式、约束、1:1/1:N/N:M 关系建模
4. [[索引与查询优化]] — B-tree/GIN/BRIN、EXPLAIN、避坑
5. [[事务与并发控制]] — ACID、隔离级别、MVCC、锁

### 非关系型与向量（专题）
6. [[NoSQL 数据库]] — KV/文档/列族/图、CAP、选型
7. [[向量数据库]] — embedding、相似度、ANN、pgvector、RAG

## 🧠 核心速查表

| 需求 | 选型 |
| --- | --- |
| 强一致、关系复杂、SQL | PostgreSQL / MySQL |
| 缓存、高速 KV | Redis |
| 灵活文档结构 | MongoDB |
| 关系密集（社交/推荐） | Neo4j（图） |
| 海量写入宽行 | Cassandra（列族） |
| 语义搜索 / RAG | 向量库（pgvector / Milvus / Qdrant） |
| 全文检索 | PostgreSQL（tsvector）/ Elasticsearch |

## 📈 进阶方向

- **PostgreSQL 运维**：备份恢复（pg_dump/WAL）、复制、分区、扩展生态。
- **性能与扩展**：读写分离、分片、连接池（见 [[系统设计概览]]）。
- **向量与 AI**：[[向量数据库]] 的 RAG 流水线，配合 [[PyTorch MOC]] 做 embedding、[[sklearn MOC]] 做召回后排序。

---

*🔗 返回 [[🗺️ 知识地图 MOC]] · [[Home]]*
