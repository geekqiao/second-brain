---
tags:
  - subject/css
  - layout
aliases:
  - grid
  - 网格布局
created: 2026-07-15
status: 🟢 已完成
---

# Grid 布局

> [!info] 所属：[[CSS MOC]] · 上一篇 [[Flexbox 布局]] · 下一篇 [[响应式设计]]

## 核心概念

**CSS Grid** 是为**二维布局**（同时控制行和列）设计的系统。如果说 [[Flexbox 布局|Flexbox]] 适合「一条线」上的组件排布，Grid 则适合**整页骨架**——行列同时规划，还能用「画格子」的方式命名区域，所见即所得。

### 核心属性（容器）
| 属性 | 作用 |
| --- | --- |
| `display: grid` | 启用网格 |
| `grid-template-columns` | 定义列（如 `200px 1fr 200px`） |
| `grid-template-rows` | 定义行 |
| `gap` / `row-gap` / `column-gap` | 间距 |
| `grid-template-areas` | 用名字画布局图 |
| `justify-items` / `align-items` | 单元格内对齐 |

### 列宽单位
- `fr`：按比例分配剩余空间（如 `1fr 2fr` = 1:2）。
- `repeat(n, size)`：重复，如 `repeat(3, 1fr)`。
- `minmax(min, max)`：最小最大区间，配合 `auto-fit` 做自适应。
- `auto-fill` / `auto-fit`：自动列数，**无需 media query 即可响应式**。

### 项目（子）属性
- `grid-column: 1 / 3`：跨第 1 到第 3 条网格线（占两列）。
- `grid-row`：跨行。
- `grid-area: 名字`：对应 template-areas 里的命名区域。

### Flex vs Grid 选用
- **一维**（一行或一列内的对齐/分布）→ Flexbox。
- **二维**（行列网格、整页布局、卡片墙）→ Grid。
- 二者可嵌套：Grid 排页面骨架，内部组件用 Flex。

## 代码示例

```css
/* 经典：header / sidebar / main / footer 页面骨架
   用 template-areas 画图，最直观 */
.page {
  display: grid;
  min-height: 100vh;
  gap: 1rem;
  grid-template-columns: 200px 1fr;        /* 两列：侧边栏固定 + 主区自适应 */
  grid-template-rows: auto 1fr auto;       /* 三行：头/中/尾 */
  grid-template-areas:
    "header  header"     /* ← 照着画，每行用引号包，名字对齐即可 */
    "sidebar main"
    "footer  footer";
}
.page header  { grid-area: header; }
.page .sidebar { grid-area: sidebar; }
.page main    { grid-area: main; }
.page footer  { grid-area: footer; }

/* 自适应卡片墙：不需要 media query！
   每张卡最少 250px，自动排满一行再换列 */
.cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1rem;
}

/* 跨列：第一个卡片占两列 */
.cards .featured {
  grid-column: span 2;
}

/* 显式网格线跨区 */
.banner {
  grid-column: 1 / 4;   /* 从第1条线到第4条线，占3列 */
}
```

```html
<div class="page">
  <header>头部</header>
  <nav class="sidebar">侧边</nav>
  <main>主内容</main>
  <footer>页脚</footer>
</div>
```

## ⚠️ 易错点 / 面试要点

- **`auto-fit` vs `auto-fill`**：列数少于填充时，`auto-fit` 会把空轨道**拉伸**撑满（卡片变大）；`auto-fill` 保留空位（卡片不变）。做自适应卡片墙通常要 `auto-fit`。
- **`fr` 是分配剩余空间**：如果有固定列（`200px`），`fr` 只瓜分去掉固定列后的剩余部分，不是总宽的比例。
- **Grid 线从 1 开始数**：`grid-column: 1 / 3` 是「从第1根线到第3根线」，占 2 列，不是占 3 列。
- **template-areas 名字必须对齐成矩形**：一个名字占的区域不能是 L 形，否则布局失效。
- **整页布局首选 Grid**：过去用 float/position 拼页面是噩梦，Grid 一段 CSS 画完。
- **与 Flex 嵌套**：Grid 的某个区域内部如果是一行按钮，内部用 Flex 排更顺手。

## 相关链接

- [[CSS MOC]] · [[Flexbox 布局]]
- 配合 media query 做断点：[[响应式设计]]
