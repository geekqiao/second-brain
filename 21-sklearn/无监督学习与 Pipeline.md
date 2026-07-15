---
tags:
  - subject/sklearn
  - unsupervised
aliases:
  - 无监督学习
  - Pipeline
  - KMeans
  - PCA
created: 2026-07-15
status: 🟢 已完成
---

# 无监督学习与 Pipeline

> [!info] 所属：[[sklearn MOC]] · 上一篇 [[模型评估与选择]]

## 核心概念

没有标签时，用**无监督学习**发现数据的内在结构（聚类、降维）。而 **Pipeline** 则把「预处理 → 模型」串成一条流水线，是 sklearn 生产级用法，能**自动避免数据泄露**、让代码干净可复用。

### 聚类（Clustering）—— 无标签分组
把相似样本归为一类，发现数据的自然分组。
- **`KMeans`**：最常用。指定 K 个簇，算法迭代找簇中心。要**预先定 K**。
- 评估：`silhouette_score`（轮廓系数，[-1,1]，越大越好），因为没真标签。

### 降维（Dimensionality Reduction）
高维数据难可视化、易维度灾难，降到低维便于可视化/加速/去噪。
- **`PCA`（主成分分析）**：线性降维，保留方差最大的方向。保留多少维用 `n_components`。
- 用途：可视化（降到 2D 画图）、特征压缩、去噪。

### KMeans 与 PCA 也要缩放
二者都基于**距离/方差**，特征尺度不同会主导结果，必须先标准化。

### Pipeline（流水线）
把多个步骤（缩放 → 降维 → 模型）**串联成一个 estimator**，对外暴露统一接口（`fit`/`predict`）。
- **核心好处**：`fit` 时每一步**只看训练数据**，自动避免数据泄露（不用手动管理 fit/transform 顺序）。
- 可直接喂给 `cross_val_score` / `GridSearchCV`。
- `ColumnTransformer`：对不同列用不同预处理（数值列缩放、类别列独热）。

## 代码示例

```python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA
from sklearn.cluster import KMeans
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split, cross_val_score
from sklearn.datasets import load_iris
import matplotlib.pyplot as plt

X, y = load_iris(return_X_y=True)

# 1) 聚类：KMeans（必须先标准化）
kmeans_pipe = Pipeline([
    ('scaler', StandardScaler()),
    ('kmeans', KMeans(n_clusters=3, random_state=42, n_init=10)),
])
labels = kmeans_pipe.fit_predict(X)          # 无监督，没有 y
print("簇标签:", labels[:10])

# 2) 降维：PCA 可视化
pca_pipe = Pipeline([
    ('scaler', StandardScaler()),
    ('pca', PCA(n_components=2)),
])
X_2d = pca_pipe.fit_transform(X)
plt.scatter(X_2d[:,0], X_2d[:,1], c=y); plt.title('PCA 2D'); plt.show()

# 3) Pipeline 串联「标准化 + 模型」（生产标准写法）
clf_pipe = Pipeline([
    ('scaler', StandardScaler()),
    ('clf', LogisticRegression(max_iter=200)),
])
# 一行就完成训练，且自动避免数据泄露
clf_pipe.fit(X, y)
print("Pipeline 准确率:", clf_pipe.score(X, y))

# 可直接交叉验证 / 网格搜索（对整个流程）
scores = cross_val_score(clf_pipe, X, y, cv=5)
print(f"5折CV: {scores.mean():.3f} ± {scores.std():.3f}")

# 4) 网格搜索调 Pipeline 内部参数：用 步骤名__参数名
from sklearn.model_selection import GridSearchCV
grid = GridSearchCV(clf_pipe, {'clf__C': [0.1, 1, 10]}, cv=5)
grid.fit(X, y)
print("最优 C:", grid.best_params_['clf__C'])

# 5) ColumnTransformer：数值列与类别列分别处理
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import OneHotEncoder
import numpy as np
# 假设列 0,1 是数值，列 2 是类别
pre = ColumnTransformer([
    ('num', StandardScaler(), [0, 1]),
    ('cat', OneHotEncoder(handle_unknown='ignore'), [2]),
])
full = Pipeline([('pre', pre), ('clf', LogisticRegression())])
```

## ⚠️ 易错点 / 面试要点

- **生产代码用 Pipeline**：手动 fit/transform 多个步骤容易出错（尤其数据泄露），Pipeline 自动管理且可整体调参、整体保存。
- **Pipeline 避免数据泄露**：`fit` 时后续步骤看不到测试集数据，从机制上杜绝了泄露（相比手动 `fit_transform(X)` 全量）。
- **KMeans 要先标准化、且要定 K**：尺度不同会让大尺度特征主导聚类；K 的选择可用肘部法则或轮廓系数。
- **PCA 保留方差解释**：`pca.explained_variance_ratio_` 看每维保留了多少信息，别盲目降到固定维数而丢太多信息。
- **调 Pipeline 参数用双下划线**：`步骤名__参数名`（如 `clf__C`），这是 GridSearchCV 访问 Pipeline 内部参数的语法。
- **`fit_predict` vs `fit` + `predict`**：聚类常用 `fit_predict` 一步到位并返回簇标签。
- **无监督评估难**：没有真标签，靠轮廓系数等内部指标，或人工抽样验证簇的含义。

## 相关链接

- [[sklearn MOC]] · [[模型评估与选择]]
- 预处理细节：[[数据预处理]]
- 深度学习的训练循环思想相通：[[训练循环]]
