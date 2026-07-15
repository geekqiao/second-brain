---
tags:
  - subject/javascript
  - dom
aliases:
  - DOM
  - 事件
  - event
  - 事件委托
created: 2026-07-15
status: 🟢 已完成
---

# DOM 与事件

> [!info] 所属：[[JavaScript MOC]] · 上一篇 [[ES6+ 核心特性]] · 下一篇 [[模块化与包管理]]

## 核心概念

**DOM（Document Object Model）** 是浏览器把 HTML 解析成的**对象树**，JS 通过它动态操作页面。**事件（Event）** 是用户或浏览器行为的信号（点击、输入、加载）。二者是「让网页动起来」的基石。现代开发多用框架（[[前端框架概览]]）封装，但底层仍是 DOM + 事件。

### 选取元素
- `document.querySelector('#id / .class / tag')`：取**第一个**匹配（现代首选）。
- `document.querySelectorAll(...)`：取全部（返回 NodeList）。
- `getElementById` / `getElementsByClassName`：旧 API，后者返回动态集合。

### 操作内容与属性
- `.textContent`（纯文本）/ `.innerHTML`（解析 HTML，**有 XSS 风险**）。
- `.classList.add/remove/toggle/contains`（操作 class，首选）。
- `.setAttribute` / `.getAttribute` / `.dataset`（自定义属性 `data-*`）。
- 修改样式：`.style.color = 'red'`（仅内联；批量样式加 class）。

### 增删元素
`document.createElement` → `parent.appendChild` / `insertBefore` → `el.remove()`。
- 性能：频繁操作触发布局重排，可用 `DocumentFragment` 批量插入。

### 事件流三阶段
**捕获（capture，外→内）→ 目标（target）→ 冒泡（bubble，内→外）**。
- 默认在**冒泡**阶段监听（从内向外）。
- `addEventListener(type, handler, useCapture)`，第三参 `true` 改为捕获。

### 事件委托（Event Delegation）
利用**冒泡**：把监听器加在**父元素**上，用 `event.target` 判断实际触发的子元素。好处：动态添加的子元素也能响应、监听器数量少、内存省。

## 代码示例

```js
// 1) 选取与操作
const btn = document.querySelector('.btn');
btn.textContent = '点我';
btn.classList.add('active');
btn.dataset.id = '42';            // HTML: data-id="42"
console.log(btn.dataset.id);      // '42'

// 2) 创建并插入
const li = document.createElement('li');
li.textContent = '新项';
document.querySelector('ul').appendChild(li);

// 3) 事件监听（优先 addEventListener）
btn.addEventListener('click', (e) => {
  e.preventDefault();             // 阻止默认行为（如表单提交/链接跳转）
  console.log('clicked', e.target);
});

// 4) 事件委托：列表项点击，无需给每个 li 单独绑定
document.querySelector('ul').addEventListener('click', (e) => {
  const item = e.target.closest('li');   // 向上找最近的 li
  if (!item) return;
  console.log('点了', item.textContent);
  // 即使后续动态新增的 li 也能响应！
});

// 5) 阻止冒泡 / 捕获
inner.addEventListener('click', (e) => {
  e.stopPropagation();            // 阻止冒泡到父元素
}, false);                         // false=冒泡阶段（默认）

// 6) 常见事件类型
// 鼠标：click / dblclick / mouseenter / mouseleave
// 键盘：keydown / keyup（用 e.key 判断键名）
// 表单：input / change / submit
// 加载：DOMContentLoaded（DOM 就绪）/ load（含资源）

document.addEventListener('DOMContentLoaded', () => {
  console.log('DOM 准备好了，可安全操作元素');
});
```

## ⚠️ 易错点 / 面试要点

- **`innerHTML` 的 XSS 风险**：插入用户输入的内容时，`innerHTML` 会执行其中的 `<script>`/事件属性。用 `textContent` 或框架的自动转义。
- **事件委托的 `closest`**：点击的是 li 内部的 span 时，`e.target` 是 span 不是 li，要用 `e.target.closest('li')` 找到目标容器。
- **冒泡 vs 捕获**：默认冒泡（第三参 false/省略）；需要拦截父级之前先处理用捕获（true）。
- **`preventDefault` vs `stopPropagation`**：前者阻止**默认行为**（跳转/提交），后者阻止**继续传播**（冒泡）。别混。
- **`DOMContentLoaded` vs `load`**：前者 DOM 解析完即可（快，推荐）；后者等图片等资源全加载完（慢）。
- **直接改 `.style` 是内联样式**，优先级高、难维护；批量外观变化应通过增删 class（配合 [[CSS MOC]]）。
- ** NodeList 不是数组**：`querySelectorAll` 返回的 NodeList 可 `forEach` 但没有 `map`，转数组用 `[...nodes]` 或 `Array.from`。

## 相关链接

- [[JavaScript MOC]] · [[ES6+ 核心特性]]
- 操作的对象是 [[HTML MOC]] 的结构
- 配合 CSS：[[CSS MOC]]
- 现代封装：[[前端框架概览]]
