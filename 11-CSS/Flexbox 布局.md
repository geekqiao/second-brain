---
tags:
  - subject/css
  - layout
aliases:
  - flex
  - 弹性布局
  - flexbox
created: 2026-07-15
status: 🟢 已完成
---

# Flexbox 布局

> [!info] 所属：[[CSS MOC]] · 上一篇 [[盒模型]] · 下一篇 [[Grid 布局]]

## 核心概念

**Flexbox**（弹性盒）是为**一维布局**（一行或一列）设计的现代布局系统。它让对齐、分布、自适应间距这些过去要用 hack（float/position）的难题变得声明式。绝大多数组件级布局用 Flexbox 就够。

### 容器（父）属性
| 属性 | 作用 | 常用值 |
| --- | --- | --- |
| `display: flex` | 启用 flex | / |
| `flex-direction` | 主轴方向 | `row`(默认) / `column` / `row-reverse` |
| `justify-content` | **主轴**对齐 | `flex-start` / `center` / `space-between` / `space-around` / `space-evenly` |
| `align-items` | **交叉轴**对齐 | `stretch`(默认) / `center` / `flex-start` / `flex-end` |
| `flex-wrap` | 是否换行 | `nowrap`(默认) / `wrap` |
| `gap` | 项之间间距 | `8px` / `1rem` |

### 项目（子）属性
| 属性 | 作用 |
| --- | --- |
| `flex: 1` | 占满剩余空间（缩写：grow/shrink/basis） |
| `flex-grow` | 放大比例（0 不放大） |
| `flex-shrink` | 缩小比例 |
| `flex-basis` | 初始尺寸 |
| `order` | 改变排列顺序（不改 DOM） |
| `align-self` | 单独覆盖容器的 align-items |

### 主轴 vs 交叉轴
- **主轴** = `flex-direction` 指向的方向；用 `justify-content` 控制。
- **交叉轴** = 与主轴垂直的方向；用 `align-items` 控制。
- 一句话口诀：**「主轴 justify，交叉 align」**。

## 代码示例

```css
/* 经典：水平垂直居中（最小代码） */
.center {
  display: flex;
  justify-content: center;   /* 主轴居中 */
  align-items: center;       /* 交叉轴居中 */
  height: 100vh;
}

/* 导航栏：logo 在左，链接在右 */
.nav {
  display: flex;
  justify-content: space-between; /* 两端对齐 */
  align-items: center;
  gap: 1rem;
}
.nav .links {
  display: flex;
  gap: 1rem;                   /* 链接间距 */
}

/* 三列等宽卡片 + 自动换行 + 间距 */
.cards {
  display: flex;
  flex-wrap: wrap;             /* 放不下就换行 */
  gap: 1rem;
}
.cards .item {
  flex: 1 1 250px;             /* grow shrink basis：基础250px，可伸缩 */
}

/* 圣杯布局：头部/尾部固定，中间三栏 */
.layout {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}
.layout main {
  display: flex;
  flex: 1;                     /* 占满中间高度 */
}
.layout .sidebar { flex: 0 0 200px; } /* 固定宽 */
.layout .content { flex: 1; }         /* 自适应 */
```

```html
<nav class="nav">
  <div class="logo">Logo</div>
  <div class="links">
    <a href="#">首页</a>
    <a href="#">关于</a>
  </div>
</nav>
```

## ⚠️ 易错点 / 面试要点

- **`flex: 1` 是缩写**：等价于 `flex: 1 1 0%`（可放大、可缩小、basis 为 0）。而 `flex: 1 1 auto` 行为不同（basis 取内容尺寸），细节别混。
- **主轴方向决定谁是 justify**：`flex-direction: column` 时主轴变成垂直，`justify-content` 就控制垂直对齐——很多人在这里转不过弯。
- **`gap` 比用 margin 简洁**：旧做法给每个 item 加 `margin-right` 再处理最后一个，`gap` 一次搞定，且换行时也对。
- **Flex 不发生 margin 折叠**：Flex 容器直接子元素的垂直 margin 会相加，不折叠。
- **`flex-shrink: 0`**：不想被压缩的元素（如图标、固定栏）设它，防止挤变形。
- **居中别再用 `margin: 0 auto` + 负 margin 那套老 hack**，Flex 一行搞定。
- **整页布局考虑 Grid**：一维用 Flex，二维（行列网格）用 [[Grid 布局]]。

## 相关链接

- [[CSS MOC]] · [[盒模型]]
- 二维布局：[[Grid 布局]]
- 自适应屏幕：[[响应式设计]]
