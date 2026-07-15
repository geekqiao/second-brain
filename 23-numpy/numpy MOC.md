---
tags:
  - subject/numpy
  - moc
aliases:
  - numpy
  - np
  - ndarray
  - numpy索引
created: 2026-07-15
status: 🟢 已完成
---

# numpy MOC

> [!info] 重点科目 · [[🗺️ 知识地图 MOC]] · [[Home]]

## 📖 概述

**NumPy** 是 Python 数值计算的基石——它提供高效的 **n 维数组（ndarray）** 和向量化运算，是几乎所有数据科学/ML 库（[[pandas MOC|pandas]]、[[sklearn MOC|sklearn]]、[[PyTorch MOC|PyTorch]]）的底层依赖。掌握 numpy = 掌握「用数组思维批量处理数据，告别 for 循环」。

> 学 numpy 的核心：理解 **ndarray（形状+dtype）**、**向量化与广播**（不用写循环）、**视图 vs 副本**（无数 bug 的源头）。这些通了，pandas/torch 的张量操作都是同一套思路。

## 📚 学习路径

1. [[ndarray 创建与属性]] — 创建数组、shape/dtype/ndim
2. [[索引与切片]] — 基础/布尔/花式索引、视图 vs 副本
3. [[广播与向量化运算]] — 广播规则、为什么快
4. [[数组变形与拼接]] — reshape/transpose/concatenate
5. [[通用函数与聚合]] — ufunc、axis 聚合、where

## 🧠 核心速查表

| 需求 | 写法 |
| --- | --- |
| 建数组 | `np.array([1,2,3])` / `np.zeros((2,3))` / `np.arange(0,10,2)` |
| 看属性 | `a.shape` / `a.dtype` / `a.ndim` / `a.size` |
| 改形状 | `a.reshape(2,3)` |
| 切片 | `a[1:3]` / `a[a>0]`（布尔）/ `a[[0,2]]`（花式） |
| 逐元素运算 | `a * 2` / `a + b`（广播） |
| 聚合 | `a.sum()` / `a.mean(axis=0)` |
| 拼接 | `np.concatenate([a,b])` / `np.vstack` / `np.hstack` |
| 条件赋值 | `np.where(cond, x, y)` |
| 随机 | `np.random.randn(3,4)` / `np.random.seed(42)` |

## 📈 进阶方向

- **线性代数**：`np.linalg`（矩阵乘 `@`、求逆、特征值、SVD）。
- **随机数**：`np.random` 新版 Generator API（`np.random.default_rng()`）。
- **性能**：向量化代替循环、预分配数组、`numba` 加速、避免广播陷阱。
- **衔接**：[[pandas MOC|pandas]]（表格 = 带标签的 numpy）、[[PyTorch MOC|PyTorch]]（GPU 张量，API 极似 numpy）。

---

*🔗 返回 [[🗺️ 知识地图 MOC]] · [[Home]]*
