---
tags:
  - subject/html
  - moc
aliases:
  - HTML
  - HTML索引
created: 2026-07-15
status: 🟢 已完成
---

# HTML MOC

> [!info] 重点科目 · [[🗺️ 知识地图 MOC]] · [[Home]]

## 📖 概述

**HTML（HyperText Markup Language）** 是网页的**结构与语义层**。它用一组标签描述「页面上有什么、各是什么角色」。HTML 本身不管长什么样（那是 [[CSS MOC]] 的事）、也不管怎么动（那是 [[JavaScript MOC]] 的事）——它只负责把内容的**结构和含义**表达清楚。

> 写好 HTML 的核心不是记住多少标签，而是**用对的标签表达对的语义**（semantic）。语义化带来 SEO、可访问性、可维护性三重收益。

## 📚 学习路径

1. [[文档结构与语义化]] — 一个 HTML 文档由哪些部分构成，语义化标签如何选用
2. [[文本链接与列表]] — 文本内容、超链接、列表的正确写法
3. [[表单与输入]] — 收集用户输入：input 全家族与原生验证
4. [[多媒体与嵌入]] — 图片、音视频、响应式图片、iframe
5. [[HTML5 新特性]] — 语义元素 / 新输入 / 存储 / canvas / Web APIs
6. [[元数据与 SEO]] — `<head>` 里决定分享与搜索表现的元数据
7. [[可访问性 a11y]] — 让所有人（含残障用户）都能用你的页面

## 🧠 核心速查表

| 需求 | 推荐标签 |
| --- | --- |
| 页面骨架 | `header` / `nav` / `main` / `article` / `section` / `aside` / `footer` |
| 标题层级 | `h1`（每页一个）→ `h2`~`h6` |
| 文本强调 | `strong`（重要）/ `em`（语气强调），少用 `b`/`i` |
| 链接 | `<a href target rel>` |
| 列表 | `ul`+`li`（无序）/ `ol`+`li`（有序）/ `dl`（定义） |
| 图片 | `<img src alt loading="lazy">` |
| 表单输入 | `<input type>` + `<label for>` |
| 容器（无语义时才用） | `div`（块）/ `span`（行内） |

## 📈 进阶方向

- **Web Components**：用自定义元素封装可复用组件。
- **HTML 规范**：读 WHATWG HTML Living Standard 了解最新特性。
- 配合 [[CSS MOC]] 与 [[JavaScript MOC]]，进入前端框架（[[前端框架概览]]）。

---

*🔗 返回 [[🗺️ 知识地图 MOC]] · [[Home]]*
