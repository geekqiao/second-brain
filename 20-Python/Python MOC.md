---
tags:
  - subject/python
  - moc
aliases:
  - Python
  - py
  - Python索引
created: 2026-07-15
status: 🟢 已完成
---

# Python MOC

> [!info] 重点科目 · [[🗺️ 知识地图 MOC]] · [[Home]]

## 📖 概述

**Python** 是一门**简洁、通用、生态强大**的高级语言。它以「可读性」著称——代码接近自然语言，让你专注于解决问题而非与语法搏斗。应用极广：Web 后端（Django/Flask）、数据科学与 AI（NumPy/Pandas/PyTorch）、自动化脚本、运维、爬虫、科研。是初学者入门和专家做复杂系统的共同选择。

> 学 Python 的核心：理解**数据模型（一切皆对象）**、**可变性**、**迭代器/生成器**、**装饰器**。这些打通后，标准库和第三方库都是顺着这些机制构建的，举一反三。

## 📚 学习路径

1. [[数据类型与数据结构]] — 内置类型、可变性、list/dict/set/tuple
2. [[控制流（条件与循环）]] — 顺序、if/elif/else、for/while、推导式
3. [[函数与作用域]] — def、参数传递、LEGB、lambda
4. [[面向对象]] — class、继承、魔术方法、数据类
5. [[装饰器迭代器与生成器]] — decorator、iter、yield
6. [[异常与上下文管理]] — try/except、自定义异常、with
7. [[异步与并发]] — async/await、threading、multiprocessing
8. [[模块包与标准库]] — import 系统、venv、常用标准库

## 🧠 核心速查表

| 需求 | 首选 |
| --- | --- |
| 可变序列 | `list` `[1,2]` |
| 不可变序列 | `tuple` `(1,2)` |
| 键值映射 | `dict` `{'k':'v'}` |
| 去重 / 集合运算 | `set` |
| 字符串拼接 | f-string `f'{x}'` |
| 遍历+索引 | `for i, v in enumerate(xs)` |
| 并行遍历 | `zip(a, b)` |
| 列表推导 | `[x*2 for x in xs if x>0]` |
| 函数装饰 | `@decorator` |
| 上下文管理 | `with open(...) as f:` |
| 包管理 | `venv` + `pip`（或 uv/poetry） |

## 📈 进阶方向

- **Web**：Django（全功能）/ FastAPI（现代异步 API）。
- **数据/AI**：NumPy、Pandas、PyTorch / TensorFlow。
- **性能**：Cython、C 扩展、`asyncio`。
- **工具链**：`uv`/`poetry`（包管理）、`ruff`/`black`（格式化 lint）、`pytest`（测试）。

---

*🔗 返回 [[🗺️ 知识地图 MOC]] · [[Home]]*
