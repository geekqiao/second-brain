---
tags:
  - subject/numpy
  - basics
aliases:
  - ndarray创建
  - numpy数组
  - numpy属性
created: 2026-07-15
status: 🟢 已完成
---

# ndarray 创建与属性

> [!info] 所属：[[numpy MOC]] · 下一篇 [[索引与切片]]

## 核心概念

**ndarray（n-dimensional array）** 是 numpy 的核心数据结构——一个**同质、固定类型、多维**的数值数组。和 Python list 的关键区别：ndarray 元素类型统一、内存连续、运算极快。

### 创建数组
| 方式 | 示例 | 用途 |
| --- | --- | --- |
| 从列表 | `np.array([1,2,3])` | 手动构造 |
| 全零/全一/空 | `np.zeros((2,3))` / `np.ones(...)` / `np.empty(...)` | 预分配 |
| 等差数列 | `np.arange(0, 10, 2)` | 范围（不含上界） |
| 等分 | `np.linspace(0, 1, 5)` | 0~1 均匀 5 个点（含两端） |
| 单位矩阵 | `np.eye(3)` | 对角线为 1 |
| 随机 | `np.random.randn(2,3)` | 标准正态；`randint`/`rand` 等 |

### 四大属性
- `a.shape`：形状（各维大小组成的元组），如 `(2, 3)`。
- `a.dtype`：元素类型，如 `int64`、`float64`。**numpy 默认 float64**（不是 Python 的 int）。
- `a.ndim`：维度数（轴数）。
- `a.size`：元素总数 = `shape` 各项之积。

### dtype（类型）很重要
numpy 数组类型固定。混入不同类型会被**向上转换**（int→float→str）。运算性能和精度都依赖 dtype。可指定 `dtype=np.float32` 省内存（深度学习常用）。

### 维度术语
- 0D 标量 `()`、1D 向量 `(3,)`、2D 矩阵 `(2,3)`、3D+ 张量。注意 1D 数组 shape 是 `(3,)` 不是 `(3,1)`。

## 代码示例

```python
import numpy as np

# 1) 创建
a = np.array([1, 2, 3])                 # 1D，[1 2 3]
b = np.array([[1, 2], [3, 4]])          # 2D 矩阵
zeros = np.zeros((2, 3))                # 2x3 全零（默认 float64）
seq = np.arange(0, 10, 2)               # [0 2 4 6 8]（不含 10）
lin = np.linspace(0, 1, 5)              # [0. 0.25 0.5 0.75 1.]（含两端）

# 2) 属性
print(a.shape, a.dtype, a.ndim, a.size) # (3,) int64 1 3
print(b.shape)                          # (2, 2)

# 3) dtype 控制
f = np.array([1, 2, 3], dtype=np.float32)   # 指定 float32（省内存）
mixed = np.array([1, 2.5, 3])               # int+float → 自动转 float64
print(mixed.dtype)                          # float64

# 4) 随机（设种子可复现）
rng = np.random.default_rng(42)             # 新版推荐 Generator
x = rng.standard_normal((2, 3))             # 2x3 标准正态
y = rng.integers(0, 10, size=5)             # [0,10) 的 5 个随机整数

# 5) 改 dtype / 改形状（详见变形笔记）
a_float = a.astype(np.float64)              # 类型转换，返回新数组
c = np.arange(6).reshape(2, 3)              # 一维 → 二维（详见变形笔记）
```

## ⚠️ 易错点 / 面试要点

- **`np.arange` 不含上界**：`np.arange(0, 5)` 是 `0,1,2,3,4`。浮点步长时 `arange` 可能因精度出意外，用 `linspace`（按个数等分）更稳。
- **默认 float64**：`np.zeros(3)` 是 float64 不是 int。要整数写 `dtype=int`。
- **类型自动提升**：`np.array([1, 2.5])` 全变 float；混字符串全变 str（性能崩溃）。保持数组类型纯净。
- **1D shape 是 `(3,)`**：不是 `(3)`（那是括号包整数）也不是 `(3,1)`（那是二维）。`(3,)` 和 `(1,3)`、`(3,1)` 行为不同（影响广播）。
- **`np.empty` 不是全零**：它只分配内存不初始化，值是「垃圾」，需随后填值。比 zeros 快但别直接用其值。
- **`default_rng` 新 API**：旧 `np.random.seed/randn` 仍能用，但官方推荐 `np.random.default_rng(seed)`（更高质量、更现代）。
- **形状要匹配**：很多 bug 来自 shape 不符，养成随时 `print(a.shape)` 的习惯。

## 相关链接

- [[numpy MOC]] · [[索引与切片]]
- 张量基础（GPU 版）：[[01-张量基础]]（PyTorch，API 极似 numpy）
