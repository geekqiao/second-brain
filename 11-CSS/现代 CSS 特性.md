---
tags:
  - subject/css
  - modern
aliases:
  - 现代 CSS
  - CSS 变量
  - 自定义属性
created: 2026-07-15
status: 🟢 已完成
---

# 现代 CSS 特性

> [!info] 所属：[[CSS MOC]] · 上一篇 [[动画过渡与变换]]

## 核心概念

CSS 近年进化神速，很多过去必须靠预处理器（Sass）或 JS 才能做的事，现在**原生支持**。掌握这些能大幅减少代码与依赖。

### 自定义属性（CSS 变量）
`--name: value;` 定义，`var(--name)` 使用。
- 作用域跟随级联：在 `:root` 定义就是全局。
- **可被 JS 动态修改**，是实现主题切换、动态样式的利器。
- 区别于 Sass 变量：CSS 变量是**运行时**的，Sass 变量是编译时。

### `:has()` 父选择器（关系选择器）
CSS 终于能「根据子元素状态选父元素」。过去要靠 JS `:has()` 一行搞定。

### 容器查询（Container Queries）
让组件**根据父容器宽度**自适应，而非整个视口。组件可移植到任何位置都好看。

### CSS 嵌套（Nesting）
原生支持像 Sass 那样嵌套选择器，减少重复。

### `@layer` 层
显式控制样式层的优先级，终结「靠选择器特异性堆叠」的混乱。

### 现代颜色函数
`color-mix()`（混色）、`oklch()`（感知均匀，调色更自然）、`light-dark()`（明暗模式自适应）。

## 代码示例

```css
/* 1) CSS 变量 + 主题切换 */
:root {
  --brand: #2563eb;
  --bg: #ffffff;
  --text: #1a1a1a;
  --radius: 8px;
}
[data-theme="dark"] {
  --bg: #1a1a1a;
  --text: #eeeeee;
}
body { background: var(--bg); color: var(--text); }
.btn { background: var(--brand); border-radius: var(--radius); }

/* 2) :has() —— 根据子元素状态选父元素 */
/* 「卡片里若有图片，就加内边距」过去要 JS，现在一行 */
.card:has(img) { padding: 1rem; }
/* 表单未填完时整块标红 */
form:has(input:invalid) { outline: 2px solid red; }
/* 选「后面跟着 h3 的 h2」 */
h2:has(+ h3) { margin-bottom: 0; }

/* 3) 容器查询：组件按自身容器宽度变布局 */
.sidebar { container-type: inline-size; }
@container (min-width: 400px) {
  .card { grid-template-columns: 1fr 1fr; } /* 容器够宽就双列 */
}

/* 4) 原生嵌套（现代浏览器支持） */
.card {
  padding: 1rem;
  & .title { font-size: 1.2rem; }
  &:hover { transform: translateY(-2px); }
}

/* 5) @layer 显式分层，控制优先级而非靠特异性 */
@layer base, components, utilities;
@layer base { p { line-height: 1.6; } }
@layer components { .btn { /* 优先级高于 base */ } }
@layer utilities { .text-center { text-align: center; } /* 最高 */ }

/* 6) 现代颜色 */
.mixed { color: color-mix(in oklch, var(--brand), black 20%); } /* 加深20% */
.ok { background: oklch(70% 0.15 145); }            /* 感知均匀的绿 */
```

```js
// JS 动态切换主题：改一个属性即可
document.documentElement.dataset.theme = 'dark';
// 或动态改单个变量
document.documentElement.style.setProperty('--brand', '#e11d48');
```

## ⚠️ 易错点 / 面试要点

- **CSS 变量 vs Sass 变量**：CSS 变量是运行时、可继承、可被 JS 改、能用在 media query 之外各处；Sass 变量编译时定值。现代项目优先用原生变量。
- **`var()` 要给兜底值**：`var(--gap, 1rem)`，防止变量未定义时报错。
- **`:has()` 浏览器支持**：较新特性（2023 起主流支持），给旧浏览器用要注意降级，或作为渐进增强。
- **容器查询需先声明 `container-type`**：不设 `container-type: inline-size`，`@container` 不生效。
- **嵌套别忘了 `&`**：原生嵌套里写 `:hover` 要用 `&:hover`，直接写 `:hover` 语义不同。
- **`@layer` 改变优先级规则**：在 layer 内，普通规则的特异性**被层顺序压制**——后声明的 layer 优先，而非选择器谁更具体。能彻底解决「第三方库样式覆盖」的难题。
- **`oklch` 调色更人性**：调整 lightness 时视觉变化更均匀，比 HSL 更适合做色板。

## 相关链接

- [[CSS MOC]] · [[选择器与优先级]]
- 与 JS 联动改样式：[[DOM 与事件]]
- 框架里的样式方案：[[前端框架概览]]
