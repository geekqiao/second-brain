---
tags:
  - subject/pytorch
  - autograd
aliases:
  - autograd
  - 自动求导
  - backward
  - 计算图
created: 2026-07-15
status: 🟢 已完成
---

# 自动求导 autograd

> [!info] 所属：[[PyTorch MOC]] · 上一篇 [[张量基础]] · 下一篇 [[构建模型 nn.Module]]

## 核心概念

**autograd** 是 PyTorch 的自动微分引擎——你只要用张量写出**前向计算**（从输入到 loss），它就自动帮你算出**每个参数的梯度**。这是训练神经网络的基石：有了梯度，优化器才知道往哪个方向调参数。

### 计算图（Computational Graph）
- PyTorch 用**动态图**（define-by-run）：每次前向传播实时构建计算图，记录每个运算。
- 图的**叶子节点**是输入/参数，**根节点**是 loss（标量）。
- `backward()` 从 loss 反向遍历图，用链式法则算出每个节点的梯度。

### `requires_grad`
- 张量设 `requires_grad=True`，表示「需要对这个张量求梯度」，autograd 会跟踪涉及它的运算。
- 模型参数（`nn.Parameter`）默认 `requires_grad=True`。
- 不需要梯度的场景（评估、推理）应关闭，省内存算力。

### `backward()` 与 `.grad`
- `loss.backward()`：反向传播，把梯度累加到各张量的 `.grad` 属性。
- 梯度是**累加**的——所以每个训练步要先 `optimizer.zero_grad()` 清零（见 [[训练循环]]）。
- `backward()` 只能对一个**标量**（如 loss）调用。

### 关闭梯度追踪
- `with torch.no_grad():`：块内运算不建图、不求导。**评估/推理必用**，省内存、加速。
- `.detach()`：把张量从计算图剥离，变成普通张量（不再连到梯度）。

### 梯度相关方法
- `x.grad`：读取梯度。
- `x.grad_fn`：记录创建该张量的函数（建图的依据）。

## 代码示例

```python
import torch

# 1) 手动体验 autograd：y = x^2，dy/dx = 2x
x = torch.tensor(3.0, requires_grad=True)
y = x ** 2            # 前向：记录运算建图
y.backward()          # 反向：自动求导
print(x.grad)         # tensor(6.)  ← 正好 2x=6

# 2) 神经网络式：线性 y = w*x + b，算 w/b 的梯度
w = torch.tensor(1.0, requires_grad=True)
b = torch.tensor(0.0, requires_grad=True)
x = torch.tensor(2.0)
y_target = torch.tensor(5.0)

y = w * x + b
loss = (y - y_target) ** 2     # 前向到 loss
loss.backward()                # 反向
print("w.grad =", w.grad)      # 梯度告诉你 w 该往哪调
print("b.grad =", b.grad)

# 3) 梯度累加：不清零会叠加（训练常见坑）
w = torch.tensor(1.0, requires_grad=True)
for i in range(3):
    y = (w * 2).sum()
    y.backward()
print(w.grad)                  # tensor(6.) ← 三次累加，不是 2！

# 4) 关闭梯度：评估/推理
model_w = torch.tensor(2.0, requires_grad=True)
with torch.no_grad():          # 块内不建图
    out = model_w * 100
print(out.requires_grad)       # False（省内存、加速）

# 5) detach：剥离计算图（如记录指标、画图）
x = torch.tensor(1.0, requires_grad=True)
y = x * 5
y_detached = y.detach()        # 不再连梯度，可安全转 numpy/记录
```

## ⚠️ 易错点 / 面试要点

- **梯度是累加的，不清零会叠加**：训练循环每步必须 `optimizer.zero_grad()` 再 `backward()`，否则梯度累加导致训练异常。这是最常见的训练 bug。
- **`backward()` 默认释放计算图**：调用后图被释放，不能对同一图再 `backward()`。需要多次（如 RNN 截断）用 `backward(retain_graph=True)`。
- **评估/推理要 `torch.no_grad()`**：不关梯度会白白建图，浪费内存算力，甚至影响结果。`model.eval()` + `no_grad` 是推理标配。
- **`.grad` 在 `backward()` 后才有值**：前向传播后 `.grad` 是 None，必须先 `backward()`。
- **只对叶子节点保留梯度**：中间结果的梯度默认反向后被释放（节省内存），只留 `requires_grad=True` 叶子的 `.grad`。要看中间梯度用 `retain_grad()`。
- **detached 张量不传梯度**：`.detach()` 切断反传，常用于「冻结」某部分或把数据移出图（转 numpy、记录 loss 值）。
- **梯度爆炸/消失**：深层网络梯度可能极大（NaN）或极小（学不动）。用梯度裁剪 `torch.nn.utils.clip_grad_norm_`、合适初始化、BatchNorm 缓解。

## 相关链接

- [[PyTorch MOC]] · [[张量基础]]
- 梯度怎么用：[[训练循环]]（optimizer.step）
- 模型参数都是 requires_grad 的张量：[[构建模型 nn.Module]]
