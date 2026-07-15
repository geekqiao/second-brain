---
tags:
  - subject/sklearn
  - moc
aliases:
  - sklearn
  - scikit-learn
  - sklearn索引
created: 2026-07-15
status: 🟢 已完成
---

# sklearn MOC

> [!info] 重点科目 · [[🗺️ 知识地图 MOC]] · [[Home]]

## 📖 概述

**scikit-learn（sklearn）** 是 Python 最经典的**经典机器学习库**——覆盖分类、回归、聚类、降维、预处理、模型选择的全流程，API 高度统一、易上手。它是入门机器学习、做表格数据建模、快速搭建 baseline 的首选工具。深度学习另见 [[PyTorch MOC]]。

> 学 sklearn 的核心不是背算法，而是吃透它**统一的 API 设计**（`fit` / `predict` / `transform` 范式）。一旦理解这套范式，所有模型/预处理器的用法都如出一辙，换算法就像换零件。

## 📚 学习路径

1. [[sklearn 入门与 API 设计]] — estimator 范式、模块结构、第一个模型
2. [[数据预处理]] — 划分、标准化、编码、缺失值
3. [[监督学习算法]] — 线性/树/森林/SVM/KNN 一览
4. [[模型评估与选择]] — 指标、交叉验证、网格搜索
5. [[无监督学习与 Pipeline]] — KMeans/PCA + Pipeline 串联流程

## 🧠 核心速查表

| 需求 | 首选 |
| --- | --- |
| 划分训练/测试集 | `train_test_split(X, y)` |
| 标准化 | `StandardScaler().fit_transform(X)` |
| 分类 | `LogisticRegression` / `RandomForestClassifier` |
| 回归 | `LinearRegression` / `RandomForestRegressor` |
| 聚类 | `KMeans` |
| 降维 | `PCA` |
| 评估 | `accuracy_score` / `classification_report` |
| 交叉验证 | `cross_val_score` |
| 调参 | `GridSearchCV` |
| 串联流程 | `Pipeline([('scaler',...),('clf',...)])` |

## 📈 进阶方向

- **特征工程**：特征选择、衍生特征、目标编码。
- **梯度提升**：在表格数据上通常更强 → XGBoost / LightGBM / CatBoost（sklearn 接口兼容）。
- **不平衡数据**：`imbalanced-learn` 过采样/欠采样。
- **模型解释**：SHAP / feature_importances_ 看模型怎么决策。

---

*🔗 返回 [[🗺️ 知识地图 MOC]] · [[Home]]*
