---
tags:
  - subject/javascript
  - es6
aliases:
  - ES6
  - ES2015
  - 现代JS语法
created: 2026-07-15
status: 🟢 已完成
---

# ES6+ 核心特性

> [!info] 所属：[[JavaScript MOC]] · 上一篇 [[异步编程]] · 下一篇 [[DOM 与事件]]

## 核心概念

**ES6（ECMAScript 2015）** 是 JS 历史上最大的一次升级，此后每年发布一版（ES2016+）。这些特性让 JS 更简洁、更安全、更易维护。**现代 JS 开发 = 默认使用这些语法**，旧写法（var、拼接字符串、function 表达式）已被淘汰。

### 高频特性清单
| 特性 | 解决的问题 |
| --- | --- |
| `let` / `const` | 块作用域、不可变绑定（见 [[作用域与闭包]]） |
| 箭头函数 `=>` | 简洁 + 词法 this（见 [[原型与 this]]） |
| 模板字面量 `` ` `` | 字符串插值、多行 |
| 解构赋值 | 从对象/数组取值 |
| 展开语法 `...` | 展开/收集、浅拷贝 |
| 默认参数 / 剩余参数 | 灵活的函数签名 |
| `Promise` / `async-await` | 异步（见 [[异步编程]]） |
| `class` | 面向对象语法糖（见 [[原型与 this]]） |
| 模块 `import/export` | 原生模块化（见 [[模块化与包管理]]） |
| `Map` / `Set` | 更强的集合类型 |
| 可选链 `?.` / 空值合并 `??` | 安全访问嵌套属性 |
| `for...of` | 遍历可迭代对象的值 |

## 代码示例

```js
// 1) 模板字面量
const name = 'Ada', lang = 'JS';
console.log(`Hi ${name}, learning ${lang}.`);   // 插值
const html = `
  <div>${name}</div>
`;                                               // 多行

// 2) 解构
const user = { id: 1, name: 'Ada', role: 'admin' };
const { name: who, role = 'guest' } = user;     // 重命名 + 默认值
const [first, ...rest] = [10, 20, 30];           // 数组解构 + 收集

// 3) 展开（浅拷贝 / 合并）
const a = [1, 2], b = [3, 4];
const merged = [...a, ...b];                      // [1,2,3,4]
const obj = { ...user, age: 30 };                // 复制并加字段

// 4) 箭头函数 + 默认参数 + 剩余参数
const greet = (name = 'stranger', ...tags) =>
  `Hi ${name} [${tags.join(',')}]`;

// 5) 可选链 ?.  与  空值合并 ??
const city = user?.address?.city ?? '未知';      // 任一层为 null/undefined 安全停止
// 旧写法：user && user.address && user.address.city

// 6) Map / Set
const m = new Map([['a', 1]]);                   // 键可为任意类型（对象做键也行）
m.set('b', 2);
const s = new Set([1, 1, 2, 2, 3]);              // 自动去重 → {1,2,3}

// 7) for...of（值）vs for...in（键，慎用）
for (const v of [10, 20]) console.log(v);        // 10, 20
for (const k in { a: 1 }) console.log(k);        // 'a'（遍历可枚举属性）

// 8) 数组高阶方法（函数式风格）
const nums = [1, 2, 3, 4];
nums.map(x => x * 2)            // [2,4,6,8]  映射
    .filter(x => x > 4)         // [6,8]      过滤
    .reduce((sum, x) => sum + x, 0); // 14   归约
```

## ⚠️ 易错点 / 面试要点

- **箭头函数不能当构造函数/方法**：无自己的 this/arguments，对象字面量的方法用它可能拿不到实例 this，改用普通方法。
- **`==` vs `===`、`var` vs `let`** 这些旧坑在 ES6+ 项目里应直接消失。
- **解构默认值触发条件是 `undefined`**：值为 `null` 或 `0` 时默认值**不生效**。空值合并 `??` 也只对 null/undefined 生效（与 `||` 区别：`||` 还把 `0`/`''` 当假）。
- **`||` vs `??`**：`0 || '默认'` 得 `'默认'`（0 被当假）；`0 ?? '默认'` 得 `0`。需要保留 `0`/`''` 时用 `??`。
- **展开是浅拷贝**：嵌套对象仍共享引用，深拷贝用 `structuredClone`。
- **`for...in` 会遍历原型链上的可枚举属性**，且顺序不保证数字键顺序；遍历数组用 `for...of` 或 `.forEach`。
- **`Map` vs 对象**：键需要非字符串、需要有序、频繁增删时用 `Map` 性能更好。

## 相关链接

- [[JavaScript MOC]] · [[异步编程]]
- 模块化细节：[[模块化与包管理]]
