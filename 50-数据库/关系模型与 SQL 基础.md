---
tags:
  - subject/database
  - relational
aliases:
  - 关系模型
  - SQL基础
  - join
created: 2026-07-15
status: 🟢 已完成
---

# 关系模型与 SQL 基础

> [!info] 所属：[[数据库 MOC]] · 下一篇 [[PostgreSQL 入门与核心特性]]

## 核心概念

关系型数据库（RDBMS）建立在**关系模型**上：数据以**二维表**存储，表之间靠**键**关联。**SQL** 是操作它的标准语言。这一篇是所有关系库（PostgreSQL/MySQL/SQLite）的通用底座，学透它，换哪个库都通。

### 关系模型三要素
- **表（Table/Relation）**：由行（row/tuple）和列（column/attribute）组成。
- **键**：
  - **主键（Primary Key）**：唯一标识一行，非空唯一。
  - **外键（Foreign Key）**：指向另一表的主键，建立**关系**。
- **关系（Relationship）**：表与表的关联靠外键，常见三种：1:1、1:N（最常见）、N:M（用中间表实现）。

### 关系的完整性
- **实体完整性**：主键非空唯一。
- **参照完整性**：外键必须指向存在的主键（或为空），保证不会引用「幽灵」数据。

### SQL 四大类（复习 [[数据库基础]]，这里展开）
| 类别 | 作用 | 关键字 |
| --- | --- | --- |
| DDL | 定义结构 | `CREATE / ALTER / DROP / TRUNCATE` |
| DML | 操作数据 | `INSERT / UPDATE / DELETE` |
| **DQL** | 查询数据 | `SELECT`（最核心） |
| DCL | 权限控制 | `GRANT / REVOKE` |

### JOIN（连接）—— 关系库的灵魂
把多张表按某字段关联查询。核心四种：
- **INNER JOIN**：交集（两表都有的）。
- **LEFT JOIN**：保留左表全部，右表没有的填 NULL。
- **RIGHT JOIN**：保留右表全部。
- **FULL OUTER JOIN**：并集，缺的填 NULL。

## 代码示例

```sql
-- DDL：建表（带主键、外键、约束）
CREATE TABLE users (
    id    SERIAL PRIMARY KEY,            -- SERIAL = 自增整数
    name  VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL
);

CREATE TABLE orders (
    id       SERIAL PRIMARY KEY,
    user_id  INTEGER REFERENCES users(id) ON DELETE CASCADE,  -- 外键，删用户连带删订单
    amount   NUMERIC(10,2) CHECK (amount >= 0),               -- 约束：金额非负
    created  TIMESTAMP DEFAULT now()
);

-- DML：增删改
INSERT INTO users (name, email) VALUES ('Ada', 'ada@x.com');
UPDATE users SET name = 'Ada L' WHERE id = 1;
DELETE FROM users WHERE id = 1;

-- DQL：基本查询
SELECT name, email FROM users WHERE name LIKE 'A%' ORDER BY name DESC LIMIT 10;

-- JOIN：查每个订单的下单人和金额（保留所有订单→LEFT JOIN）
SELECT o.id, u.name, o.amount
FROM orders o
LEFT JOIN users u ON o.user_id = u.id;

-- 聚合 + 分组：每个用户的订单数与总金额
SELECT u.name, COUNT(o.id) AS order_cnt, COALESCE(SUM(o.amount), 0) AS total
FROM users u
LEFT JOIN orders o ON o.user_id = u.id
GROUP BY u.name
HAVING SUM(o.amount) > 100;          -- HAVING 过滤聚合后的结果（WHERE 过滤行）
```

## ⚠️ 易错点 / 面试要点

- **WHERE vs HAVING**：`WHERE` 在分组前过滤**行**，`HAVING` 在分组后过滤**组**（聚合条件）。顺序：`WHERE → GROUP BY → HAVING`。
- **LEFT JOIN 防丢数据**：要保留「可能没匹配」的一侧用 LEFT JOIN；用 INNER JOIN 会丢掉没下单的用户。
- **`NULL` 的诡异**：`NULL` 参与比较结果是 `UNKNOWN`（不是 true/false）。`WHERE x != 1` 会**排除** x 为 NULL 的行；判空用 `IS NULL` / `IS NOT NULL`。聚合 `COUNT(col)` 不计 NULL 行，`COUNT(*)` 计所有行。
- **外键要建索引**：外键列默认**不一定**有索引，大表 JOIN/删除会慢。生产环境通常给外键加索引。
- **`ON DELETE` 行为**：`CASCADE`（连带删）、`SET NULL`、`RESTRICT`（阻止删）。选错会误删数据或删不掉。
- **不要用 SELECT \***：明确列出列，避免表结构变化时出错，且利于优化与索引覆盖。
- **N:M 用中间表**：多对多不能直接加外键，要建一张连接表（含两个外键）。

## 相关链接

- [[数据库 MOC]] · [[PostgreSQL 入门与核心特性]]
- 概念入门：[[数据库基础]]
- 设计表结构：[[表设计与关系建模]]
