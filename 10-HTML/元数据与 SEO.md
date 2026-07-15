---
tags:
  - subject/html
  - meta-seo
aliases:
  - SEO
  - meta tags
  - Open Graph
created: 2026-07-15
status: 🟢 已完成
---

# 元数据与 SEO

> [!info] 所属：[[HTML MOC]] · 上一篇 [[HTML5 新特性]] · 下一篇 [[可访问性 a11y]]

## 核心概念

`<head>` 里不放可见内容，但决定两件大事：**搜索引擎怎么理解你**（SEO）和 **链接被分享时长什么样**（Open Graph / 社交卡片）。这些「看不见」的元数据影响巨大。

### 基础 `<meta>`
- `charset="UTF-8"`：字符编码，必须最早出现。
- `viewport`：控制移动端视口，响应式必备。
- `description`：搜索结果摘要（影响点击率）。
- `keywords`：**已被搜索引擎忽略**，写了也没用（历史遗留）。
- `robots`：控制爬虫索引行为（`index/noindex`, `follow/nofollow`）。

### `<title>`
- 每页唯一、简洁（≤60 字符）、含关键词。浏览器标签、搜索结果、收藏都用它。

### Open Graph（og:）—— 社交分享卡片
源自 Facebook，被微信/微博/Twitter/Slack 等广泛采用，决定链接被分享时的**标题、描述、预览图**。

### Twitter Card
`twitter:card` 等专门为 Twitter/X 优化（多数情况 og: 已够用，可二者都加）。

### canonical
`<link rel="canonical">` 告诉搜索引擎「这页的规范 URL 是哪个」，避免重复内容被分散权重。

### 结构化数据（JSON-LD）
用 JSON 描述页面内容类型（文章、产品、面包屑），有机会在搜索结果获得**富摘要（rich snippet）**。

## 代码示例

```html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <!-- 基础 SEO -->
  <title>HTML 入门教程 | 我的博客</title>
  <meta name="description" content="从零学习 HTML 文档结构与语义化，含可运行示例。">
  <meta name="robots" content="index, follow">
  <link rel="canonical" href="https://example.com/html-intro">

  <!-- Open Graph：社交分享卡片 -->
  <meta property="og:type" content="article">
  <meta property="og:title" content="HTML 入门教程">
  <meta property="og:description" content="从零学习 HTML 文档结构与语义化。">
  <meta property="og:image" content="https://example.com/img/cover.jpg">
  <meta property="og:url" content="https://example.com/html-intro">

  <!-- Twitter Card -->
  <meta name="twitter:card" content="summary_large_image">
  <meta name="twitter:title" content="HTML 入门教程">
  <meta name="twitter:image" content="https://example.com/img/cover.jpg">

  <!-- 结构化数据 JSON-LD -->
  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "Article",
    "headline": "HTML 入门教程",
    "author": { "@type": "Person", "name": "Ada" },
    "datePublished": "2026-07-15"
  }
  </script>
</head>
```

## ⚠️ 易错点 / 面试要点

- **`description` 影响点击率而非排名**：但它决定搜索结果里显示的那行字，写得好能显著提升点击。
- **`og:image` 要绝对 URL**：很多平台不解析相对路径，图片需可公开访问且尺寸达标（推荐 1200×630）。
- **`title` 别堆关键词**：「HTML | CSS | JS | 教程 | 免费」这种堆砌会被惩罚。
- **canonical 解决重复内容**：同一内容多个 URL（带参数、移动版）会分散权重，用 canonical 指向规范版。
- **`keywords` meta 已死**：别花时间写，Google 早在 2009 年就不看它。
- **SPA 的 SEO 难题**：纯客户端渲染的内容爬虫可能看不到，需 SSR / 预渲染（见 [[前端框架概览]]）。

## 相关链接

- [[HTML MOC]] · [[文档结构与语义化]]
- 移动端视口与响应式：[[响应式设计]]
