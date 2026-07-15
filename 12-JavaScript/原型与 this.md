---
tags:
  - subject/javascript
  - prototype
aliases:
  - 原型
  - 原型链
  - this绑定
created: 2026-07-15
status: 🟢 已完成
---

# 原型与 this

> [!info] 所属：[[JavaScript MOC]] · 上一篇 [[作用域与闭包]] · 下一篇 [[异步编程]]

## 核心概念

JS 是一门**基于原型（prototype-based）** 的面向对象语言，没有传统的类继承（`class` 只是语法糖）。`this` 则是函数运行时的上下文。这两个机制都「动态」「反直觉」，是 JS 难点所在。

### 原型链（Prototype Chain）
- 每个对象有一个隐藏链接 `[[Prototype]]`（即 `__proto__`），指向它的原型对象。
- 访问属性时，自身找不到就沿 `__proto__` 向上找，直到 `Object.prototype`，再到 `null`——这条链叫**原型链**。
- JS 通过原型链实现「继承」与方法共享。

### `prototype` vs `__proto__`
- `prototype`：**函数**特有的属性，指向「由该函数 new 出来的对象的原型」。
- `__proto__`：**每个对象**都有的属性，指向它的原型（构造函数的 `prototype`）。

### `class` 语法糖
`class` 本质是构造函数 + 原型的语法糖，写起来像传统 OOP，底层仍是原型链。

### `this` 的四条绑定规则（优先级从高到低）
1. **`new` 绑定**：`new Foo()` 中 this 指向新创建的对象。
2. **显式绑定**：`fn.call(obj)` / `apply` / `bind`。
3. **隐式绑定**：`obj.fn()` 中 this 指向 `obj`。
4. **默认绑定**：独立调用，非严格模式指向 `window`（全局），严格模式 `undefined`。
- **箭头函数**：没有自己的 this，继承**定义时**外层的 this（词法 this）。

### `new` 做了什么
1. 创建空对象；2. 链接到构造函数的 prototype；3. 绑定 this 执行构造函数；4. 返回该对象（若构造函数返回对象则用那个）。

## 代码示例

```js
// 1) 原型链：方法共享
function Person(name) {
  this.name = name;
}
Person.prototype.sayHi = function () {           // 挂在原型上，所有实例共享
  return `Hi, I'm ${this.name}`;
};
const p = new Person('Ada');
p.sayHi();                          // "Hi, I'm Ada"
console.log(p.__proto__ === Person.prototype); // true

// 2) class 语法糖（等价于上面）
class PersonC {
  constructor(name) { this.name = name; }
  sayHi() { return `Hi, I'm ${this.name}`; }
}
class Student extends PersonC {                  // extends 实现继承
  constructor(name, grade) { super(name); this.grade = grade; }
  study() { return `${this.name} studying`; }
}

// 3) this 的四种绑定
function show() { console.log(this.label); }
const ctx = { label: 'A' };

show();                 // 默认绑定：window 或 undefined
ctx.m = show; ctx.m();  // 隐式绑定：this = ctx → 'A'
show.call({ label: 'B' }); // 显式绑定：'B'
const bound = show.bind({ label: 'C' });
bound();                // 'C'（永久绑定）

// 4) 箭头函数的词法 this（解耦回调中的 this 丢失）
class Timer {
  constructor() { this.s = 0; }
  start() {
    setInterval(() => { this.s++; }, 1000);  // 箭头函数继承 this
    // 若用普通 function(){ this.s++ } —— this 会是 window/undefined
  }
}

// 5) 原型链查找终点
console.log(Object.prototype.__proto__); // null（链的终点）
```

## ⚠️ 易错点 / 面试要点

- **`this` 是调用时决定的**：同一个函数，谁来调用（调用方式）决定 this，不是定义时决定。这是和「词法作用域」最大的区别。
- **回调里 this 丢失**：把方法当回调传出去（如 `setTimeout(obj.fn, 0)`），this 不再是 obj。用 `bind` 或箭头函数修复。
- **箭头函数没有自己的 this/arguments/super/new.target**：适合当回调和「保持外层 this」，不适合当对象方法或构造函数（不能 `new`）。
- **`prototype` 是函数的，`__proto__` 是对象的**：两者方向相反，别搞混。实际代码用 `Object.getPrototypeOf(obj)` / `Object.create()` 而非直接碰 `__proto__`。
- **`instanceof` 沿原型链判断**：`[] instanceof Array` 为 true，因为查到了 `Array.prototype`。
- **`class` 不提升（TDZ）**、**没有变量提升**，且必须先 `extends` 再 `super()`。
- **继承自有 bug**：经典原型链继承会共享引用类型属性，组合寄生式继承 / 直接用 `class extends` 才稳妥。

## 相关链接

- [[JavaScript MOC]] · [[作用域与闭包]]
- 面向对象对照：[[面向对象]]（Python class）
