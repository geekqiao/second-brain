---
tags:
  - subject/html
  - html5
aliases:
  - HTML5
  - HTML5 features
created: 2026-07-15
status: 🟢 已完成
---

# HTML5 新特性

> [!info] 所属：[[HTML MOC]] · 上一篇 [[多媒体与嵌入]] · 下一篇 [[元数据与 SEO]]

## 核心概念

HTML5（第 5 版）不只是新标签，更带来一批 **Web API**，让网页具备过去只有原生 App 才有的能力：本地存储、绘图、离线、定位、后台任务等。

### 结构与输入（已在前面详述）
- 语义结构标签（`header`/`nav`/`article`…）见 [[文档结构与语义化]]。
- 新 `input` 类型与原生校验见 [[表单与输入]]。
- 原生 `<video>`/`<audio>` 见 [[多媒体与嵌入]]。

### 本地存储（Web Storage）
- `localStorage`：持久化，关闭浏览器仍在；容量约 5~10MB。
- `sessionStorage`：仅当前标签页会话内有效。
- 二者都只能存**字符串**，对象要 `JSON.stringify`。
- （进阶：`IndexedDB` 存大量结构化数据。）

### 绘图
- `<canvas>`：**位图**，用 JS 逐像素绘制，适合游戏、复杂动画、图表。
- `<svg>`：**矢量**，DOM 节点，缩放不失真，适合图标、简单图形。

### 实用 Web API（了解即可）
- **Geolocation**：获取地理位置（需授权）。
- **Drag and Drop**：原生拖拽。
- **Web Workers**：把耗时计算放后台线程，不卡 UI。
- **History API**：`pushState` 实现 SPA 无刷新路由。
- **Clipboard / Notification / Fullscreen** 等小工具 API。

## 代码示例

```html
<!-- localStorage 存取 -->
<script>
  // 存
  localStorage.setItem('user', JSON.stringify({ name: 'Ada', age: 30 }));
  // 取
  const user = JSON.parse(localStorage.getItem('user'));
  console.log(user.name); // 'Ada'
  // 删
  localStorage.removeItem('user');
</script>

<!-- canvas 画一个矩形 -->
<canvas id="c" width="200" height="100"></canvas>
<script>
  const ctx = document.getElementById('c').getContext('2d');
  ctx.fillStyle = '#4CAF50';
  ctx.fillRect(10, 10, 150, 80);   // 绿色矩形
  ctx.font = '16px sans-serif';
  ctx.fillStyle = '#fff';
  ctx.fillText('Hello Canvas', 30, 55);
</script>

<!-- Geolocation -->
<button onclick="locate()">获取位置</button>
<script>
  function locate() {
    navigator.geolocation.getCurrentPosition(
      pos => console.log(pos.coords.latitude, pos.coords.longitude),
      err => console.warn('被拒绝或失败', err)
    );
  }
</script>

<!-- SVG 内联图标（矢量，可被 CSS 染色） -->
<svg width="24" height="24" viewBox="0 0 24 24" fill="currentColor">
  <circle cx="12" cy="12" r="10"/>
</svg>
```

## ⚠️ 易错点 / 面试要点

- **localStorage vs cookie**：cookie 每次请求自动带上、容量小（4KB）、用于鉴权；localStorage 容量大、不自动发送、用于前端存数据。**别把敏感信息存 localStorage**（XSS 可读）。
- **localStorage 只存字符串**：存对象忘了 `JSON.stringify`，取出来是 `"[object Object]"`。
- **canvas vs svg 选用**：大量动态元素/逐帧动画用 canvas（性能好）；少量可交互矢量图用 svg（每个图元是 DOM，可绑事件）。
- **Web Worker 不能碰 DOM**：它运行在独立线程，只能通过 `postMessage` 与主线程通信。
- **Geolocation 必须 HTTPS**：非安全上下文下 API 不可用。

## 相关链接

- [[HTML MOC]]
- 异步处理存储/网络：[[异步编程]]
- 深入操作 DOM：[[DOM 与事件]]
