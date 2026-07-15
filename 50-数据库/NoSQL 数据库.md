---
tags:
  - subject/database
  - nosql
aliases:
  - NoSQL
  - 非关系数据库
  - MongoDB
  - Redis
created: 2026-07-15
status: 🟢 已完成
---

# NoSQL 数据库

> [!info] 所属：[[数据库 MOC]] · 上一篇 [[事务与并发控制]] · 下一篇 [[向量数据库]]

## 核心概念

**NoSQL（Not Only SQL）** 是为关系型数据库（[[关系模型与 SQL 基础|SQL]]）搞不定的场景而生的——海量数据、灵活结构、极致吞吐、特殊数据形态。它放弃部分一致性/关系能力，换来**扩展性、灵活性、性能**。理解 NoSQL 的关键是知道**四大类型**及各自适用场景。

### 四大类型
| 类型 | 数据模型 | 代表 | 适合 |
| --- | --- | --- | --- |
| **键值 KV** | `key → value` | **Redis**、DynamoDB | 缓存、会话、高速读写 |
| **文档 Document** | JSON/BSON 文档 | **MongoDB**、CouchDB | 灵活结构、内容管理、半结构化 |
| **宽列 Column-family** | 行可动态加列 | Cassandra、HBase | 海量写入、时序、日志 |
| **图 Graph** | 节点 + 边 + 属性 | **Neo4j**、JanusGraph | 关系密集（社交、推荐、反欺诈） |

### SQL vs NoSQL 取舍
| 维度 | 关系型 SQL | NoSQL |
| --- | --- | --- |
| 结构 | 固定 schema，强一致 | 灵活/无 schema |
| 关系 | 强（JOIN、外键） | 弱（去规范化、聚合） |
| 事务 | 强 ACID | 多为最终一致（BASE） |
| 扩展 | 偏纵向（单机变强） | 偏横向（多机分片） |
| 查询 | SQL 通用 | 各自有 API |

> **不是替代，而是互补**。现代架构常**混合用**：PostgreSQL 存核心交易数据、Redis 做缓存、MongoDB 存灵活内容、向量库做语义检索。

### CAP 定理（复习 [[系统设计概览]]）
分布式存储三选二：**C** 一致性、**A** 可用性、**P** 分区容忍。网络必分区（P 必选），故在 **CP**（强一致）与 **AP**（高可用）间权衡。NoSQL 多偏 **AP**（最终一致 BASE）。

### BASE（最终一致性）
- **B**asically **A**vailable：基本可用。
- **S**oft state：软状态（允许中间态）。
- **E**ventually consistent：最终一致（不是立刻一致，但最终会一致）。

## 代码示例

```javascript
// MongoDB（文档库）：存灵活 JSON 文档，无需预定义表结构
db.users.insertOne({
  name: "Ada",
  age: 30,
  tags: ["ml", "db"],              // 字段可任意嵌套
  address: { city: "BJ", zip: "100000" }
});
// 查询：按任意嵌套字段
db.users.find({ "address.city": "BJ", age: { $gt: 25 } });
// 建索引
db.users.createIndex({ "address.city": 1 });
```

```python
# Redis（键值库）：纯内存，极快，常做缓存
import redis
r = redis.Redis(host='localhost', port=6379, decode_responses=True)

r.set('user:1:name', 'Ada')                    # 简单 KV
r.setex('session:abc', 3600, 'user_id=1')      # 带过期（缓存/会话）
r.incr('page_views')                           # 原子计数（防并发竞态）
r.lpush('queue:tasks', 'task1', 'task2')       # 列表（消息队列）

# 缓存模式：先查缓存，未命中再查 DB（见 [[系统设计概览]] 的缓存层）
val = r.get('user:1:name')
if val is None:
    val = query_db(1)          # 回源
    r.setex('user:1:name', 600, val)   # 回填缓存
```

```cypher
// Neo4j（图数据库）：用节点和边表达关系，关系查询极快
// 「Ada 认识的朋友的朋友」在 SQL 里要多重 JOIN，图库一句搞定
CREATE (ada:Person {name:'Ada'})-[:KNOWS]->(bob:Person {name:'Bob'})
MATCH (p:Person {name:'Ada'})-[:KNOWS*1..2]->(fof:Person)
RETURN fof.name
```

## ⚠️ 易错点 / 面试要点

- **NoSQL 不是「比 SQL 好」**：它是为特定场景的**取舍**。强一致、复杂关系、事务、SQL 生态 → 还是关系库。NoSQL 强在扩展性与灵活。
- **选型看数据形态**：键值→缓存；文档→半结构内容；宽列→海量写时序；图→关系网络。选错类型事倍功半。
- **最终一致性是 NoSQL 常态**：读到的可能是旧数据（AP）。需要强一致的金融/库存场景要谨慎，或选 CP 系统。
- **MongoDB 的 schemaless 陷阱**：「无 schema」不等于「该无 schema」。文档结构混乱后查询和维护灾难。仍需设计稳定的文档结构。
- **Redis 不是持久化数据库**：内存为主，虽有 RDB/AOF 持久化，但定位是**缓存/高速场景**。当成主存储有丢数据风险（尽管有持久化）。
- **分片（sharding）很复杂**：NoSQL 的横向扩展靠分片，但跨分片查询、事务、再平衡都是难题。别以为用了 NoSQL 就自动解决扩展。
- **混合架构是主流**：核心交易用 PostgreSQL，缓存用 Redis，灵活内容用 MongoDB，语义检索用向量库（[[向量数据库]]）。各取所长。

## 相关链接

- [[数据库 MOC]] · [[事务与并发控制]]
- 关系型对照：[[关系模型与 SQL 基础]]
- 选型原理：[[系统设计概览]]（CAP、缓存）
