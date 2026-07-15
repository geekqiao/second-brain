---
tags:
  - subject/javascript
  - moc
aliases:
  - JavaScript
  - JS
  - JS索引
created: 2026-07-15
status: 🟢 已完成
---

# JavaScript MOC

> [!info] 重点科目 · [[🗺️ 知识地图 MOC]] · [[Home]]

## 📖 概述

**JavaScript（JS）** 是网页的**行为层**——它让 [[HTML MOC]] 的结构和 [[CSS MOC]] 的外观**动起来**：响应用户操作、与服务器通信、动态更新页面。如今 JS 已不限于浏览器：Node.js 让它跑在服务器，Deno/Bun 是新一代运行时。它是前端、全栈开发的核心语言。

> 学 JS 的关键是啃下几个「反直觉」的核心机制：**类型转换、作用域与闭包、原型与 this、异步模型**。这些通了，其余都是语法糖。

## 📚 学习路径

1. [[类型与类型转换]] — 7 种原始类型 + 对象，以及坑最多的隐式转换
2. [[作用域与闭包]] — var/let/const、作用域链、闭包
3. [[原型与 this]] — 原型链继承、this 的四条绑定规则
4. [[异步编程]] — Event Loop、Promise、async/await
5. [[ES6+ 核心特性]] — 解构、展开、箭头函数、类等现代语法
6. [[DOM 与事件]] — 操作页面元素、事件流与委托
7. [[模块化与包管理]] — ES Modules、import/export、npm

## 🧠 核心速查表

| 场景 | 首选 |
| --- | --- |
| 声明变量 | `const`（默认）/ `let`（需重赋值），别用 `var` |
| 判断相等 | `===`（严格），避免 `==`（会转型） |
| 异步 | `async/await`（基于 Promise） |
| 遍历数组 | `map/filter/reduce/forEach` |
| 字符串拼接 | 模板字面量 `` `Hello ${name}` `` |
| 对象/数组复制 | 展开语法 `{...obj}` / `[...arr]` |
| 解耦 this | 箭头函数（继承外层 this） |
| 模块化 | ES Modules（`import/export`） |

## 📈 进阶方向

- **TypeScript**：JS + 静态类型，大型项目标配（见 [[前端框架概览]]）。
- **运行时**：Node.js（服务端）、Deno、Bun。
- **框架**：React / Vue / Svelte（见 [[前端框架概览]]）。
- **深入规范**：读 MDN 与 TC39 提案，理解语言演进。

---

*🔗 返回 [[🗺️ 知识地图 MOC]] · [[Home]]*
