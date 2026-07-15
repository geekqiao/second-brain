---
tags:
  - subject/css
  - moc
aliases:
  - CSS
  - CSS索引
created: 2026-07-15
status: 🟢 已完成
---

# CSS MOC

> [!info] 重点科目 · [[🗺️ 知识地图 MOC]] · [[Home]]

## 📖 概述

**CSS（Cascading Style Sheets，层叠样式表）** 是网页的**表现层**——它决定 [[HTML MOC]] 写好的内容**长什么样**：颜色、字体、间距、布局、动画。CSS 不处理逻辑（那是 [[JavaScript MOC]]），只负责视觉与版式。

> 学 CSS 的两个坎：**布局**（怎么把元素摆到想要的位置）和**层叠机制**（多条规则冲突时谁赢）。攻克这两点，CSS 就从「玄学」变成可控的工具。

## 📚 学习路径

1. [[选择器与优先级]] — 选元素、以及规则冲突时谁说了算
2. [[盒模型]] — 每个元素都是一个盒子，布局的基础
3. [[Flexbox 布局]] — 一维布局王者（行或列）
4. [[Grid 布局]] — 二维布局王者（行+列）
5. [[响应式设计]] — 适配各种屏幕
6. [[动画过渡与变换]] — 让界面动起来
7. [[现代 CSS 特性]] — 变量、:has()、容器查询等新武器

## 🧠 核心速查表

| 想做的事 | 首选方案 |
| --- | --- |
| 居中一个元素 | Flexbox：`display:flex; justify-content:center; align-items:center;` |
| 一行/一列排布 | Flexbox |
| 行列网格页面 | Grid |
| 适配手机/桌面 | Media Query / 视口单位 |
| 改主题色 | CSS 变量（`--brand: #...`）+ `var()` |
| hover/状态过渡 | `transition` |
| 关键帧动画 | `@keyframes` + `animation` |
| 不影响布局的位移 | `transform: translate()` |

## 📈 进阶方向

- **CSS 架构**：BEM / OOCSS / 原子化（Tailwind）等命名与组织方法论。
- **预处理器**：Sass / Less（变量、嵌套、混入）。
- **新特性追踪**：容器查询、`:has()`、`@layer`、视图过渡（View Transitions）。
- 配合 [[JavaScript MOC]] 进入 [[前端框架概览]]。

---

*🔗 返回 [[🗺️ 知识地图 MOC]] · [[Home]]*
