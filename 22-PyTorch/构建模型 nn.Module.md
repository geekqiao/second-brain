---
tags:
  - subject/pytorch
  - model
aliases:
  - nn.Module
  - PyTorch模型
  - 神经网络层
  - forward
created: 2026-07-15
status: 🟢 已完成
---

# 构建模型 nn.Module

> [!info] 所属：[[PyTorch MOC]] · 上一篇 [[自动求导 autograd]] · 下一篇 [[训练循环]]

## 核心概念

PyTorch 里所有网络都继承自 **`nn.Module`**。它提供一套统一的模型组织方式：**在 `__init__` 里声明层，在 `forward` 里定义数据怎么流过这些层**。这种「声明结构 + 定义前向」的方式清晰灵活，是 PyTorch 的灵魂。

### nn.Module 的职责
- **注册子模块/参数**：在 `__init__` 里赋值的层（`nn.Linear` 等）和参数会被自动跟踪，便于 `.parameters()` 遍历、`.to(device)` 迁移、保存加载。
- **定义前向传播**：重写 `forward(self, x)`，描述输入怎么层层变换成输出。
- **不要直接调 `forward`**：用 `model(x)`（即 `__call__`），它会额外处理钩子等逻辑。

### 常用层（nn）
| 层 | 作用 |
| --- | --- |
| `nn.Linear(in, out)` | 全连接层（线性变换） |
| `nn.Conv2d(in, out, k)` | 2D 卷积（图像） |
| `nn.Embedding(num, dim)` | 嵌入层（NLP、离散ID→向量） |
| `nn.ReLU()` / `nn.GELU()` | 激活函数 |
| `nn.Dropout(p)` | 随机失活（正则化） |
| `nn.BatchNorm1d/2d` | 批归一化 |
| `nn.Sequential(...)` | 把多个层顺序串联（简单网络） |

### 激活函数
没有激活函数，多层线性层等价于一层（线性组合还是线性）。激活引入非线性，让网络拟合复杂函数。`ReLU`（`max(0,x)`）最常用。

### 模型参数
- `model.parameters()`：返回所有可学习参数（喂给优化器）。
- `model.to(device)`：把模型和参数迁到 GPU。
- `model.train()` / `model.eval()`：切换训练/评估模式（影响 Dropout、BatchNorm 行为）。

## 代码示例

```python
import torch
import torch.nn as nn

# 1) 自定义模型：经典写法
class MLP(nn.Module):
    def __init__(self, in_dim, hidden, out_dim):
        super().__init__()
        self.net = nn.Sequential(
            nn.Linear(in_dim, hidden),
            nn.ReLU(),
            nn.Dropout(0.2),
            nn.Linear(hidden, hidden),
            nn.ReLU(),
            nn.Linear(hidden, out_dim),
        )

    def forward(self, x):
        return self.net(x)        # 定义数据流

model = MLP(in_dim=784, hidden=128, out_dim=10)
print(model)                      # 打印结构

# 2) 参数与设备
device = 'cuda' if torch.cuda.is_available() else 'cpu'
model = model.to(device)

# 遍历参数（喂给优化器）
for p in model.parameters():
    print(p.shape)

# 3) 前向传播：用 model(x)，不是 model.forward(x)
x = torch.randn(32, 784).to(device)     # 32 个样本，每 784 维
logits = model(x)                        # 前向，输出 [32, 10]
print(logits.shape)

# 4) train / eval 模式切换（重要！）
model.train()    # 训练模式：Dropout 生效、BatchNorm 用 batch 统计
model.eval()     # 评估模式：Dropout 关闭、BatchNorm 用累计统计

# 5) 更灵活的 forward（分支、残差连接）
class ResBlock(nn.Module):
    def __init__(self, dim):
        super().__init__()
        self.fc1 = nn.Linear(dim, dim)
        self.fc2 = nn.Linear(dim, dim)
        self.act = nn.ReLU()

    def forward(self, x):
        h = self.act(self.fc1(x))
        h = self.fc2(h)
        return self.act(x + h)     # 残差：输入 + 变换
```

## ⚠️ 易错点 / 面试要点

- **调 `model(x)` 而非 `model.forward(x)`**：`__call__` 会触发注册的钩子、做额外检查。直接调 `forward` 会绕过这些，行为可能不一致。
- **忘记 `super().__init__()`**：`nn.Module` 子类的 `__init__` 里漏了这行，层不会注册，`parameters()` 为空、训练失效。
- **`model.eval()` 评估时必加**：否则 Dropout 还在随机丢弃、BatchNorm 用 batch 统计（评估不稳定）。配合 `torch.no_grad()`（见 [[自动求导 autograd]]）。
- **数据要迁到与模型相同的 device**：模型 `.to(device)` 后，输入也要 `.to(device)`，否则设备不一致报错。
- **最后一层要不要激活**：分类用 `CrossEntropyLoss` 时**最后一层不要加 softmax**（该损失内含 softmax-log）；回归最后一层不加激活（线性输出）。
- **`nn.Sequential` 适合线性堆叠**：有分支/残差/条件逻辑时，写自定义 `forward` 更清晰。
- **参数初始化**：默认初始化通常够用，但有时需自定义（Xavier/Kaiming）以稳定深层训练。

## 相关链接

- [[PyTorch MOC]] · [[自动求导 autograd]]
- 模型怎么训练：[[训练循环]]
- 喂数据给模型：[[数据加载 Dataset & DataLoader]]
