---
tags:
  - subject/sklearn
  - basics
aliases:
  - sklearn入门
  - estimator
  - fit predict
created: 2026-07-15
status: 🟢 已完成
---

# sklearn 入门与 API 设计

> [!info] 所属：[[sklearn MOC]] · 下一篇 [[数据预处理]]

## 核心概念

sklearn 之所以好用，在于它有一套**极其统一的 API 设计**。几乎所有 estimator（估计器：模型、预处理器）都遵循同一套接口，学一个等于会一百个。

### 三大对象类型
| 类型 | 作用 | 代表 |
| --- | --- | --- |
| **Estimator**（估计器） | 从数据学参数 | 几乎所有模型/转换器 |
| **Transformer**（转换器） | 改变数据形态 | `StandardScaler`、`PCA` |
| **Predictor**（预测器） | 做预测 | `LogisticRegression`、`RandomForest` |

### 统一接口（核心范式）
- `fit(X, [y])`：**从数据学习**（训练 / 拟合参数）。所有 estimator 都有。
- `transform(X)`：**转换数据**（Transformer 专用）。
- `fit_transform(X)`：fit + transform 的便捷合体（有时更快）。
- `predict(X)`：**预测**（Predictor 专用）。
- `score(X, y)`：返回默认评估指标（分类是 accuracy，回归是 R²）。

> 口诀：**Transformer 用 `fit_transform`，Predictor 用 `fit` + `predict`**。所有模型都遵循，换算法零成本。

### 数据约定
- 特征矩阵 `X`：二维，`(样本数, 特征数)`，通常是 NumPy 数组或 pandas DataFrame。
- 标签 `y`：一维，`(样本数,)`。
- sklearn 处理的是**数值**，文本/类别需先转数值（见 [[数据预处理]]）。

### 模块结构
`sklearn.<模块>`：`linear_model`（线性模型）、`ensemble`（集成）、`tree`（树）、`svm`（支持向量机）、`neighbors`（近邻）、`cluster`（聚类）、`decomposition`（降维）、`preprocessing`（预处理）、`model_selection`（模型选择）、`metrics`（评估）。

## 代码示例

```python
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier

# 1) 准备数据
iris = load_iris()
X, y = iris.data, iris.target          # X:(150,4)  y:(150,)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# 2) 创建模型（estimator）
clf = RandomForestClassifier(n_estimators=100, random_state=42)

# 3) 统一范式：fit 训练
clf.fit(X_train, y_train)

# 4) predict 预测
y_pred = clf.predict(X_test)

# 5) score 评估（分类默认 accuracy）
print("准确率:", clf.score(X_test, y_test))   # 例如 1.0

# Transformer 的范式（同样统一）
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X_train)   # 学均值方差 + 转换
X_test_scaled = scaler.transform(X_test)    # 用训练集参数转换测试集
```

## ⚠️ 易错点 / 面试要点

- **数据泄露（data leakage）**：预处理（如标准化）必须**只用训练集 fit**，再 transform 测试集。如果用全量数据 fit 再划分，测试集信息泄露进训练，评估会虚高。正确做法见 [[数据预处理]]，或用 [[无监督学习与 Pipeline|Pipeline]] 自动避免。
- **`fit` 会覆盖**：对同一个 estimator 重复 `fit` 会重置已学参数，不是增量学习（除 `partial_fit` 的少数模型）。
- **输入要数值化、无缺失**：sklearn 一般不处理 NaN（旧版）、不直接吃字符串，需预处理。
- **`random_state` 固定可复现**：涉及随机（森林、划分）的步骤设 `random_state`，保证结果可复现、可调试。
- **分类与回归用对模型**：`RandomForestClassifier` vs `RandomForestRegressor`，标签是类别还是连续数值要选对。
- **`score` 的默认指标**：分类是 accuracy（不平衡数据会误导），回归是 R²。别只看 score，要看业务相关指标（见 [[模型评估与选择]]）。

## 相关链接

- [[sklearn MOC]] · [[数据预处理]]
- 依赖：[[数据类型与数据结构]]（NumPy/Pandas）、[[Python MOC]]
- 概念基础：[[数据结构与算法概览]]
